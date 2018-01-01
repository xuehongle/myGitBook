1.UITableView在ios6之后，加入了UIRefreshControl，用来下拉刷新

只能下拉刷新，不能上拉加载；无法自定义样式

2.原理

在tableview后面加一个view, tableView.insertSubView\(refreshView, atIndex: 0\)。

根据tableview滑动的距离（-contentOffset.y - contentInset.top）（scrollviewDidScroll代理）来执行动画。

视差滚动

松手后，刷新视图停住一会，锁定再刷新（scrollviewWillEndDragging代理）在代理方法里检查是否拉动了足够的长度，来决定是否进行刷新操作。

锁住可根据contentInset.top增加一个固定值来搞定

https://github.com/FangYiXiong/SwiftPullToRefresh\_Part3\_Finished

3.MJRefresh等第三方控件

