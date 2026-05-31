# Less 变量与混入

### 变量

`Less`中的变量用于存储值，使这些值能够在代码中重复使用。通过变量，您可以从单一位置控制可复用的值。在`Less`中，使用`@`符号创建变量。这一点在 Listing 4-10 中进行了演示。

**代码清单 4-10**. 声明和使用`Less`变量的示例

```less
@fontColor:#666;
body{
        color:@fontColor;
}
```

**代码清单 4-11**. 代码清单 4-10 的输出

```css
body {
  color: #666;
}
```

变量的用途不仅限于赋值。它们还可以用于选择器、属性名称等位置。这可以通过插值（interpolation）实现，如代码清单 4-12 所示。

**代码清单 4-12**. 演示插值的示例

```less
@parent: container;
.@{parent} {
  width:100%;
}
```

**代码清单 4-13**. 代码清单 4-12 的输出

```css
.container {
  width: 100%;
}
```

代码清单 4-14 至 4-18 展示了一些有效的变量声明示例。

**代码清单 4-14**. 变量声明的有效示例

```less
@color: red;
@list: a b c d;
@csv-list: a, b, c, d;
```

尽管变量类似于常量，但它们的值是可变的。它遵循一个简单的原则：后声明的变量生效。

在`Less`中，变量遵循惰性加载（lazy loading）模式，如代码清单 4-15 所示。惰性加载在技术上的含义是，对象可以在需要时才进行初始化。这与急加载（eager loading）相反，急加载要求在使用前完成初始化。

**代码清单 4-15**. 演示变量惰性加载的示例

```less
.container {
        font-size: @size;
}
@size: 12px;
```

**代码清单 4-16**. 代码清单 4-15 的输出

```css
.container {
  font-size: 12px;
}
```

从`Less`版本 1.7 开始，可以将一组属性存储到变量中。代码应放置在花括号内，类似于混入（mixin）的写法。这称为规则集（ruleset），如代码清单 4-17 所示。

**代码清单 4-17**. 演示存储和使用规则集的示例

```less
@disclaimer-ruleset: { color: #CCC; font-size: 10px; };
.disclaimer {
        @disclaimer-ruleset();
}
```

在代码清单 4-17 中，我们将一个规则集存储在变量中，并在`disclaimer`类内部使用它。代码清单 4-18 显示了渲染后的最终输出。

**代码清单 4-18**. 代码清单 4-17 的输出

```css
.disclaimer {
  color: #CCC;
  font-size: 10px;
}
```

### 注释

注释有助于记录代码，使其更易于阅读和理解。您无需为了减小文件大小而减少注释数量。当文件被编译并生成最终的`CSS`代码时，注释会被移除。

在`Less`中添加注释的方式与`CSS`相同。注释写在`/* */`之间。您也可以使用`//`前缀来编写单行注释。

有一种方法可以通知编译器某个特定注释是重要的，并且它应保留在最终编译版本中，如代码清单 4-19 所示。

**代码清单 4-19**. 告知编译器某个注释是重要的

```less
/*!
This needs to come in final output
!*/
```

### 混入

混入是可复用的代码块，可以在整个代码中使用。混入可以包含完整的`CSS`规则。混入可以参数化；即它们可以接受参数，这使其非常灵活。在`Less`中定义混入的方式如代码清单 4-20 所示。

**代码清单 4-20**. `Less`中的混入示例

```less
.notification(@value:5px) {
  border-radius: @value;
  padding: @value;
}
.errorLooks{
  .notification;
}
.successLooks{
  .notification(10px);
}
```

在`Less`中，混入的声明方式类似于任何类定义。要在另一个规则中包含一个混入，只需写出混入的名称，前面加上句点，并可选地在括号内提供参数。混入允许您通过简单地将混入名称作为其属性之一，将一个类的所有属性嵌入到另一个类中。

**代码清单 4-21**. 代码清单 4-20 的输出

```css
.errorLooks {
  border-radius: 5px;
  padding: 5px;
}
.successLooks {
  border-radius: 10px;
  padding: 10px;
}
```

如您所见，`notification`扩展类并未在最终输出中渲染。

此外，您可以混入一个类或`ID`选择器，这在代码清单 4-22 中进行了演示。

**代码清单 4-22**. 混入`ID`和类的示例

```less
.notification,#notification {
  border-radius: 5px;
  padding: 5px;
}
.errorLooks{
  .notification;
}
.successLooks{
  #notification;
}
```

**代码清单 4-23**. 代码清单 4-22 的输出

```css
.notification,
#notification {
  border-radius: 5px;
  padding: 5px;
}
.errorLooks {
  border-radius: 5px;
  padding: 5px;
}
.successLooks {
  border-radius: 5px;
  padding: 5px;
}
```

如果您想创建一个混入，但不在渲染后的`.css`文件中输出它，只需在其后附加一对括号即可。您还可以在混入定义中包含选择器以及样式规则。这在代码清单 4-24 中进行了演示。

**代码清单 4-24**. 演示如何在混入定义中包含选择器的示例

