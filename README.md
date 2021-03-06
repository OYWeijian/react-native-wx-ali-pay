# react-native-wx-ali-pay
> react-native版 微信支付，支付宝 插件
> 本插件基于 [xiaozhicheng/react-native-pay](https://github.com/xiaozhicheng/react-native-pay) 做了bug修补

# Link

## CMD
```
react-native link react-native-wx-ali-pay
```

##  Android
>none

## iOS
### Step1
在工程target的```General``` -> ```Link Frameworks and Libraries``` -> 加入```libsqlite3.tbd, libc++.tbd, libz.tbd```

### Step2
在工程target的```General``` -> ```Link Frameworks and Libraries``` -> 加入```AlipaySDK.framework，CoreMotion.framework```(注意: 加入的AlipaySDK.framework需要是插件支付宝文件里的AlipaySDK.framework)

### Step3
在工程target的```Build Settings```-> ```Frameworks Search Paths``` -> 加入```"$(SRCROOT)/../node_modules/react-native-wx-ali-pay/ios/PaySdk/支付宝"```

### Step4(可忽略-增强稳定性)
#### AppDegelate.m
>添加文件添加内容
```
import "WXApi.h"
....

(BOOL)application:(UIApplication *)application handleOpenURL:(NSURL *)url { if ([url.host isEqualToString:@"safepay"]) { //支付宝回调 ...//添加回调方法 return YES; }else{ return [WXApi handleOpenURL:url delegate:[WXApiManager sharedManager]]; }}

(BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation {

if ([url.host isEqualToString:@"safepay"]) { //支付宝回调 ...//添加回调方法 return YES; }else{ //微信回调 return [WXApi handleOpenURL:url delegate:[WXApiManager sharedManager]]; } }
```

# Usage
```
import Pay from 'react-native-wx-ali-pay'

const wxObj = {
	appid: 'wxc838755f42580832',
	partnerid: '1494834082',
	noncestr: 'b8mcefmi0kf38a5g3',
	timestamp: '1521906649',
	prepayid: 'wx20180324235049f7cc4eb6260442375925',
	package: 'Sign=WXPay',
	sign: '227FF067FE26CA8F8538260D26D9F435',
}

const aliObj = {
	orderString: ''
}

Pay.onWxPay(wxObj).then(e => console.info(e))

Pay.onAliPay(aliObj).then(e => console.info(e))

// or 
let wxRes = await Pay.onWxPay(wxObj)
if (wxRes.code != 200) return alert(wxRes.msg)
// do something 

let aliRes = await Pay.onAliPay(aliObj)
if (aliRes.code != 200) return return alert(aliRes.msg)
// do something 
```

#  Contributor
[OYWeijian](https://github.com/OYWeijian)
