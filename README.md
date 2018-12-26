# pure-aliyun-php-sdk-push

阿里云移动推送的`aliyun-open-api-push`改良版

- 支持composer
- 使用命名空间

## 版本号说明
前三位表示阿里云sdk的版，后一位表示本地版本

## 使用方法

### 安装
```
composer require pure-aliyun-openapi-php-sdk/pure-aliyun-open-api-push
```

### 使用
```php
\AliPure\AliCore\ConfigPure::init();

$accessKeyId = 'xxx';
$accessKeySecret = 'xxx';
$appKey = 'xxx';

$iClientProfile = \AliPure\AliCore\Profile\DefaultProfile::getProfile("cn-hangzhou", $accessKeyId, $accessKeySecret);
$client = new \AliPure\AliCore\DefaultAcsClient($iClientProfile);

// 推送目标
$request->setAppKey($appKey);
$request->setTarget("ALL"); //推送目标: DEVICE:推送给设备; ACCOUNT:推送给指定帐号,TAG:推送给自定义标签; ALL: 推送给全部
$request->setTargetValue("ALL"); //根据Target来设定，如Target=device, 则对应的值为 设备id1,设备id2. 多个值使用逗号分隔.(帐号与设备有一次最多100个的限制)
$request->setDeviceType("ANDROID"); //设备类型 ANDROID iOS ALL.
$request->setPushType("MESSAGE"); //消息类型 MESSAGE NOTICE
$request->setTitle("php title"); // 消息的标题
$request->setBody("php body"); // 消息的内容

// 推送配置: iOS
$request->setiOSBadge("5"); // iOS应用图标右上角角标
$request->setiOSMusic("default"); // iOS通知声音
$request->setiOSApnsEnv("DEV");//iOS的通知是通过APNs中心来发送的，需要填写对应的环境信息。"DEV" : 表示开发环境 "PRODUCT" : 表示生产环境
$request->setiOSRemind("false"); // 推送时设备不在线（既与移动推送的服务端的长连接通道不通），则这条推送会做为通知，通过苹果的APNs通道送达一次(发送通知时,Summary为通知的内容,Message不起作用)。注意：离线消息转通知仅适用于生产环境
$request->setiOSRemindBody("iOSRemindBody");//iOS消息转通知时使用的iOS通知内容，仅当iOSApnsEnv=PRODUCT && iOSRemind为true时有效
$request->setiOSExtParameters("{\"k1\":\"ios\",\"k2\":\"v2\"}"); //自定义的kv结构,开发者扩展用 针对iOS设备

// 推送配置: Android
$request->setAndroidNotifyType("NONE");//通知的提醒方式 "VIBRATE" : 震动 "SOUND" : 声音 "BOTH" : 声音和震动 NONE : 静音
$request->setAndroidNotificationBarType(1);//通知栏自定义样式0-100
$request->setAndroidOpenType("URL");//点击通知后动作 "APPLICATION" : 打开应用 "ACTIVITY" : 打开AndroidActivity "URL" : 打开URL "NONE" : 无跳转
$request->setAndroidOpenUrl("http://www.aliyun.com");//Android收到推送后打开对应的url,仅当AndroidOpenType="URL"有效
$request->setAndroidActivity("com.ali.demo.OpenActivity");//设定通知打开的activity，仅当AndroidOpenType="Activity"有效
$request->setAndroidMusic("default");//Android通知音乐
$request->setAndroidPopupActivity("com.ali.demo.PopupActivity");//设置该参数后启动辅助托管弹窗功能, 此处指定通知点击后跳转的Activity（辅助弹窗的前提条件：1. 集成第三方辅助通道；2. StoreOffline参数设为true
$request->setAndroidPopupTitle("Popup Title");
$request->setAndroidPopupBody("Popup Body");
$request->setAndroidExtParameters("{\"k1\":\"android\",\"k2\":\"v2\"}"); // 设定android类型设备通知的扩展属性


$response = $client->getAcsResponse($request);
print_r("\r\n");
print_r($response);
```

