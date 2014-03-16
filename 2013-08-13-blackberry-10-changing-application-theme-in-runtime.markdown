Title: Blackberry 10: Changing application theme in runtime
Date: 2013-08-13 23:15
Tags: Blackberry 10, Cascades, Qt, Theme

Do you want to change an application theme (from bright to dark and back)? Yes, you can.</br>
Do you want to do it in the runtime? No, you can not; a user has to restart the application to apply new theme.

You can easily implement this as it is done in, for example, [FancyTran](http://appworld.blackberry.com/webstore/content/2817/).
The application should update the installed application bar-descriptor.xml file according to the [VisualStyle](http://developer.blackberry.com/cascades/reference/bb__cascades__visualstyle.html).</br>
__Note__: this trick is worked only with old notation: 
```
<cascadesTheme>dark</cascadesTheme>
```

Add following code snippets to appropriate project files:

applicationui.hpp:
```cpp
Q_INVOKABLE void changeTheme(int type);
```

applicationui.cpp:
```cpp
void ApplicationUI::changeTheme(int type) {
        if (1 != type && 2 != type) {
                return;
        }

        const QString values[] = {"<cascadesTheme>bright</cascadesTheme>", "<cascadesTheme>dark</cascadesTheme>"};
        QFile file("app/native/bar-descriptor.xml");
        QByteArray content;

        if (!file.open(QIODevice::ReadOnly)) {
                return;
        } else {
                content = file.readAll();
                file.close();
        }

        if (content.length()) {
                if (!content.contains(values[type - 1].toAscii())) {
                        if (!file.open(QIODevice::WriteOnly | QIODevice::Text)) {
                                return;
                        } else {
                                if (content.contains(values[type % 2].toAscii())) {
                                        content = content.replace(values[type % 2].toAscii(), values[type - 1].toAscii());
                                } else {
                                        content = content.replace("</qnx>", (values[type - 1] + "\n</qnx>").toAscii());
                                }

                                file.write(content);
                                file.close();
                        }
                }
        }
}
```

So, if you export application instance as context property in the applicationui.cpp
```
qml->setContextProperty("App", this);
```
you can invoke implemented method from the QML:
```
....

DropDown {
    id: themeDropDown
    title: qsTr("Theme") + Retranslate.onLocaleOrLanguageChanged

    Option {
        text: qsTr("Bright") + Retranslate.onLocaleOrLanguageChanged
        value: VisualStyle.Bright
        selected: VisualStyle.Bright == Application.themeSupport.theme.colorTheme.style
    }

    Option {
        text: qsTr("Dark") + Retranslate.onLocaleOrLanguageChanged
        value: VisualStyle.Dark
        selected: VisualStyle.Dark == Application.themeSupport.theme.colorTheme.style
    }
}

....

onSomething: {
    App.changeTheme(themeDropDown.selectedValue);
}
```

Restart application and enjoy chosen theme.