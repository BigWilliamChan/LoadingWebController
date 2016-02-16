# LoadingWebController
ios UIWebView 进度条 仿微信那种
ios8之前只有通过有私有的API貌似实现，但是苹果审核可能会不准通过。其实现在能看到的大多数都是假的，并不是真正的进度。
网上有很多第三方库，个人认为最好的是NJKWebViewProgress （github网址https://github.com/ninjinkun/NJKWebViewProgress）。它是一个 UIWebView 的进度条接口库，UIWebView 本身是不提供进度条。
对于他实现的原理，看来下大体应该是这样：webViewDidStartLoad:(UIWebView)webView是请求的开始，加载网页都要经过它，[_webViewProxyDelegatewebViewDidStartLoad:webView];未加载网页之前，得到一个URL 有多少个资源需加载，然后使用_loadingCount++来计数。_maxLoadCount=fmax(_maxLoadCount,_loadingCount); 进度使用floatremainPercent = (float)_loadingCount/ (float)_maxLoadCount;来计算 每次webViewDidFinishLoad、didFailLoadWithError请求都加入了

NSString *waitForCompleteJS = [NSString stringWithFormat:@"window.addEventListener('load',function() { 
var iframe = document.createElement('iframe');
 iframe.style.display = 'none'; 
iframe.src = '%@://%@%@';
 document.body.appendChild(iframe);  }, false);",
 webView.request.mainDocumentURL.scheme, 
webView.request.mainDocumentURL.host, completeRPCURLPath];
这样的js到web view中，来检测网页是否加载完成。*
把得到进度逻辑和展示进度的视图分开写，用代理把两个类联系起来，结构清晰、实现起来也会方便很多
实在抱歉，后来没更新博客，我直接贴git地址吧
https://github.com/chenwei910101/LoadingWebController
