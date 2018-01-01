NSOperation的作用

配合使用NSOperation和NSOperationQueue也能实现多线程编程



NSOperation和NSOperationQueue实现多线程的具体步骤

先将需要执行的操作封装到一个NSOperation对象中

然后将NSOperation对象添加到NSOperationQueue中

系统会自动将NSOperationQueue中的NSOperation取出来

将取出的NSOperation封装的操作放到一条新线程中执行



NSOperation的子类

NSOperation是个抽象类，并不具备封装操作的能力，必须使用它的子类

使用NSOperation子类的方式有3种

NSInvocationOperation

NSBlockOperation

自定义子类继承NSOperation，实现内部相应的方法



NSInvocationOperation

NSOperationQueue \* queue = \[\[NSOperationQueue alloc\]init\];

\[queue setMaxConcurrentOperationCount:5\];//最大并发数 

    for \(int i=0; i&lt;10; i++\) {

        NSInvocationOperation \* op = \[\[NSInvocationOperation alloc\]initWithTarget:self selector:@selector\(downloadImage:\) object:@\(i\)\];

        \[queue addOperation:op\];

    }

默认情况下，调用了start方法后并不会开一条新线程去执行操作，而是在当前线程同步执行操作

只有将NSOperation放到一个NSOperationQueue中，才会异步执行操作



NSBlockOperation

NSOperationQueue \* queue = \[\[NSOperationQueue alloc\]init\];

    for \(int i=0; i&lt;10; i++\) {

        NSBlockOperation \* op = \[NSBlockOperation blockOperationWithBlock:^{

            NSLog\(@"%@ %@", \[NSThread currentThread\], @\(i\)\);

        }\];

        \[queue addOperation:op\];

    }

与 NSInvocationOperation 的一样

 不同点：使用 block 来定义操作，所有的代码写在一起，更简单，便于维护！



NSOperationQueue

NSOperationQueue的作用

NSOperation可以调用start方法来执行任务，但默认是同步执行的

如果将NSOperation添加到NSOperationQueue（操作队列）中，系统会自动异步执行NSOperation中的操作



添加操作到NSOperationQueue中

- \(void\)addOperation:\(NSOperation \*\)op;

- \(void\)addOperationWithBlock:\(void \(^\)\(void\)\)block;



队列的取消、暂停、恢复

取消队列的所有操作

- \(void\)cancelAllOperations;

提示：也可以调用NSOperation的- \(void\)cancel方法取消单个操作

暂停和恢复队列

- \(void\)setSuspended:\(BOOL\)b; // YES代表暂停队列，NO代表恢复队列

- \(BOOL\)isSuspended;



操作依赖

NSOperation之间可以设置依赖来保证执行顺序

比如一定要让操作A执行完后，才能执行操作B，可以这么写

\[operationB addDependency:operationA\]; // 操作B依赖于操作A

可以在不同queue的NSOperation之间创建依赖关系



自定义NSOperation

自定义NSOperation的步骤很简单

重写- \(void\)main方法，在里面实现想执行的任务



重写- \(void\)main方法的注意点

自己创建自动释放池（因为如果是异步操作，无法访问主线程的自动释放池）

经常通过- \(BOOL\)isCancelled方法检测操作是否被取消，对取消做出响应



