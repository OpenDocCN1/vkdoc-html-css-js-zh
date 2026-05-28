# 4. 属性选择器

属性选择器允许根据元素的属性及其属性值向元素添加样式。

## 属性选择器

属性选择器将匹配使用了指定属性的元素，无论其属性值为何。

```
input[type] {}
```

此选择器仅匹配使用了 `type` 属性的 `input` 元素，例如以下元素：

```
<input type="text">
```

## 属性值选择器

`[attribute=value]` 选择器将同时根据属性和属性值进行匹配。

```
input[type="submit"] {}
```

那些 `type` 属性设置为 `submit` 的 `input` 元素将匹配此规则，如下例所示：

```
<input type="submit">
```

## 语言属性选择器

语言属性选择器用于匹配 `lang` 属性。

```
p[lang|="en"] {}
```

此选择器将匹配任何 `lang` 属性值以“en”开头的元素，例如“en-US”。请注意，此类语言代码不区分大小写。

```
<p lang="en">英语</p>
<p lang="en-US">美式英语</p>
```

## 分隔值选择器

`[attribute~=value]` 选择器将应用于其属性值在以空格分隔的单词列表中包含给定单词的元素。

```
input[value~="word"] {}
```

此规则将选中以下两个元素。该单词需要大小写完全匹配；例如，该选择器不会定位“Word”或“words”。

```
<input type="text" value="word">
<input type="text" value="word word2">
```

## 值子串选择器

`[attribute*=value]` 选择器匹配属性值包含指定子串的元素。

```
p[title*="para"] {}
```

`title` 属性包含“para”的段落元素将匹配此规则。

```
<p title="my paragraph"></p>
```

## 值开头选择器

`[attribute^=value]` 选择器匹配每个属性值以指定字符串开头的元素。

```
p[title^="first"] {}
```

`title` 属性值以“first”开头的段落将应用此规则。

```
<p title="first paragraph"></p>
```

## 值结尾选择器

`[attribute$=value]` 选择器匹配属性值以指定字符串结尾的元素。

```
p[title$="1"] {}
```

在以下代码中，`title` 属性的值以“1”结尾，因此将匹配此规则：

```
<p title="paragraph 1"></p>
```