# Centrixlink-Unity-Plugin

### 版本支持及依赖

* Android 4.0以上系统版本（API LEVEL 14+）
* iOS7以上系统版本
* Unity5.0.1以上系统版本
* 申请：[App ID、App Key](https://dashboard.centrixlink.com/login)

### 集成说明

### 1.Import CentrixlinkAds.unitypackage

### 2.設定App ID、App Key

1.菜單項目 Centrixlink->Config Setting

<img src="http://i.imgur.com/MqnOcQJ.png" width="200">

2.填入您的App ID、App Key 之后点击save按钮

<img src="http://i.imgur.com/zXTqVrN.png" width="403">

### 3.引用命名空間
``` C#
using Centrixlink.Advertisements;
```
### 4.設定錯誤回呼
CentrixlinkAds能让你处理来自CentrixlinkAds的錯誤信息 而不是直接输出
```	C#
//CentrixlinkAds回传錯誤信息時會调用
Advertisement.AddOnErrors (OnError);

public void OnError(string errorMsg)
{
	//收到回呼时的处理 （errorMsg 为CentrixlinkAds的錯誤信息）
}
```

### 5.設定广告准备状态回呼

``` C#
//广告视频準備狀態调用回呼
Advertisement.AddOnAdPlayableChangeds(OnAdPlayableChanged);
public void OnAdPlayableChanged(bool isAdPlayable)
{
	//收到回呼时的处理 （isAdPlayable 为广告视频是否己能播放）
}
```

### 6.初始化CentrixlinkAds
初始化仅需要被调用一次，并且用于使 CentrixlinkAds插件做好准备以向用户呈现广告。<font color="red">尽可能愈早地在您的应用程序中初始化 CentrixlinkAds，因为 SDK 将需要 數十秒进行初始化并缓存广告以供播放，並請一定要先設定OnAdPlayableChanged回呼再调用初始化，
以取得广告准备完成的通知</font>

``` C#
Advertisement.Init ();
```

### 7.播放广告
``` C#
//播放全屏广告
Advertisement.FetchAD ();

//播放非全屏广告(默認大小及位置)
Advertisement.FetchInterstitialAD ();

//客製化播放非全屏广告
//上边距, 左边距, 短边占比(例如：在竖屏模式下，指的是视频广告播放窗口宽度占整个屏幕宽的比例，反之横屏模式下是指视频广告播放窗口高度占整个屏幕高的比例)
//若參數為(0,0,0)(1,0,0)(0,1,0),(0,0,1)則視頻為中央最大顯示
Advertisement.FetchInterstitialAD (top, left, scale);

```

###	Unity應用程序生命周期与SDK关联处理
请在unity应用程序生命周期变更时调用
``` C#
void OnApplicationPause(bool pauseStatus)
{
	if (pauseStatus)
	{
		Advertisement.OnPause ();
	}
	else
	{
		Advertisement.OnResume ();
	}
}
```

### 設定其他回呼
``` C#
//视频广告开始播放时会调用
Advertisement.AddOnAdStarts (OnAdStartCall);
public void OnAdStartCall(string adid)
{
	//收到回呼时的处理（adid 为广告ID）
}

//广告播放結束時會调用
Advertisement.AddOnAdEnds (OnAdEnd);
public void OnAdEnd(string adid, bool wasSuccessfulView, bool wasCallToActionClicked, AdEndState state)
{
	//收到回呼时的处理 （adid为视频广告ID, wasSuccessfulView为视频广告是否完整播放，wasCallToActionClicked为是否点击了视频广告, state为广告播放状态）
}
```

### 移除回呼
``` C#
Advertisement.RemoveOnAdPlayableChangeds(myCallback);
Advertisement.RemoveOnAdStarts(myCallback);
Advertisement.RemoveOnAdEnds(myCallback);
Advertisement.RemoveOnErrors(myCallback);
```
