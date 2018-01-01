UIView动画

```
- (void) frameClick {
//    Block动画
    [UIView animateWithDuration:2 delay:0 options:UIViewAnimationOptionCurveEaseInOut animations:^{
        _imageView.frame = CGRectMake(100, 200, 100, 100);
    } completion:^(BOOL finished) {
        NSLog(@"移动结束!");
    }];
}

- (void) scaleClick {
    [UIView beginAnimations:nil context:nil];
    [UIView setAnimationDuration:2];
    [UIView setAnimationDelay:0];
    [UIView setAnimationCurve:UIViewAnimationCurveEaseInOut];
    [UIView setAnimationDelegate:self];
    [UIView setAnimationDidStopSelector:@selector(finish)]; //设置代理后，才能用
    _imageView.frame = CGRectMake(_imageView.frame.origin.x, _imageView.frame.origin.y, 200, 200);
    [UIView commitAnimations];
    
}
```

UIImage帧动画

```
// 创建一个NSArray集合，其中集合元素都是UIImage对象
    images = [NSArray arrayWithObjects:
              [UIImage imageNamed:@"guide01.jpg"],
              [UIImage imageNamed:@"guide02.jpg"],
              [UIImage imageNamed:@"guide03.jpg"], nil];
    // 设置iv控件需要动画显示的图片为images集合元素
    self.imageView.animationImages = images;
    // 设置动画持续时间
    self.imageView.animationDuration = 3;
    // 设置动画重复次数
    self.imageView.animationRepeatCount = 999999;
    // 让iv控件开始播放动画
    [self.imageView startAnimating];
```



