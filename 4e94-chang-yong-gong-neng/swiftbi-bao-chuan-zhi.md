CustomView类中代码



class CustomView: UIView {  

  

  //声明一个属性btnClickBlock，type为闭包可选类型  

  //闭包类型：\(\)-&gt;\(\) ，无参数，无返回值  

  var btnClickBlock:\(\(\)-&gt;\(\)\)?;  

  

  //重写 init\(frame: CGRect\)构造函数  

  override init\(frame: CGRect\) {  

    super.init\(frame:frame\);  

    //创建按钮  

    let btn = UIButton\(frame: CGRect\(x: 15, y: 15, width: 80, height: 32\)\);  

    btn.setTitle\("按钮", for: .normal\);  

    btn.backgroundColor = UIColor.blue;  

    //绑定事件  

    btn.addTarget\(self, action: \#selector\(CustomView.btnClick\), for: .touchDown\);  

    //添加  

    addSubview\(btn\);  

  

  }  

  //按钮点击事件函数  

  func btnClick\(\){  

  

    if self.btnClickBlock != nil {  

      //点击按钮执行闭包  

      //注意：属性btnClickBlock是可选类型，需要先解包  

      self.btnClickBlock!\(\);  

    }  

  }  

  

  required init?\(coder aDecoder: NSCoder\) {  

    fatalError\("init\(coder:\) has not been implemented"\)  

  }  

  

}  



Controller类中代码：

class ViewController: UIViewController {  

  

  override func viewDidLoad\(\) {  

    super.viewDidLoad\(\)  

  

    //创建CustomView对象  

    let cutomeView = CustomView\(frame: CGRect\(x: 50, y: 50, width: 200, height: 200\)\);  

    //给cutomeView的btnClickBlock闭包属性赋值  

    cutomeView.btnClickBlock = {  

      // \(\) in 无参数可以省略  

      //当按钮被点击时会执行此代码块  

      print\("按钮被点击"\);  

    }  

    cutomeView.backgroundColor = UIColor.yellow;  

    //添加到控制器view上  

    self.view.addSubview\(cutomeView\);  

  

  }  

}  

