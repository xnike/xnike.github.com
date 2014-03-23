Title: Cascades for Blackberry 10. Theme based asset selector
Date: 2012-12-19 16:58
Tags: [BB10, BBNDK, Blackberry, Cascades, QML

Upcoming Blackberry 10 release will have full theme support. According to the [API Documentation](https://developer.blackberry.com/cascades/reference/bb__cascades__themesupport.html) there are Brigth and Dark themes:
{% img center https://developer.blackberry.com/cascades/files/reference/images/theme_examples.png %}

The default theme is Bright. To change the application theme to Dark specify the following configuration value in the application`s bar-descriptor.xml file:

```
<env var="CASCADES_THEME" value="dark"/>
```

or previously

```
<cascadesTheme>dark</cascadesTheme>
```

If You have, for example, a background image for each of the available themes - "asset:///images/bright/background.png" and "asset:///images/dark/background.png" - You can easily manage it by adding a globally available property:

```js
import bb.cascades 1.0

Page {
    id: mainPage;
    // this property will contain string representation of the current application theme.
    property string theme: themeStyleToString(Application.themeSupport.theme.colorTheme.style);
    
    Container {
        ImageView {
            imageSource: "asset:///images/" + mainPage.theme + "/background.png"
        }       
    }
    
    function themeStyleToString(style) {
        switch(style) {
            case VisualStyle.Bright:    return "bright"
            case VisualStyle.Dark:      return "dark"
        }
            
        // will use dark as default in case of unknown
        return "dark"
    }
}
```

Unfortunately, right now I know only one way to change the application theme - via bar-descriptor.xml.