Swift

1.Alamofire

OC

1.AFNetworking

2.SDWebImage

https://github.com/rs/SDWebImage

3.SVProgressHUD

https://github.com/SVProgressHUD/SVProgressHUD

原理: 我猜是加在window上，这样在vc关闭时也可以显示

4.MBProgressHUD

https://github.com/jdg/MBProgressHUD

5.MJRefresh

https://github.com/CoderMJLee/MJRefresh

6.TPKeyboardAvoiding

https://github.com/michaeltyson/TPKeyboardAvoiding

键盘叫回去的两种方法

1.// 谁叫出的键盘, 那么谁就是"第一响应者", 让"第一响应者"辞职, 就可以把键盘叫回去

 \[self.txtfield2 resignFirstResponder\];

2.// self.view就表示是当前控制器所管理的那个view（每一个控制器都会管理一个view）

    // 这时把键盘叫回去的思路就是：让控制器所管理的view停止编辑，这样的话, 凡是这个view中的子控件叫出的键盘就都回去了。

   \[self.view endEditing:YES\];

7.IQKeyboardManager

https://github.com/hackiftekhar/IQKeyboardManager

8.FDFullscreenPopGesture  滑动返回手势

https://github.com/forkingdog/FDFullscreenPopGesture
