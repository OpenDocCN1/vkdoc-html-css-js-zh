# 可见性（visibility）

`visibility`属性可以通过将其值设置为`hidden`来隐藏元素，而无需移除其占用的空间。

`visibility (block) : inherit | visible | hidden | collapse`

`collapse`值旨在仅用于某些表格元素：行（`<tr>`）、列（`<col>`）、列组（`<colgroup>`）以及行组（`<thead>`、`<tbody>`和`<tfoot>`）。根据规范，`collapse`应移除隐藏的元素（如同`display: none`）并使该空间可供其他元素使用。遗憾的是，并非所有主流浏览器都遵循此值的规范。将`display`属性设置为`none`会带来更一致的浏览器行为，因此应优先使用。