```less
.notification() {
  border-radius: 5px;
  padding: 5px;
  &:hover{
    border:2px solid #fff;
  }
}
.errorLooks{
  .notification;
}
.successLooks{
  .notification;
}
```

**代码清单 4-25**. 代码清单 4-24 的输出

```css
.errorLooks {
  border-radius: 5px;
  padding: 5px;
}
.errorLooks:hover {
  border: 2px solid #fff;
}
.successLooks {
  border-radius: 5px;
  padding: 5px;
}
.successLooks:hover {
  border: 2px solid #fff;
}
```

并不总是需要包含整个混入。您也可以只包含其中的特定部分，例如，其中的选择器。这在代码清单 4-26 中进行了演示。

**代码清单 4-26**. 在另一个选择器内部使用混入中的选择器

```less
.notification() {
  border-radius: 5px;
  padding: 5px;
  .inner{
      border:2px solid #fff;
  }
}
.errorLooks{
  .notification;
}
.successLooks{
  .notification .inner;
}
```

**代码清单 4-27**. 代码清单 4-26 的输出

```css
.errorLooks {
  border-radius: 5px;
  padding: 5px;
}
.errorLooks .inner {
  border: 2px solid #fff;
}
.successLooks {
  border: 2px solid #fff;
}
```

如果您观察代码清单 4-27，`.sucessLooks`只采用了混入中内部样式（`inner`）的样式。当您在混入后附加`!important`时，所有属性都会带上`!important`，如代码清单 4-28 所示。

**代码清单 4-28**. 在混入中使用`!important`的实现



```
.notification() {
    border-radius: 5px;
    padding: 5px;
    .inner {
        border: 2px solid #fff;
    }
}
.errorLooks {
    .notification;
}
.successLooks {
    .notification .inner !important;
}
```

**清单 4-29**：清单 4-28 的输出

```
.errorLooks {
  border-radius: 5px;
  padding: 5px;
}
.errorLooks .inner {
  border: 2px solid #fff;
}
.successLooks {
  border: 2px solid #fff !important;
}
```

Mixin 类似于任何函数式编程语言中的函数。因此，与任何函数式语言一样，它们可以接受参数。这在清单 4-30 中得到了演示，我们为圆角创建了一个参数化的 mixin，它接受一个半径参数。

**清单 4-30**：演示参数化 Mixin 的示例

```
.roundedEdges (@radius) {
    -webkit-border-radius: @radius;
    -moz-border-radius: @radius;
    -o-border-radius: @radius;
    border-radius: @radius
}

.info {
    font-weight: bold;
    .roundedEdges(50px)
}
```

**清单 4-31**：清单 4-30 的输出

```
.info {
  font-weight: bold;
  -webkit-border-radius: 50px;
  -moz-border-radius: 50px;
  -o-border-radius: 50px;
  border-radius: 10px;
}
```

Mixin 中的参数可以有默认值，当调用 mixin 时没有传递参数时，可以使用该默认值。因此，你可以通过为半径参数添加一个默认值来修改前面的示例。这看起来就像清单 4-32 所示。

**清单 4-32**：演示 Mixin 参数默认值的示例

```
.roundedEdges (@radius:5px) {
    -webkit-border-radius: @radius;
    -moz-border-radius: @radius;
    -o-border-radius: @radius;
    border-radius: @radius
}

.info {
    font-weight: bold;
    .roundedEdges(50px);
}

.note {
    font-style: italic;
    .roundedEdges();
}
```

**清单 4-33**：清单 4-32 的输出

```
.info {
  font-weight: bold;
  -webkit-border-radius: 50px;
  -moz-border-radius: 50px;
  -o-border-radius: 50px;
  border-radius: 50px;
}
.note {
  font-style: italic;
  -webkit-border-radius: 5px;
  -moz-border-radius: 5px;
  -o-border-radius: 5px;
  border-radius: 5px;
}
```

Mixin 可以接受多个参数，形式为用分号分隔的值，这与其他编程语言类似。在 Less 中，使用分号，因为逗号的行为存在歧义。逗号用于分隔参数和列表项。

Less 中的 Mixin 也支持重载。这在清单 4-34 中进行了演示。

**清单 4-34**：展示 Mixin 重载的示例

```
.specialArea(@color; @width) {
    border: 2px dashed @color;
    width: @width;
    color: @color;
}
.specialArea(@color;) {
    color: @color;
}

.fullEmphasis {
    background-color: #BBB;
    .specialArea(#FFF);
}

.sideEmphasis {
    background-color: #BBB;
    .specialArea(#FFF,200px)
}
```

在清单 4-34 中，`specialArea` 有两个变体。一个接受单个参数——颜色，另一个接受两个参数。这段代码的输出如清单 4-35 所示。

**清单 4-35**：清单 4-34 的输出

```
.fullEmphasis {
  background-color: #BBB;
  color: #FFF;
}
.sideEmphasis {
  background-color: #BBB;
  border: 2px dashed #ffffff;
  width: 200px;
  color: #FFF;
}
```

Less 有两个特殊的变量，分别称为 `@arguments` 和 `@rest`。

#### `@arguments`

