---
layout: post
title:  "MAC 浏览器打开APP"
category: web
tags: [url,scheme,QT]
---
# MAC 浏览器打开APP

## 通过mac的浏览器打开应用程序，并捕获到传递的参数，在应用程序中进行相关处理
1. 修改info.plist
在info.plist中添加以下文本
```
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleURLName</key>
        <string>Open File</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>opener</string>
        </array>
    </dict>
</array>
```
**注意** <key>CFBundleURLTypes</key> 这句不能忽略，key和string都是配对的。
其中opener为你需要在浏览器中输入的url的前缀
比如 opener://sample.com
2. info.plist注入到mac系统的scheme中
打包，安装你的应用程序test.app。在浏览器中输入
opener://sample.com
就会浏览器弹框弹出提示是否打开test.app  点击是，系统就会发送事件。
**注意** 一定要安装才能注册。在代码调试中是不起作用的
3. qt 接收信号
qt 4版本以后已经支持接收scheme信号,无需写OC的代码
重载QApplication中的event，捕获openfile事件即可
```
bool QtSingleApplication::event(QEvent *event)
{
#ifdef Q_OS_MAC
    if (event->type() == QEvent::FileOpen)
    {
        QFileOpenEvent *openEvent = static_cast<QFileOpenEvent *>(event);
        if (!openEvent->file().isEmpty())
        {
            qDebug()<<"QtSingleApplication::event QEvent::FileOpen"<<openEvent->file();
            emit messageReceived(openEvent->file());
        }
        else if (openEvent->url().isValid())
        {//xxx
            qDebug()<<"QtSingleApplication::event QEvent::urlopen"<<openEvent->url().toString(QUrl::None);
            emit messageReceived(openEvent->url().toString());
        }
    }
#endif // Q_OS_MACOS
    return QApplication::event(event);
}
```
**注意**  *在mac上输入的url要合法qt的QEvent::FileOpen才能拿到url。否则拿不到url。
例如opener://startMeeting:776356825/ 浏览器认为startMeeting后面跟的是端口号，而端口号超过了65535则是非法的。
所以以上url可以定义为：opener://startMeeting?776356825/
或者扩展性更好的例如  opener://startMeeting?meetingid=776356825/*
