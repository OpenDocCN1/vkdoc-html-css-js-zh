# 字体

有一个便捷的简写属性`font`，它能够在一个声明中设置所有字体属性。

```
font : inherit | <font-style> + <font-variant> +
       <font-weight> + <font-size> / <line-height> +
       <font-family>
```

这些属性必须按照上述顺序指定。只要保持此顺序，任何一个属性都可以省略（除了**必须**指定的`font-size`和`font-family`）。如果某个属性被省略，则会使用该属性的默认值，即继承父级的值。以下示例向段落元素应用了四个字体属性：

```
p { font: italic 50%/125% Verdana; }
```

这个字体声明在一个声明中设置了`font-style`、`font-size`、`line-height`和`font-family`属性。由于未包含`font-variant`和`font-weight`，使用此声明的一个副作用是它们都会被重新设置为`normal`。

