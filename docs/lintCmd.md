## 代码扫描分析工具
    Correctness：不够完美的编码，比如硬编码、使用过时 API 等
    Performance：对性能有影响的编码，比如：静态引用，循环引用等
    Internationalization：国际化，直接使用汉字，没有使用资源引用等
    Security：不安全的编码，比如在 WebView 中允许使用 JavaScriptInterface 等