`@arguments` 变量包含传递给 mixin 并存在于其内部的所有参数的列表。在 Less 中，列表是由空格分隔的值，因此你可以将 `@arguments` 用于由空格分隔的属性，例如边框样式、外边距等。这在清单 4-36 中进行了演示。

**清单 4-36**：演示 `@arguments` 用法的示例

```
.borderAroundElement(@width:1px; @style:solid; @color: #000) {
    border: @arguments;
}
.subsection {
    .borderAroundElement();
}
.impInlineContent {
    .borderAroundElement(2px;dashed;#f00);
}
```

此示例为边框参数提供了值，然后使用 `@arguments` 将这些值赋给 border 属性。此清单的输出如清单 4-37 所示。

**清单 4-37**：清单 4-36 的输出

```
.subsection {
  border: 1px solid #000;
}
.impInlineContent {
  border: 2px dashed #f00;
}
```

#### `@rest`

`@rest` 使你能灵活地使用数量不定的参数调用 mixin。这在清单 4-38 中进行了演示。

**清单 4-38**：演示 `@rest` 用法的示例

```
.borderAroundElement(@width:10px; @rest...) {
    width: @width;
    border: @rest;
}
.subsection {
    .borderAroundElement(100px;1px solid #666);
}
```

正如你在前面的示例中所见，当你传递一个属性列表进行赋值时，`@rest` 就派上了用场。

#### 返回值

Mixin 不像在其他编程语言中那样返回值。然而，你可以利用它们的作用域来模拟这种行为。如果一个变量在调用者的作用域中未定义，它将被复制到调用者的作用域中。这在清单 4-39 中进行了演示。

**清单 4-39**：演示返回值的示例

```
.setPaddingMarginMixin() {
    @padding: 15px;
    @margin: 30px;
}
.modifiedPMMixin() {
    @margin: 25px;
    padding: @padding;
    margin: @margin;
    .setPaddingMarginMixin();
}
.container {
    .modifiedPMMixin();
}
```

在这段代码中，`@padding` 的值被返回，然后被添加到了 `modifiedPMMixin` 的作用域中。这段代码的输出如清单 4-40 所示。

**清单 4-40**：清单 4-39 的输出

```
.container {
  padding: 15px;
  margin: 25px;
}
```

#### 嵌套规则

CSS 不会体现元素之间的这种关系。使用 Less，你的代码能够反映元素之间的关系，并且可以像清单 4-41 那样编写。

**清单 4-41**：使用 Less 反映元素之间关系的示例

```
#sidebar {
    .heading {
        font-weight: bold;
        color: #BBB;
    }
    ul {
        list-style: none;
        li {
            a {
                color: #006;
            }
        }
    }
}
```

**清单 4-42**：清单 4-41 的输出

```
#sidebar .heading {
  font-weight: bold;
  color: #BBB;
}
#sidebar ul {
  list-style: none;
}
#sidebar ul li a {
  color: #006;
}
```

Less 提供的这种模式在以下方面是有益的：

*   它减少了代码冗余。你无需为多个子元素重复编写相同的选择器。
*   它使代码可读且易于维护。

尽管嵌套是一个有用的特性，但应谨慎使用。嵌套会导致 DOM 和 CSS 代码之间的紧密绑定，因此由于特异性问题，会限制覆盖规则的可能性。

#### 导入指令

Less 提供了一个导入指令，用于从其他位置导入样式。与 CSS 不同，CSS 要求导入语句必须位于文件开头，而 Less 允许将 `@import` 语句放置在代码中的任何位置。



`@import`语句的行为可能取决于导入文件的扩展名。

- 如果导入的文件以`.css`结尾，它将被视为普通的 CSS 导入，语句保持不变。
- 对于任何其他扩展名，它将被视为 Less 文件并导入。如果缺少`.less`扩展名，则会自动追加，并导入文件内容。

***列表 4-43***：展示各种导入方法的示例

```
@import "filetoimport";
@import "filetoimport.less";
@import "filetoimport.php";
@import "filetoimport.css";
```

在此示例中，前三种方法将文件视为 Less 文件并导入其内容。然而，最后一种方法将被视为普通的 CSS 导入，因为文件名以`.css`结尾。

你可以为导入操作指定几个选项：

- `reference`：将使用 Less 文件，但不输出其内容
- `inline`：在输出中包含导入文件的内容，但不处理它
- `less`：无论文件扩展名是什么，都将其视为 Less 文件
- `css`：无论文件扩展名是什么，都将其视为 CSS 文件
- `once`：只包含该文件一次（这是默认行为）
- `multiple`：多次包含该文件
- `optional`：当文件未找到时继续编译

#### Mixin Guards

*Guards*在需要基于条件应用表达式时非常有用。它们也被称为*条件混入（conditional mixins）*。

Less 使用带守卫的混入（guarded mixins）来实现条件执行，而不是使用`if/else`语句，如列表 4-44 所示。

***列表 4-44***：展示 Mixin Guards 的示例

```
.switchContrast (@color) when (lightness(@color) >= 60%) {
  background-color: #000;
}
.switchContrast (@color) when (lightness(@color) < 60%) {
  background-color: #FFF;
}
.switchContrast (@color) {
  color: @color;
}
.container{
    .switchContrast(#666);
}
```

