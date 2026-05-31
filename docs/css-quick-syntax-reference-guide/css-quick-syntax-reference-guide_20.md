# 13. 字体

字体属性可用于更改字体的各个方面以及加载自定义字体。它们可以应用于任何元素，并且都可以继承。

## `font-family`

`font-family` 设置字体的字型。其值可以是特定的字体名称，例如 `times` 或 `verdana`；也可以是通用字体系列名称，例如 `sans-serif`、`serif` 或 `monospace`。

`font-family : inherit | <family-names> | <generic-families>`

该属性的值是一个包含一个或多个字体名称的优先级列表。如果浏览器无法获取第一种字体，它将使用下一种字体，依此类推。

`font-family: "Times New Roman", times, serif;`

建议在列表末尾使用一个字体系列名称，以确保至少在所有浏览器中字体系列保持一致。请注意，如果字体名称包含空格，则必须用双引号括起来，如上一个示例所示。

## `font-size`

`font-size` 设置字体的大小。该值可以是任何度量单位，或是父元素字体大小的百分比。还有一些预定义的值，如下所列：

`font-size : inherit | <length> | <percentage> |`
            `smaller | larger | xx-small | x-small |`
            `small | medium | large | x-large | xx-large`

`larger` 和 `smaller` 值是相对于父元素字体大小的；其他预定义值指的是绝对大小。初始尺寸是 `medium`，对于普通文本来说是 1 em（16 像素）。

## `font-style`

`font-style` 使文本倾斜。根据规范，`italic` 是正常字体的草书伴生字型，而 `oblique` 是正常字体的倾斜形式。然而，对于大多数字体，这两种字型的渲染方式往往是相同的。

`font-style : inherit | normal | italic | oblique`

## `font-variant`

`font-variant` 可用于使用小型大写字母显示文本，而不是小写字母。`small-caps` 值会以小于常规大写字母的大写字母呈现文本。

`font-variant : inherit | normal | small-caps`

## `font-weight`

`font-weight` 设置字体的粗细。`bolder` 和 `lighter` 值相对于父元素设置粗细，数值则指定绝对粗细。`bold` 的值等于 700，`normal` 等同于 400。

`font-weight : inherit | normal | bold | bolder |`
              `lighter | 100 | 200 | ... | 900`

即使可以指定多个粗细值，但大多数字体只有一种粗体类型，如下面的示例渲染所示：

```
lighter normal bold bolder 100 200 300 400 500 600 700 800 900
```



