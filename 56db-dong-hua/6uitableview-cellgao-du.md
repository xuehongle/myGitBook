1.方法1：修改rowheight属性

self.tableView.rowHeight = 50;

缺点：高度固定



2.方法2：实现heightForRowAtIndexPath:代理方法

- \(CGFloat\)tableView:\(UITableView \*\)tableView heightForRowAtIndexPath:\(NSIndexPath \*\)indexPath {

    return 100;

}

缺点：高度固定



3.self-sizing

ios8及以后推出的

注意不要xib里最外层写死高度

self.tableView.estimatedRowHeight = 100; // 预计cell高度

self.tableView.rowHeight = UITableViewAutomaticDimension; //自动计算高度cell



4.代码动态调整

根据string的多少，字号，宽度，可算出高度

