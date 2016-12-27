# pop time SelectView;
     
-(void)openTime{
    
    [UIView animateWithDuration:0.3 animations:^{
        if (!self.grayView) {
            self.grayView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, [UIScreen mainScreen].bounds.size.width, [UIScreen mainScreen].bounds.size.height)];
            self.grayView.backgroundColor = [UIColor colorWithRed:0 green:0 blue:0 alpha:0.5];
            UITapGestureRecognizer *singleTap = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(singleTapGrayView)];
            singleTap.numberOfTapsRequired = 1;
            singleTap.numberOfTouchesRequired = 1;
            [self.grayView addGestureRecognizer:singleTap];
        }
        [[UIApplication sharedApplication].keyWindow addSubview:self.grayView];
        [self creatTimeView];
        [self creatTimeBtn];
    
        self.pickerView.top = self.view.Height - 360 - 80;
        self.timeBtn.top = self.view.Height-70;
        self.tabBarController.tabBar.hidden=YES;
        [self.view bringSubviewToFront:self.pickerView];
        [self.view bringSubviewToFront:self.timeBtn];
    }];
    }
    
# time select button
    
-(void)creatTimeBtn 
{

    self.timeBtn = [[UIButton alloc]initWithFrame:CGRectMake(10, [UIScreen mainScreen].bounds.size.height, [UIScreen mainScreen].bounds.size.width-20, 60)];
    self.timeBtn.layer.masksToBounds = YES;
    self.timeBtn.layer.cornerRadius = 5;
    self.timeBtn.tag = 108;
    self.timeBtn.backgroundColor = [UIColor whiteColor];
    [self.timeBtn setTitle:NSLocalizedString(@"CONFIG_ALERT_NO", nil) forState:UIControlStateNormal];
    [self.timeBtn setTitleColor:[UIColor colorWithRed:59/255.0 green:163/255.0 blue:252/255.0 alpha:1] forState:UIControlStateNormal];
    [self.timeBtn addTarget:self action:@selector(timePickerBtnClicked:) forControlEvents:UIControlEventTouchUpInside];
     [[UIApplication sharedApplication].keyWindow addSubview:self.timeBtn];
}

# time select pickerView
    
-(void)creatTimeView 
{
    
    
    self.pickerView =[[UIView alloc]initWithFrame:CGRectMake(10, [UIScreen mainScreen].bounds.size.height, [UIScreen mainScreen].bounds.size.width-20, 360)];
    self.pickerView.layer.masksToBounds = YES;
    self.pickerView.layer.cornerRadius = 5;
    self.pickerView.backgroundColor = [UIColor whiteColor];
    self.datePicker= [[UIDatePicker alloc]initWithFrame:CGRectMake(0, 0, [UIScreen mainScreen].bounds.size.width, 210)];
    
    self.datePicker.datePickerMode = UIDatePickerModeCountDownTimer;
    [self.pickerView addSubview:self.datePicker];
    self.pickerView.userInteractionEnabled = YES;
    [[UIApplication sharedApplication].keyWindow addSubview:self.pickerView];
    
    UIButton * openBtn = [[UIButton alloc]init];
    UIButton * openBtn = [[UIButton alloc]init];
    UIButton * closeBtn = [[UIButton alloc]init];
    UIButton * okBtn = [[UIButton alloc]init];
    openBtn.tag = 105;
    closeBtn.tag = 106;
    okBtn.tag = 107;
    NSMutableArray * array = [NSMutableArray array];
    [array addObject:openBtn];
    [array addObject:closeBtn];
    
    for (UIButton * btn in array) {
        [btn setTitleColor:[UIColor blackColor] forState:UIControlStateNormal];
        btn.layer.masksToBounds = YES;
        btn.layer.cornerRadius = 25;
        btn.layer.borderColor = [UIColor blackColor].CGColor;
        btn.layer.borderWidth = 1;
        [self.pickerView addSubview:btn];
         [btn addTarget:self action:@selector(timePickerBtnClicked:) forControlEvents:UIControlEventTouchUpInside];
        if (btn.tag==105) {
            [btn setTitle:NSLocalizedString(@"CONFIG_OPEN", nil) forState:UIControlStateNormal];
             btn.frame = CGRectMake([UIScreen mainScreen].bounds.size.width/2-100, 220, 50, 50);
        }else{
            [btn setTitle:NSLocalizedString(@"CONFIG_CLOSE", nil) forState:UIControlStateNormal];
             btn.frame = CGRectMake([UIScreen mainScreen].bounds.size.width/2, 220, 50, 50);
        }
    }
    
    okBtn.frame = CGRectMake(0, 300, [UIScreen mainScreen].bounds.size.width, 60);
    [okBtn setTitle:NSLocalizedString(@"CONFIG_ALERT_YES", nil) forState:UIControlStateNormal];
    okBtn.layer.borderColor = [UIColor colorWithRed:236/255.0 green:236/255.0 blue:236/255.0 alpha:1].CGColor;
    okBtn.layer.borderWidth = 1;
    [okBtn addTarget:self action:@selector(timePickerBtnClicked:) forControlEvents:UIControlEventTouchUpInside];
     [okBtn setTitleColor:[UIColor colorWithRed:59/255.0 green:163/255.0 blue:252/255.0 alpha:1] forState:UIControlStateNormal];
    [self.pickerView addSubview:okBtn];

}
- (void)timePickerBtnClicked:(UIButton *)btn{
    
    if (btn.tag == 105) {
        [self pickerViewGetDate];
    }else if (btn.tag == 106){
        [self pickerViewGetDate];
    }else if (btn.tag == 107){
        [self pickerViewDismiss];
    }else if (btn.tag == 108){
        [self pickerViewDismiss];
    }
}      

# TimeDate git
     
-(void)pickerViewGetDate{

    NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init];
    [dateFormatter setDateFormat:@"HH.mm"];
    self.timeStr = [dateFormatter stringFromDate:self.datePicker.date];
    int hour = [[self.timeStr componentsSeparatedByString:@"."].firstObject intValue];
    int min = [[self.timeStr componentsSeparatedByString:@"."].lastObject intValue];
    NSLog(@"%d,%d",hour,min);
    delayS = hour*3600 + min*60;

}
         
# remove subViews
     
-(void)pickerViewDismiss{

    [UIView animateWithDuration:0.3 animations:^{
        self.pickerView.top = self.view.Height;
        self.timeBtn.top = self.view.Height;
        [self.grayView removeFromSuperview];
    } completion:^(BOOL finished) {
        if (finished) {
            self.tabBarController.tabBar.hidden=NO;
        }
    }];
}
- (void)singleTapGrayView{

    [UIView animateWithDuration:0.3 animations:^{
    
        self.pickerView.top = self.view.Height;
        self.timeBtn.top = self.view.Height;
        [self.grayView removeFromSuperview];
        
    } completion:^(BOOL finished) {
    
        if (finished) {
        
            self.tabBarController.tabBar.hidden=NO;
            
        }
    }];
}


