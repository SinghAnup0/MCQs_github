🚀 For Android (Android Studio / Java / Kotlin)
By default, the Android WebView does not have access to your phone's physical vibration hardware, and it completely blocks any media from playing audio unless a user interacts with it.

Step 1: Add the Vibration Permission
Open your app's AndroidManifest.xml file and add this line inside the <manifest> tag (just above the <application> tag):

XML


<uses-permission android:name="android.permission.VIBRATE" />
Step 2: Enable JavaScript & Autoplay in your WebView
In your Activity file (e.g., MainActivity.java or MainActivity.kt), you need to configure your WebView settings to allow JavaScript to talk directly to the audio hardware:

Java


WebView myWebView = (WebView) findViewById(R.id.webview);
WebSettings webSettings = myWebView.getSettings();

// 1. Allow JavaScript to run (Required for AudioContext and Vibration API)
webSettings.setJavaScriptEnabled(true);

// 2. Disable the user-gesture requirement for audio playback
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN_MR1) {
    webSettings.setMediaPlaybackRequiresUserGesture(false);
}
🍏 For iOS (Xcode / Swift / WKWebView)
On iOS, WKWebView blocks audio autoplay by default to save battery and data, and it does not support the Web Vibration API at all (navigator.vibrate is ignored by iOS web layers).

To fix the audio block, configure your WKWebView in Swift like this:

Swift


import WebKit

let configuration = WKWebViewConfiguration()

// Fix for Audio: Allow media to play automatically without needing user gestures
configuration.mediaTypesRequiringUserActionForPlayback = []

let webView = WKWebView(frame: .zero, configuration: configuration)
💡 Haptic Workaround for iOS Apps: Since iOS ignores navigator.vibrate inside a WebView, if you want your iPhone users to feel vibrations, you would need to set up a JavaScript-to-Native Bridge (WKScriptMessageHandler) to tell Swift to trigger UIImpactFeedbackGenerator whenever a button is clicked.
