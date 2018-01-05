## apk签名(即为整个应用)
    http://m.blog.csdn.net/article/details?id=52241715

    http://blog.csdn.net/kickxxx/article/details/18252881

    https://my.oschina.net/u/816213/blog/685762?fromerr=RSS3IhKo

    keytool - list - keystore [keystore证书]

    发现，微信开放平台需要的所谓Signature实际上就是MD5证书指纹，可以通过keytool -printcert
    轻松获得，只不过打印的时候每个字节之间使用:(冒号)做了间隔。完全不需要想官方文档中描述的那样先exportcert然后再自己做md5AdobeAIR应用在微信开放平台注册时的应用签名生成方法

    ------
    采用开发工具FlashDevelop，AIRSDK4.0，采用的p12（PKCS12）证书存储格式。在微信开放平台填写应用创建表格时有一项“应用签名”需要填写，无奈联系客服不能的情况下，找到了微信官方提供的《signature_method.doc》，里面采用的是java一般使用*.keystore格式。
    因此需要对命令做些改变：
    不需要cygwin，JDK自带的keytool完全够用
    增加-storetype pkcs12
    不需要-keypass
    -alias 如果没有的话一般就是1，可以通过keytool -changealias进行修改
    p12文件是一种keystore，因此-keystore .p12
    export出来的cert需要重定向（>)到比如output.cert的文件中
    由于Windows不自带md5sum命令，所以需要第三方工具，WinMD5是个不错的GUI工具。MacOS里面的话可以直接使用命令md5。