列表 4-44 演示了实现一个 mixin guard，检查输入颜色的亮度，并根据其值决定对比色组合。

***列表 4-45***：列表 4-46 的输出

```
.container {
  background-color: #FFF;
  color: #666666;
}
```

Guards 支持以下比较运算符：`>`、`<`、`>=`、`<=`和`=`。列表 4-46 演示了这样一个场景。

***列表 4-46***：展示在 Mixin Guard 中使用比较运算符的示例

```
@mediaType: mobile;
.setScreen (@width) when (@mediaType = mobile) {}
.setScreen (@width) when (@mediaType = desktop) {}
```

Guards 还支持逻辑运算符，例如`and`、`or`和`not`。你可以使用`and`关键字来组合 guards，如列表 4-47 所示。

***列表 4-47***：展示`and`运算符的示例

```
@mediaType: mobile;
@orientation: landscape;
@width: 960px;
.setScreen (@width) when (@mediaType = mobile) and (@orientation="landscape") {}
```

你可以通过用逗号分隔 guards 来使用`or`运算符，如列表 4-48 所示。

***列表 4-48***：展示`or`运算符的示例

```
.setScreen (@width) when (@mediaType = desktop),(@width >= 960) {}
```

你也可以使用`not`运算符进行否定，如列表 4-49 所示。

***列表 4-49***：展示`not`运算符的示例

```
.setScreen (@width) when not (@width >= 960) {}
```

对于基于值类型的混入匹配，你可以使用`is`函数。参见列表 4-50。

***列表 4-50***：展示`isnumber`的示例

```
.setVal (@val1; @val2: 0) when (isnumber(@val2)) { ... }
```

Less 中可用的类型检查函数如下：

- `iscolor`
- `iskeyword`
- `isnumber`
- `isstring`
- `isurl`

稍后说明的`default`函数，可用于根据其他混入进行混入匹配。

***列表 4-51***：展示"条件混入（Conditional Mixins）"的示例

```
.setVal (@a) when (@a > 0) { ...  }
.setVal (@a) when (additionalCheck()) { ... }
```

## 引用父选择器

Less 使用`&`符号来引用当前选择器的父级。`&`符号反转了嵌套顺序，并且可以扩展或合并类，如列表 4-52 所示。

***列表 4-52***：展示父选择器的示例

```
.button
{
    background:#F00;
    &:hover{
        background: #500;
    }
}
```

***列表 4-53***：列表 4-52 的输出

```
.button {
  background: #F00;
}
.button:hover {
  background: #500;
}
```

`&`符号主要用于向一个类添加伪类。这在之前的列表中以一个简单示例进行了演示，即添加了`:hover`伪类。

另一个可以使用父选择器的常见场景是构建 clear fix。这可以通过将父选择器与`:after`一起使用来实现。这在列表 4-54 中进行了演示。

***列表 4-54***：使用父选择器构建 clear fix

```
.clearfix() {
    &:after {
        clear: both;
    }
}

.column{
    .clearfix;
}
```

***列表 4-55***：列表 4-54 的输出

```
.column:after {
  clear: both;
}
```

除了 CSS 提供的伪类之外，Less 还提供了额外的伪类，即`extend`伪类，其语法类似于 CSS 伪类。`extend`伪类有助于扩展选择器，如列表 4-56 所示。

***列表 4-56***：展示`extend`伪类的示例

```
.primary{
    color: #00F;
    &:hover {
        color: #006;
    }
}
.secondary:extend(.primary){

}
```

列表 4-56 通过使用 Less 提供的`extend`伪选择器扩展了`primary`类。此代码的输出如列表 4-57 所示。

***列表 4-57***：列表 4-58 的输出

```
.primary,
.secondary {
  color: #00F;
}
.primary:hover {
  color: #006;
}
```

## 将`&`与 Guards 结合使用

Guards 可以与`&`符号一起使用，以创建条件语句。

这在列表 4-58 中进行了演示，其中父选择器与一个 guard 结合使用，根据作为参数传入的颜色级别来决定使用哪种颜色。

***列表 4-58***：将 Guard 与父选择器结合使用的示例

```
.colorGenerator(@level) {
    & when(@level>5) {
        color: #600;
    }
    & when (@level<=5){
        color:#060;
    }
}
p.lead {
    .colorGenerator(6);
}
p.secondLead {
    .colorGenerator(1);
}
```

***列表 4-59***：列表 4-58 的输出

```
p.lead {
  color: #600;
}
p.secondLead {
  color: #060;
}
```

## 分离规则和混入

分离规则集（detached rulesets）是一组规则，包含有效的 Less 或 CSS 代码，并存储在一个变量中。在 Less 中，这些分离规则集可以作为参数传递给混入，如列表 4-60 所示。

***列表 4-60***：分离规则的示例

```
@errorStyleSettings: { color: #A00; border: 1px solid #A00 }
```



## 调用分离的规则集

可以通过在末尾添加括号来调用分离的规则集。使用括号时，分离规则集中的内容会被复制到 CSS 规则集内部。这在清单 4-61 中进行了演示。

