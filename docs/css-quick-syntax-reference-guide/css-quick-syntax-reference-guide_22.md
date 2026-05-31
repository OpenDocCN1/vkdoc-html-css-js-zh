# 自定义字体

只有字体安装在用于查看网站的设备上时，才能看到所选字体。如果需要非标准字体，可以使用`@font-face`规则从在线位置加载字体。

```
@font-face {
  font-family: MyFont;
  src: url(myfont.ttf);
}
```

此规则创建了一个名为`MyFont`的字体，并提供了一个浏览器可以从中下载该字体的 URL。有了这个规则，自定义字体就可以像任何标准字体一样使用。

```
p { font-family: "MyFont", arial, sans-serif; }
```

这种使用`@font-face`规则的方式在 Chrome 5+、Firefox 3.5+、IE9+、Safari 3.1+ 和 Opera 10+ 中受支持。如果浏览器不支持自定义字体，则会回退到列表中的下一个标准字体。

