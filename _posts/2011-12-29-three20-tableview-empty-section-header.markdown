---
author: jimneylee
layout: post
title: "解决three20在sdk5.0编译时tableView出现Empty section header"
date: 2011-12-29 14:43
comments: true
tags:
- three20
- iphone
---
如题，three20的tableview在最新的sdk5.0中，即使未设置section的title，tableview头部仍然会显示一个空的sectionview，在网上瞎搜索几天，今天终于找到解决方法，e文不好，贴在这里也给其他遇到同样问题的朋友参考下。
参考git上320讨论组：
[https://github.com/facebook/three20/issues/694](https://github.com/facebook/three20/issues/694)
解决方法，简译一下：
为`TTTableViewDelegate`方法创建一个`category`（类别），实现
`- (CGFloat)tableView:(UITableView *)tableView 
heightForHeaderInSection:(NSInteger)section`
获取section头高度的代理方法
`TTTableViewDelegate+EmptySectionHeader.m`如下：

{% highlight objc %}
#import "TTTableViewDelegate+EmptySectionHeader.h"
 
@implementation TTTableViewDelegate (EmptySectionHeader)
- (CGFloat)tableView:(UITableView *)tableView heightForHeaderInSection:(NSInteger)section
{
    id<TTTableViewDataSource> dataSource = (id<TTTableViewDataSource>)tableView.dataSource;
    if ([dataSource isKindOfClass:[TTListDataSource class]])
        return 0.;
    return 20.;
}
@end
{% endhighlight %}

---
update 2011-12-29:
原来的方法使用比较受限制，对于自定义的`datesource`无法处理，而且比较麻烦。但是我们可以修改一下这个代理方法的实现过程，参考320中`TTTableViewGroupedVarHeightDelegate`中这个代理方法的实现，修改如下：

{% highlight objc %}
#import "TTTableViewDelegate+EmptySectionHeader.h"
static const CGFloat kEmptyHeaderHeight = 0;
static const CGFloat kSectionHeaderHeight = 18;
 
static const CGFloat kGroupEmptyHeaderHeight = 5;
static const CGFloat kGroupSectionHeaderHeight = 35;
 
@implementation TTTableViewDelegate (EmptySectionHeader)
- (CGFloat)tableView:(UITableView *)tableView heightForHeaderInSection:(NSInteger)section
{
    if (tableView.style == UITableViewStylePlain) {
        if ([tableView.dataSource respondsToSelector:@selector(tableView:titleForHeaderInSection:)]) {
            NSString* title = [tableView.dataSource tableView:tableView titleForHeaderInSection:section];
            if (!title.length) {
                return kEmptyHeaderHeight;
            } else {
                return kSectionHeaderHeight;
            }
        }
        else {
            return kEmptyHeaderHeight;
        }
    }
    else {
        if ([tableView.dataSource respondsToSelector:@selector(tableView:titleForHeaderInSection:)]) {
            NSString* title = [tableView.dataSource tableView:tableView titleForHeaderInSection:section];
            if (!title.length) {
                return kGroupEmptyHeaderHeight;
            }
            else {
                return kGroupSectionHeaderHeight;
            }
        }
        else {
            return kGroupEmptyHeaderHeight;
        }
    }
}
@end
{% endhighlight %}