**清单 4-61**. 在另一个样式声明中添加分离的规则集

```
@errorStyleSettings: {
  color: #A00;
  border: 1px solid #A00;
};
.errorDialog{
    @errorStyleSettings();
}
```

混入中的分离规则可以防止代码冗余。当你需要在不同的媒体查询或媒体查询内嵌套的类中使用不同的样式规则时，这种方法最为适用。

## 数字与颜色的运算

Less 支持加、减、乘、除等算术运算。此外，还可以通过在运算表达式周围添加括号来改变执行顺序。这些运算可应用于变量、值和数字。

**清单 4-62**. 演示算术运算的示例

```
.aside{
    width:percentage(1 - (0.4+0.2))
}
```

**清单 4-63**. 清单 4-64 的输出

```
.aside {
  width: 40%;
}
```

Less 也支持颜色操作。这些操作可以对不同单位的值和颜色进行。这在清单 4-64 中进行了演示，我们取一个输入颜色并将其值减少 10%。

**清单 4-64**. 在 Less 中演示颜色操作的示例

```
@color: yellow;
.lighten(@color){
        color : @color - 10%;
}
.contrastContainer{
    .lighten(@color)
}
```

**清单 4-65**. 清单 4-64 的输出

```
.contrastContainer {
  color: #f5f500;
}
```

## 合并属性

属性合并对于接受逗号分隔或空格分隔值的属性非常有用。例如：边框、背景、过渡和`font-family`属性。可以通过在要合并的属性名称后添加`+`标志（用于逗号分隔列表）或`+_`标志（用于空格分隔列表）来合并属性。

清单 4-66 展示了一个合并逗号分隔值的示例。它合并了`font`属性的值。这里有两个混入——`fontSettings`和`fallbackFonts`——并且`fallbackFonts`的值被合并到`fontSettings`中。

请注意，如果你漏掉了`+`: 并将其写成`:`，那么来自`fallbackFonts`的内容将被添加在新的一行，并覆盖`fontSettings`提供的字体设置。

**清单 4-66**. 演示合并逗号分隔值的示例

```
.fallbackFonts(){
    font-family+: Helvetica,sans-serif;
}
.fontSettings(){
    font-family+: Calibri;
    .fallbackFonts;
}
body {
    .fontSettings;
}
```

**清单 4-67**. 清单 4-66 的输出

```
body {
  font-family: Calibri, Helvetica, sans-serif;
}
```

## 循环混入

可以让混入递归执行。这可以通过将它们与混入守卫或模式结合来实现。

这在清单 4-68 中进行了演示，该清单使用混入守卫和循环生成了 12 列网格。

**清单 4-68**. 演示混入递归行为的示例

```
.generateGridLayout(12);
.generateGridLayout(@n, @i: 1) when (@i =< @n) {
  .col-@{i} {
    width: (@i * 100% / @n);
  }
  .generateGridLayout(@n, (@i + 1));
}
```

**清单 4-69**. 清单 4-68 的输出

```
.col-1 {
  width: 8.33333333%;
}
.col-2 {
  width: 16.66666667%;
}
.col-3 {
  width: 25%;
}
.col-4 {
  width: 33.33333333%;
}
.col-5 {
  width: 41.66666667%;
}
.col-6 {
  width: 50%;
}
.col-7 {
  width: 58.33333333%;
}
.col-8 {
  width: 66.66666667%;
}
.col-9 {
  width: 75%;
}
.col-10 {
  width: 83.33333333%;
}
.col-11 {
  width: 91.66666667%;
}
.col-12 {
  width: 100%;
}
```

## 函数

