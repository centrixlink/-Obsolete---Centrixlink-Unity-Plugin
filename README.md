# Centrixlink-Unity-Plugin



### 集成说明

[API參考文件](https://unity-doc.centrixlink.com/v1-2/index.html)

### 1.请依据你的unity版本，导入合适的CentrixlinkAds.unitypackage版本

### 2.設定App ID、App Key

* 申请：[App ID、App Key](https://dashboard.centrixlink.com/login)

1.菜單項目 Centrixlink->Config Setting

<img src="http://i.imgur.com/MqnOcQJ.png" width="200">

2.填入您的App ID、App Key 之后点击save按钮

<img src="http://i.imgur.com/zXTqVrN.png" width="403">

### 3.引用命名空間
``` C#
using Centrixlink.Advertisements;
```

### 4.設定錯誤回呼事件List
CentrixlinkAds能让你处理来自CentrixlinkAds的錯誤信息 而不是直接输出

```	C#
//若有需要使用 可对List<Action<string>> onDebugLogEvents 做操作
//CentrixlinkAds回传錯誤信息時會调用
Advertisement.onDebugLogEvents.Add(OnDebugLog);

private void OnDebugLog(string message)
{
    //收到回呼时的处理 （message 为CentrixlinkAds的錯誤信息）
} 
```
### 5.設定广告准备状态回呼事件List
``` C#
//List<Action<bool>> onHasPreloadADEvents
//本地是否有预加载广告變化的回呼
Advertisement.onHasPreloadADEvents.Add(OnAdPlayableChanged);
private void OnAdPlayableChanged(bool isAdPlayable)
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
Advertisement.PlayAD ();

//播放非全屏广告(默認大小及位置)
Advertisement.PlayUnFullScreenAD ();

//客製化播放非全屏广告
//上边距, 左边距, 短边占比(例如：在竖屏模式下，指的是视频广告播放窗口宽度占整个屏幕宽的比例，反之横屏模式下是指视频广告播放窗口高度占整个屏幕高的比例)
//若參數為(0,0,0)(1,0,0)(0,1,0),(0,0,1)則視頻為中央最大顯示
Advertisement.PlayUnFullScreenAD (top, left, scale);

```

### 8.PlayerSetting 注意事项

将Minimun API Level  设置为Android4.0以上

<img src="http://i.imgur.com/vbBX2q9.png" width="500">

### 设置全屏視頻是否跟随应用方向
``` C#
/**
设置是否跟随应用方向
@param boolead enable android默认值为true ios默认值为false;
*/
centrixlink.setEnableFollowAppOrientation(enable);
```

### 設定視頻回呼事件List
``` C#
//视频广告即将显示的回呼事件List
//string 广告ID
public static List<Action<string>> onVideoADWillShowEvents;

//视频广告成功显示的回呼事件List
//string 广告ID
public static List<Action<string>> onVideoADDidShowEvents

//视频广告关闭的回呼事件List
//string 广告ID, bool 是否完整播放, bool 是否點擊
public static List<Action<string, bool, bool>> onVideoADCloseEvents

//视频广告失敗的回呼事件List
//ADPlayError 錯誤信息物件
public static List<Action<ADPlayError>> onVideoADShowFailEvents

//视频广告点击的回呼事件List
public static List<Action> onVideoADActionEvents = new List<Action>();
```

### 設定開屏回呼
``` C#
//開屏广告显示的回呼事件List
//string 广告ID
public static List<Action<string>> onSplashADDidShowEvents;

//開屏广告点击的回呼事件List
public static List<Action> onSplashADActionEvents;

//開屏广告失敗的回呼事件List
//ADPlayError 錯誤信息物件
public static List<Action<ADPlayError>> onSplashADShowFailEvents;

//開屏广告关闭的回呼事件List
//string 广告ID, bool 是否完整播放, bool 是否點擊, bool 是否略過
public static List<Action<string, bool, bool, bool>> onSplashADClosedEvents;

//開屏广告略過的回呼事件List
//string 广告ID
public static List<Action<string>> onSplashADSkipEvents;
```


### Unity4.X 注意事項
#### Android Manifest設定

请将以下代码加入至你的AndroidManifest中呼叫开屏方法的Activity里
``` C#
<meta-data android:name="unityplayer.UnityActivity" android:value="true" />
<meta-data android:name="unityplayer.ForwardNativeEventsToDalvik" android:value="true" />
```
<font color="red">若你没更改过预设的Activity及Manifest样版 可照以下方式修改</font>

#### 1.找到默认的Manifest样板
MAC位置:  
/Applications/Unity/Unity.app/Contents/PlaybackEngines/AndroidPlayer/AndroidManifest.xml  

WINDOWS位置:  
C:\Program Files\Unity\Editor\Data\PlaybackEngines\AndroidPlayer\AndroidManifest.xml 

#### 2.加入以下代码

<img src="http://i.imgur.com/cTWZt2M.png" width="600">



