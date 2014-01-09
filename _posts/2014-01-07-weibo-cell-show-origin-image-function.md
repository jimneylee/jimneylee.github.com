---
author: jimneylee
layout: post
title: "实现微博图片查看原图功能"
date: 2014-01-07 10:37
comments: true
tags:
- iphone
- weibo
---

今天在我的[开源新浪微博客户端](https://github.com/jimneylee/SinaMBlogNimbus)上加上这个功能。之前预想实现比较复杂，今天详细考虑下，实现还是比较简单，核心代码是10来行即可搞定。

![image](http://www.cocoachina.com/bbs/attachment/Fid_6/6_22435_2fbef273f26a4b5.gif)

###实现思路剖析

####1、获取视图动画的初始frame
这个功能实现的复杂性在于，微博Cell本身视图内容就多层嵌套，包含当前微博和被转发微博，同时Cell又是位于tableView上，视图显示的UI比一般的简单布局要复杂多。
但是我们通过将图片视图，从当前父视图，往上层父视图转换，一直到window级别，这样我们就准确获取到动画放大显示前的原位置，对应于fromRect。

    - (void)showContentOriginImage
    {
        if (self.viewController) {
            if ([self.viewController isKindOfClass:[UITableViewController class]]) {
                UITableView* tableView = ((UITableViewController*)self.viewController).tableView;
                UIWindow* window = [UIApplication sharedApplication].keyWindow;
                
                // convert rect to self(cell)
                CGRect rectInCell = [self.contentView convertRect:self.contentImageView.frame toView:self];
        
                // convert rect to tableview
                CGRect rectInTableView = [self convertRect:rectInCell toView:tableView];//self.superview
                
                // convert rect to window
                CGRect rectInWindow = [tableView convertRect:rectInTableView toView:window];
                
                // show photo full screen
                UIImage* image = self.contentImageView.image;
                if (image) {
                    rectInWindow = CGRectMake(rectInWindow.origin.x + (rectInWindow.size.width - image.size.width) / 2.f,
                                              rectInWindow.origin.y + (rectInWindow.size.height - image.size.height) / 2.f,
                                              image.size.width, image.size.height);
                }
                SMFullScreenPhotoBrowseView* browseView =
                [[SMFullScreenPhotoBrowseView alloc] initWithUrlPath:self.statusEntity.original_pic
                                                           thumbnail:self.contentImageView.image
                                                            fromRect:rectInWindow];
                [window addSubview:browseView];
            }
        }
    }

####2、原图下载进度显示
通过实现`MBRoundProgressView`的`MBRoundProgressViewDelegate`方法，计算当前图片的下载进度progress，进度视图采用`MBRoundProgressView`，实现比较容易

    - (void)networkImageView:(NINetworkImageView *)imageView readBytes:(long long)readBytes totalBytes:(long long)totalBytes
    {
        CGFloat progress = (float)readBytes / (float)totalBytes;
        self.progressIndicator.progress = progress;
    }
    
####3、图片缩放功能
难点在于缩放的同时，调整imageView的center始终位于中间位置，主要考虑imageView高度小于屏幕高度后center调整，这边实现我参考SO的解答：http://stackoverflow.com/questions/1316451/center-content-of-uiscrollview-when-smaller
以前这个问题困扰我很久，今天终于得到解决，挺高兴的。

    -(void)scrollViewDidZoom:(UIScrollView *)scrollView {
        CGFloat offsetX = (scrollView.bounds.size.width > scrollView.contentSize.width)?
        (scrollView.bounds.size.width - scrollView.contentSize.width) * 0.5f : 0.f;
        
        CGFloat offsetY = (scrollView.bounds.size.height > scrollView.contentSize.height)?
        (scrollView.bounds.size.height - scrollView.contentSize.height) * 0.5f : 0.f;
        
        self.imageView.center = CGPointMake(scrollView.contentSize.width * 0.5f + offsetX,
                                     scrollView.contentSize.height * 0.5f + offsetY);
    }

####4、图片保存
保存功能实现比较简单，偷懒情况下一句`UIImageWriteToSavedPhotosAlbum`即可搞定，但是对于稳定性比较高的APP还是需要严格的书写和良好的提示

    - (void)savePhoto
    {
    	if (self.isOriginPhotoLoaded && self.imageView.image) {
            __block MBProgressHUD* hud = [MBProgressHUD showHUDAddedTo:self animated:YES];
            hud.labelText = @"保存中...";
            self.saveBtn.enabled = NO;
            UIImageWriteToSavedPhotosAlbum(self.imageView.image, self,
                                           @selector(image:didFinishSavingWithError:contextInfo:), nil);
    	}
        else {
            [SMGlobalConfig showHUDMessage:@"图片未下载完成，无法保存" addedToView:self];
        }
    }

    //pragma mark - SavedPhotosAlbum CallBack
    -(void) image:(UIImage *)image didFinishSavingWithError:(NSError *)error contextInfo:(void *)contextInfo
    {
        if (error) {
            [SMGlobalConfig showHUDMessage:@"保存失败" addedToView:self];
        }
        else {
            [MBProgressHUD hideHUDForView:self animated:YES];
            
            __block MBProgressHUD* hud = [MBProgressHUD showHUDAddedTo:self animated:YES];
            hud.customView = [[UIImageView alloc] initWithImage:[UIImage nimbusImageNamed:@"37x-Checkmark.png"]];
            hud.mode = MBProgressHUDModeCustomView;
            hud.labelText = @"保存成功";
            [hud hide:YES afterDelay:1.5f];
            
            self.saveBtn.enabled = YES;
        }
    }

今天把之前一直困扰我的功能实现，现在感觉心情大好，希望给路过没有错过的同学一点帮助和提示，如果有什么问题，欢迎在github项目的issue，提出你的见解和想法。