Less 提供了许多内置函数。通过这些内置函数，你可以根据需要操作值。这些函数可以在守卫表达式中使用。完整的函数列表请访问 [`lesscss.org/functions/`](http://lesscss.org/functions/)。

我们不会涵盖所有可用的函数，而是介绍来自不同类别的函数。函数可以根据其输入和输出进行分类。主要有以下几种类型的函数：

* 数学函数
* 颜色函数
* 列表函数
* 字符串函数
* 类型函数

Less 函数支持原生 JavaScript 函数，因为 Less 是用 JavaScript 编写的。目前，可以在 Less 代码中评估 JavaScript 表达式；不过，这在未来的版本中可能会被移除。

### 列表函数

此类别中的函数可用于迭代 CSV 列表。此类别包含两个主要函数——`extract()`和`length()`。顾名思义，`extract()`用于从 CSV 中获取值，`length()`用于检查它们的长度。

`extract` 函数在清单 4-70 中进行了演示，该示例使用`extract()`函数为社交媒体图标生成类。

**清单 4-70**. 演示列表函数用法的示例

```
@socials: "twitter","facebook","gplus";
.generateSocialIcons(@i) when (@i > 0) {
    .generateSocialIcons((@i - 1));
    @socialName: e(extract(@socials, @i));
    li.@{socialName}{
        width:16px;height:16px;
        background-image:url("@{socialName}.png");
    }
}
.generateSocialIcons(length(@socials));
```

此示例从名为`@socials`的列表中提取值，并使用该值生成特定的社交媒体类。输出如清单 4-71 所示。

**清单 4-71**. 清单 4-70 的输出

```
li.twitter {
  width: 16px;
  height: 16px;
  background-image: url("twitter.png");
}
li.facebook {
  width: 16px;
  height: 16px;
  background-image: url("facebook.png");
}
li.gplus {
  width: 16px;
  height: 16px;
  background-image: url("gplus.png");
}
```

### 颜色函数

Less 提供的颜色函数处理各种操作，如颜色定义、运算、混合和通道操作。

在 CSS2 中，颜色以 RGB 格式声明。RGB 由三个通道组成：红色、绿色和蓝色。随着 CSS3 的出现，出现了新的颜色定义，例如 HSL 和 HSV，它们类似于 RGB 值。Less 支持各种颜色定义方法和通道操作方法。这些颜色声明方法在清单 4-72 中进行了演示。

**清单 4-72**. 演示各种颜色声明技术的示例

```
.colorDefinition {
    color: rgb(255,133,75);
    color: rgba(255,133,75,.4);
    color: hsl(76, 80%, 20%);
    color: red;
}
```

除了颜色声明函数之外，Less 还提供了两种方法——`darken()`和`lighten()`——可用于计算输入颜色和强度提供的深色或浅色变体。这在清单 4-73 中进行了演示。

**清单 4-73**. 演示`lighten`和`darken`函数的示例

```
@primaryColor: #00A;
@secondaryColor: #0A0;
.heading{
    color:lighten(@primaryColor,20%);
}
.impNotice{
    color:darken(@primaryColor,35%);
}
.successfulEvent{
    color:lighten(@secondaryColor,10%);
}
```



### 颜色函数

***清单 4-74***. *清单 4-73* 的输出结果

```
.heading {
  color: #1111ff;
}
.impNotice {
  color: #000000;
}
.successfulEvent {
  color: #00dd00;
}
```

在设计中使用高对比度是一个重要因素。高对比度不仅影响视障人士设计的可用性，对视力正常的人也有好处。然而，计算正确的对比度值并不容易。Less 提供了`contrast()`函数，该函数计算一个易于识别的颜色。

`contrast()`函数最多可以接受四个参数，其中只有第一个参数是必需的。

*   将需要对比度的颜色定义为首个参数。
*   第二个和第三个参数定义了深色和浅色。
*   最后一个可输入参数是阈值。阈值的默认值是 43%。

高于阈值颜色的颜色被视为浅色，`contrast()`函数返回第二个参数中定义的深色。

其他提供的一些函数包括：

*   `saturate()`
*   `desaturate()`
*   `fadein()`
*   `fadeout()`
*   `fade()`
*   `spin()`
*   `mix()`
*   `grayscale()`

你可以通过应用颜色混合函数来生成新颜色，这些函数对提供的两种颜色输入执行基本操作，例如减法。

一些颜色混合函数包括：

*   `multiply()`
*   `screen()`
*   `average()`
*   `overlay()`
*   `hardlight()`
*   `softlight()`
*   `difference()`
*   `exclusion()`
*   `negation()`

让我们来看一些这些混合函数。

*   `multiply()` 对两种颜色执行乘法运算。
*   `screen()` 得出输入颜色中最亮的颜色。
*   `overlay()` 是`screen()`和`multiply()`函数的组合，结果颜色的亮度或暗度取决于第一个输入颜色的值。
*   `softlight()` 类似于`overlay()`，但它避免在其结果中出现纯黑色或纯白色。
*   `hardlight()` 与`overlay()`完全相反，结果颜色的亮度或暗度由第二个输入颜色的值决定。
*   `difference()` 按照每个通道从第一种颜色中减去第二种颜色。如果任何特定通道遇到负值，它将会被反转。
*   `average()` 按照每个通道计算两个数字的平均值。

所有这些函数已在 清单 4-75 中演示。

***清单 4-75***. 展示各种颜色混合函数的示例

```
@primaryColor: #00A;
@secondaryColor: #0A0;
@tertiaryColor: #AAA;
.heading{
    color:multiply(@primaryColor,@tertiaryColor);
    color:screen(@primaryColor,@secondaryColor);
    color:overlay(@primaryColor,@tertiaryColor);
    color:softlight(@secondaryColor,@tertiaryColor);
    color:hardlight(@primaryColor,@tertiaryColor);
    color:difference(@primaryColor,@secondaryColor);
    color:average(@primaryColor,@secondaryColor);
}
```

***清单 4-76***. 清单 4-75 的输出结果

```
.heading {
  color: #000071;
  color: #0000c6;
  color: #00b700;
  color: #5555c6;
  color: #00aaaa;
  color: #005555;
}
```

### 类型函数

Less 提供了类型函数来验证输入值的类型。如果输入类型与函数的测试匹配，这些函数返回`true`。

以下是一些类型函数：

*   `isnumber()`
*   `isstring()`
*   `iscolor()`
*   `iskeyword()`
*   `isurl()`
*   `ispixel()`
*   `isem()`
*   `ispercentage()`
*   `isunit()`

如果输入值是数字，`isnumber()`返回`true`；否则返回`false`。

***清单 4-77***. 对`isnumber`返回`true`或`false`的示例值

```
isnumber(#fff);     // false
isnumber("NormalText"); // false
isnumber(1234);     // true
isnumber(56px);     // true
```

如果输入值是字符串，`isstring()`返回`true`；否则返回`false`。

***清单 4-78***. 对`isstring`返回`true`或`false`的示例值

```
isstring(red);     // false
isstring("NormalText"); // true
```

如果输入值是颜色，`iscolor()`返回`true`；否则返回`false`。

***清单 4-79***. 对`iscolor`返回`true`或`false`的示例值

```
iscolor(#fff);     // true
iscolor("NormalText"); // false
```

如果输入值是 URL，`isurl()`返回`true`；否则返回`false`。

***清单 4-80***. 对`isurl`返回`true`或`false`的示例值

```
isurl("NormalText"); // false
isurl(url(...)); // true
```

如果输入值是像素单位数字，`ispixel()`返回`true`；否则返回`false`。

***清单 4-81***. 对`ispixel`返回`true`或`false`的示例值

```
ispixel(1234);     // false
ispixel(56px);     // true
```

如果输入值是`em`单位数字，`isem()`返回`true`；否则返回`false`。

***清单 4-82***. 对`isem`返回`true`或`false`的示例值

```
isem(56px);     // false
isem(7.8em);    // true
```

如果输入值是百分比值，`ispercentage()`返回`true`；否则返回`false`。

***清单 4-83***. 对`ispercentage`返回`true`或`false`的示例值

```
ispercentage(56px);     // false
ispercentage(7.8%);     // true
```

如果输入值的单位与第二个参数中指定的单位相同，`isunit()`返回`true`；否则返回`false`。

***清单 4-84***. 对`isunit`返回`true`或`false`的示例值

```
isunit(11px, px);  // true
isunit(2.2%, px);  // false
```

### 数学函数

Less 提供了几个数学函数来对值执行各种数学运算。在本节中，我们将介绍其中一些函数。

*   `ceil()` 类似于 JavaScript 中的版本，它会将输入数字舍入为最高的整数值。例如，`ceil(2.4)`返回`3`。
*   `floor()` 将输入数字舍入为最低的整数值。例如，`floor(2.4)`返回`2`。
*   `percentage()` 将输入数字转换为百分比值。例如，`percentage(0.5)`返回`50%`。
*   `round()` 对数字进行四舍五入。它接受两个参数。第一个参数是要舍入的数字。第二个参数是要舍入到的小数位数。例如，`round(1.67)`返回`2`，而`round(1.67,1)`将返回`1.7`。
*   `sqrt()` 计算输入值的平方根。另请注意，在执行此计算时，该函数会保留原始输入的单位。例如，`sqrt(25cm)`返回`5cm`。
*   `pow()` 返回第一个参数的第二个参数次幂的值。例如，`pow(2,2)`返回`4`。
*   `mod()` 计算输入值的模数。例如，`mod(11,6)`返回`5`。
*   `min()` 返回输入值中的最小值。例如，`min(2,1)`将返回`1`。
*   `max()` 返回输入值中的最大值。例如，`max(2,1)`将返回`2`。

### 字符串函数

Less 提供了字符串函数来操作字符串。在本节中，我们将研究这些函数。

*   `escape()` 将对字符串中发现的特殊字符进行 URL 编码。此函数不会对以下字符集进行 URL 编码：`,`, `/`, `?`, `@`, `&`, `+`, `'`, `~`, `!`, 和`$`。例如，`escape(x=10)`返回`x%3D10`。
*   `e()` 也称为 CSS 转义。它主要从输入字符串中移除引号，并返回不带引号的字符串，可用于 CSS 表达式中。我们在 清单 4-70 中使用了此函数来提取社交图标的名称并移除了其引号。
*   `replace()` 用于替换给定输入中的字符串。此函数接受四个参数：
    *   输入字符串，这是执行搜索和替换的目标。
    *   要被替换的字符串/模式。
    *   替换字符串，即用于替换搜索模式的字符串。
    *   任何可选的正则表达式标志。

例如，`replace("Is this planet?", "planet\?", "earth");` 会将单词`planet`替换为`earth`。

### 杂项函数


好的，作为一名高级文档工程师和翻译员，我将严格遵循您的注意事项和示例格式，对给定的英文文本进行翻译。

以下是翻译后的中文 Markdown 文档：


除了前面提到的函数，Less 还有一些无法归类到特定组的函数，如下所示：

- `color()` 将一个包含颜色值的字符串转换为实际的颜色值。这与字符串类别的 `e()` 函数类似。例如，`color("#555")` 返回 `#555`。
- `image-size()` 返回输入图像的尺寸。例如，`image-size("image.png")` 返回 `16px 16px`。这可以拆分为两个独立的函数：`image-width()` 和 `image-height()`。顾名思义，前者返回图像的宽度，后者返回图像的高度。
- `convert()` 将输入值转换为第二个参数指定的特定单位。此函数接受两个参数：
    - 要转换的输入值，需带有适当的单位。
    - 一个有效的单位类型。

例如，以下 `convert()` 函数调用 `convert(5s, "ms")` 返回 `5000ms`。请注意，在调用此函数时，输入单位和预期的输出单位应该是兼容的。例如，`convert(10, ms)` 只会返回 `10`，这不是预期的结果，因为输入单位（此处没有单位）与输出类型不兼容。兼容意味着单位应代表相同类型的属性。

- `default()` 与守卫（`guard`）一起使用。当根据输入找不到匹配的混合（`mixin`）时，它用于触发守卫。这在清单 4-85 中演示。

***清单 4-85***. 演示带有守卫的 `default()` 函数的示例

```
.font(1){
    font-size: 11px;
}
.font(5){
    font-size: 26px;
}
.font(@size) when (default()) {
    font-size: ~"@{size}px";
}

.header {
  .font(5);
}

.content {
  .font(12);
}
```

在此示例中，你有三个 `font` 混合的变体，如果输入值为 `1` 或 `5`，它会抛出一个预设值。对于其他情况，`default()` 返回 `true`，此时直接使用提供的值。此代码的输出如清单 4-86 所示。

***清单 4-86***. 清单 4-85 的输出

```
.header {
  font-size: 26px;
}
.content {
  font-size: 12px;
}
```

- `unit()` 的行为因传递的参数数量而异。此函数可以接受两个参数。如果只提供一个参数，它将从输入中提取并返回数值。例如，`unit(10px)` 返回 `10`。当你想要为输入值分配某个单位时，传递两个参数。例如，`unit(10,px)` 返回 `10px`。请注意，这不能与 `convert()` 函数互换使用。
- `get-unit()` 用于只从给定值中提取单位。如果输入有任何单位，将仅返回该单位。

## 将 Less 与外部框架一起使用

你可以将 Less 与其他知名框架一起使用。本节介绍如何使用 Less 与以下框架进行自定义或扩展：

- Cardinal CSS
- Ionic 框架
- Semantic Grid System

### Cardinal CSS

Cardinal ([`cardinalcss.com/`](http://cardinalcss.com/)) 是一个类似 Bootstrap 的 CSS 框架。它是一个纯 CSS 框架，本质上是移动优先和模块化的。它不包含任何类似 Bootstrap 或 Foundation 的 JavaScript 插件。Cardinal 的 CSS 是使用 Less 编写的。

虽然这个框架制作精良，但其文档需要大量改进。你会在 Less 代码框架中找到许多有用的注释。

使用此框架的 HTML 标记应类似于清单 4-87。

***清单 4-87***. 使用 Cardinal 框架的示例标记

```
<div class="grid">
  <div class="grid-item 1/4">Block 1</div>
  <div class="grid-item 1/4">Block 2</div>
  <div class="grid-item 1/4">Block 3</div>
  <div class="grid-item 1/4">Block 4</div>
</div>
```

Cardinal 支持七种由六个断点组成的网格类型，如下所示：

- `xs`: 480px;
- `sm`: 600px;
- `md`: 768px;
- `lg`: 960px;
- `xl`: 1140px;
- `xxl`: 1380px;

Less Cardinal 插件会在你的自定义代码之前导入 Cardinal 的 Less 代码。此插件可以通过执行以下命令安装：

```
npm install -g less-plugin-cardinal
```

安装后，可以使用以下命令使用该插件：

```
lessc mycode.less --cardinal
```

### Ionic 和 Less

Ionic ([`www.ionicframework.com`](http://www.ionicframework.com)) 框架用于构建混合移动应用程序，这种应用就像以原生应用方式运行的 Web 应用。由于这些应用由网页构成，因此可以使用 HTML、JavaScript 和 CSS 等 Web 技术创建。Ionic 应用是使用 HTML5 构建的。

Ionic 的 Less 插件可在 [`github.com/bassjobsen/lessplugin-ionic`](https://github.com/bassjobsen/lessplugin-ionic) 获得。它会在你的自定义 Less 代码之前导入 Ionic 框架代码。安装插件后，你可以使用以下命令简单地编译代码：

```
lessc apllication.less application.css –ionic
```

此插件可以通过运行以下命令安装：

```
npm install -g less-plugin-ionic
```

### Semantic Grid System

Semantic Grid System ([`github.com/tylertate/semantic.gs`](https://github.com/tylertate/semantic.gs)) 是一种实现响应式布局的现代且简洁的方法，标记中没有任何无语义的类（`class`）。你只需设置列宽和间距宽，设置列数，然后在像素和百分比之间进行选择。它主要是用 Less 创建的，但也支持 SCSS 和 Stylus。

这在清单 4-88 中演示。

***清单 4-88***. 演示 Semantic Grid System 的示例

```
@import 'grid.less';
@columns: 12;
@column-width: 50;
@gutter-width: 10;
@total-width: 100%;
.main {
    .column(9);
}
.sidebar {
    .column(3);
}
```

## 总结

本章介绍了另一个著名的预编译器框架——Less。本章涵盖了它的各种语言特性，包括嵌套、混合及其守卫，以及此框架提供的各种函数类别。

在下一章中，你将了解另一个名为 Compass 的著名框架。

---

