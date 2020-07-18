# Webview pentest CheatSheet

There 3 types:
- Android WebView
- iOS UIWebView (Deprecated)
- iOS WKWebView (iOS 8+)

## Strings to check
### JavaScript support
- `enableJavaScript(true)` (Android)
- `webViewPreferences.javaScriptEnabled = false` (iOS)
- `webView.addJavascriptInterface(new JavascriptBridge(), "JavascriptBridge");`(Android)
- `uiWebView.stringByEvaluatingJavaScript(from: javaScriptCode)`(ios)
- `wkWebView.evaluateJavaScript(javaScriptCode, handler: nil)`(ios)

### Access to other files
- `wkWebViewPreferences.setValue("Yes", forKey: "allowFileAccessFromFileURLs")` (WKWebView)
- `webViewSettings.setAllowFileAccess(false);` (Android)
- `webViewSettings.setAllowFileAccessFromeFileURLs(true);` (Android)
- `webViewSettings.setAllowUniversalAccessFromFileURLs(true);`(Android)
### Load HTML
- `loadData(..)`
- `loadDataWithBaseURL(..)`
- `loadHTMLString(..)`
### Remote debugging Android
- `webView.setWebContentsDebuggingEnabled(true);`


## Same-Origin Policy
It is mandatory to limit the origin policy CORS request should be hardened:
- Access-Control-Allow-Origin: http://toto.com
- Access-Control-Allow-Methods: POST, GET
- Access-Control-Allow-Credentials: True

## File Access

|  | iOS UIWebView | iOS WKWebView | Android |
|--|---------------|---------------|---------|
|Access to file | ON, cannot be disabled | ON, Cannot be disabled | ON, can be disabled |
|Access to files from file | ON, can't be disabled | OFF by default, can be enabled | OFF by default, can be enabled |
|Universal access from file (without SOP)| ON, can't be disabled | Always OFF | OFF by default, can be enabled |


