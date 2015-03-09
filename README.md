###Getting Started

Follow the steps in our [Android SDK Integration](https://github.com/RedTroops/Android-SDK/wiki/Home) until you reach Editing The Source Code section.

You should have:
* Imported RedTroops SDK.
* Edited the manifest file, including the activities and meta data.
* Added Google Play Services (if you need push notifications integration).
* Have no compilation errors.

###cocos2d-x Code

To show an interstitial ad, add this code to the click event:
```cpp
 JniMethodInfo t;
 
 if (JniHelper::getStaticMethodInfo(t,
   "<CLASS-NAME>",
   "showInterstitialAd",
   "()V")) {
  
   t.env->CallStaticVoidMethod(t.classID, t.methodID);
   t.env->DeleteLocalRef(t.classID);
 }
```
To show a banner ad, add this code to the click event:
```cpp
 JniMethodInfo t;
 
 if (JniHelper::getStaticMethodInfo(t,
   "<CLASS-NAME>",
   "showBannerAd",
   "()V")) {
  
   t.env->CallStaticVoidMethod(t.classID, t.methodID);
   t.env->DeleteLocalRef(t.classID);
 }
```

CLASS-NAME is the path of the class. An example CLASS-NAME for RedTroops-CocosTest app is org/cocos2dx/cpp/AppActivity.

###Java Code

1) Declare at the top of your activity's class:
```java
	static Activity mContext;
```

Add to your main activity's `OnCreate` or `OnResume`, call:
```java
	mContext = this;
```

2) In your main activity's `onCreate`, call:
```java
	RedTroops.getInstance(mContext).init(initFinishedListener);
```
Where `initFinishedListener` is a listener to declare as follows:
```java	
	private initFinishListener initFinishedListener = new initFinishListener() {

		@Override
		public void onSuccess() {
			// TODO Do on init success. Most probably showInterstitialAd();
		}

		@Override
		public void onFail() {
			// TODO Do on init failure
		}
	};
```

> You might want to call `RedTroops.getInstance(mContext).showInterstitialAd(mContext);` in your `OnSuccess();`

2) 
Add two methods to your Activity:
```java
	static void showInterstitialAd(){
		mContext.runOnUiThread(new Runnable() {
			
			@Override
			public void run() {
				RedTroops.getInstance(mContext).showInterstitialAd(mContext);
				
			}
		});
	}
```
```java
	static void showBannerAd(){
		mContext.runOnUiThread(new Runnable() {
			
			@Override
			public void run() {
				RedTroops.getInstance(mContext).showBanner(mContext);
			}
		});
	}
```

3) In your activity's `onPause`:
```java
	RedTroops.getInstance(this).onPause();
```
4) In your activity's `onResume`:
```java
	RedTroops.getInstance(this).onResume();
```

You may refer to CocosTest app that is available.

>Note that cocos2d folder (which contains cocos2d binaries) is not available in order to decrease the size of the repository. Please copy it from one of your projects manually if you'd like to run CocosTest app project.
