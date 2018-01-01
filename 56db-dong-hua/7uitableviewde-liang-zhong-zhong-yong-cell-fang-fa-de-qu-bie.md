转载自:http://eric-gao.iteye.com/blog/2234047

UITableView中有两种重用Cell的方法：

Ios代码

  1. - \(id\)dequeueReusableCellWithIdentifier:\(NSString \*\)identifier;  

  2. - \(id\)dequeueReusableCellWithIdentifier:\(NSString \*\)identifier forIndexPath:\(NSIndexPath \*\)indexPath NS\_AVAILABLE\_IOS\(6\_0\);  

在iOS 6中dequeueReusableCellWithIdentifier:被dequeueReusableCellWithIdentifier:forIndexPath:所取代。如此一来，在表格视图中创建并添加UITableViewCell对象会变得更为精简而流畅。而且使用dequeueReusableCellWithIdentifier:forIndexPath:一定会返回cell，系统在默认没有cell可复用的时候会自动创建一个新的cell出来。

使用dequeueReusableCellWithIdentifier:forIndexPath:的话，必须和下面的两个配套方法配合起来使用：

Ios代码

  1. // Beginning in iOS 6, clients can register a nib or class for each cell.  

  2. // If all reuse identifiers are registered, use the newer -dequeueReusableCellWithIdentifier:forIndexPath: to guarantee that a cell instance is returned.  

  3. // Instances returned from the new dequeue method will also be properly sized when they are returned.  

  4. - \(void\)registerNib:\(UINib \*\)nib forCellReuseIdentifier:\(NSString \*\)identifier NS\_AVAILABLE\_IOS\(5\_0\);  

  5. - \(void\)registerClass:\(Class\)cellClass forCellReuseIdentifier:\(NSString \*\)identifier NS\_AVAILABLE\_IOS\(6\_0\);  

1、如果是用NIB自定义了一个Cell，那么就调用registerNib:forCellReuseIdentifier:

2、如果是用代码自定义了一个Cell，那么就调用registerClass:forCellReuseIdentifier:

以上这两个方法可以在创建UITableView的时候进行调用。

这样在tableView:cellForRowAtIndexPath:方法中就可以省掉下面这些代码：

Ios代码

  1. static NSString \*CellIdentifier = @"Cell";  

  2. if \(cell == nil\)   

  3.     cell = \[\[UITableViewCell alloc\] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:CellIdentifier\];  

取而代之的是下面这句代码：

Ios代码

  1. UITableViewCell \*cell = \[tableView dequeueReusableCellWithIdentifier:@"Cell" forIndexPath:indexPath\];  

一、使用NIB

1、xib中指定cell的Class为自定义cell的类型（不是设置File's Owner的Class）

2、调用registerNib:forCellReuseIdentifier:向数据源注册cell

Ios代码

  1. \[\_tableView registerNib:\[UINib nibWithNibName:@"CustomCell" bundle:nil\] forCellReuseIdentifier:kCellIdentify\];   

3、在tableView:cellForRowAtIndexPath:中使用dequeueReusableCellWithIdentifier:forIndexPath:获取重用的cell，如果没有重用的cell，将自动使用提供的nib文件创建cell并返回（如果使用dequeueReusableCellWithIdentifier:需要判断返回的是否为空）

Ios代码

  1. CustomCell \*cell = \[\_tableView dequeueReusableCellWithIdentifier:kCellIdentify forIndexPath:indexPath\];  

4、获取cell时如果没有可重用cell，将创建新的cell并调用其中的awakeFromNib方法

二、不使用NIB

1、重写自定义cell的initWithStyle:withReuseableCellIdentifier:方法进行布局

2、注册cell

Ios代码

  1. \[\_tableView registerClass:\[CustomCell class\] forCellReuseIdentifier:kCellIdentify\];   

3、在tableView:cellForRowAtIndexPath:中使用dequeueReusableCellWithIdentifier:forIndexPath:获取重用的cell，如果没有重用的cell，将自动使用提供的class类创建cell并返回

Ios代码

  1. CustomCell \*cell = \[tableView dequeueReusableCellWithIdentifier:kCellIdentify forIndexPath:indexPath\];   

4、获取cell时如果没有可重用的cell，将调用cell中的initWithStyle:withReuseableCellIdentifier:方法创建新的cell

