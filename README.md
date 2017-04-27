# Centrixlink-Unity-Plugin

### 版本支持及依赖

* Android 4.0以上系统版本（API LEVEL 14+）
* iOS7+ 版本
* Unity版本：ＸＸＸＸＸ以上
* 申请：[App ID、App Key](https://dashboard.centrixlink.com/login)

### 集成说明

### 1.Import centrixlinkADs.package

### 2.設定App ID、App Key
(集成需要 Centrixlink 账户，因此，如果您没有 Centrixlink 账户，请创建 [Centrixlink 帐户)](https://dashboard.centrixlink.com/login)。

<img src="http://i.imgur.com/cKEe1qo.png">

填入您的App ID、App Key 之后点击save按钮

<img src="http://i.imgur.com/zXTqVrN.png">

### 3.引用centrixlink命名空間
``` C#
using Centrixlink.Advertisements;
```
### 4.設定log回呼
centrixlink能让你处理来自centrixlink的log信息 而不是直接输出
```	C#
//CentrixLink回传log信息時會调用
Advertisement.AddOnLogProcListeners (OnLogCall);

public void OnLogCall(string log, LogLevel level)
{
	//收到回呼时的处理 （log 为centrixlink的log信息, level 為log類型）
 }
```
你可以这样做
```	C#
//有收到log時會调用
Advertisement.AddOnLogProcListeners (OnLog);

public void OnLog(string log, LogLevel level)
{
	switch(level)
	{
		case LogLevel.ERROR :
			Debug.LogError("Unity error: "+log);
			break;
		case LogLevel.WARNING :
			Debug.LogWarning("Unity warning: "+log);
			break;
		default  :
			Debug.Log("Unity log: "+log);
			break;
		}
	}
}
```
### 5.設定广告准备状态回呼及初始化Centrixlink SDK
初始化仅需要被调用一次，并且用于使 Centrixlink Unity 插件做好准备以向用户呈现广告。<font color="red">尽可能愈早地在您的应用程序中初始化 Centrixlink SDK，因为 SDK 将需要 數十秒进行初始化并缓存广告以供播放</font>，可播放影片时会调用回呼，<font color="red">建議在初始化前先設定回呼</font>

``` C#
//广告视频下载成功后会调用回呼
Advertisement.AddOnAdPlayableChangeds(OnAdPlayableChanged);
public void OnAdPlayableChanged(bool isAdPlayable)
{
	//收到回呼时的处理 （ isAdPlayable为視屏是否能播放）

}

Advertisement.Init ();
```


### 6.呼叫廣告
``` C#
//播放非全屏广告
Advertisement.FetchInterstitialAD ();

//播放全屏广告
Advertisement.FetchAD ();

```


### 設定其他回呼
``` C#
//视频广告开始播放时会调用
Advertisement.AddOnAdStarts (Action<string> onAdStart);
public void OnAdStartCall(string adid)
{
	//收到回呼时的处理（adid 为广告ID）
}
    
//视频广告播放結束時會调用
Advertisement.AddOnAdEnds (Action<string, bool, bool> onAdEnd);
public void onAdEnd(string adid, bool wasSuccessfulView, bool wasCallToActionClicked)
{
	//收到回呼时的处理 （adid为视频广告ID, wasSuccessfulView为视频广告是否完整播放，wasCallToActionClicked为是否点击了视频广告）
}
 
// 视频广告播放失败時會调用
Advertisement.OnAdUnavailable (OnAdUnavailable );
public void OnAdUnavailable(string reason)
{
	//收到回呼时的处理(reason为失败原因信息）
}
```

### 移除已存在的回呼
``` C#
OnDestroy()
{
	Advertisement.RemoveOnLogProcListeners(myCallback);
	Advertisement.RemoveOnAdPlayableChangeds(myCallback );
	Advertisement.RemoveOnAdStarts(myCallback );
	Advertisement.RemoveOnAdEnds(myCallback );
	Advertisement.RemoveOnAdUnavailables(myCallback );
}
```
