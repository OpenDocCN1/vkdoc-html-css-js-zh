# 注意

截至撰写本文时，Chrome、IE 和 Opera 不允许在 `dragEnter` 和 `dragOver` 事件中访问 `dataTransfer` 对象。不过，这在 `drop` 事件中是可行的。在 `dragOver` 事件中的解决方法是：如果源对象不可用，则允许移动操作。游戏仍然可以工作，因为 `drop` 事件会忽略任何无效移动，但用户体验并不理想。`dragEnter` 事件用于应用 `drop` 类以实现样式效果，但这也无法正常工作。在接下来的章节中，我将使用 Firefox 来测试应用程序。

## 3. 将 `dragEnter()` 函数的实现替换为以下代码

这段代码与 `dragOver()` 函数基本相同，区别在于它向元素添加了 `drop` 类，而不是设置 `dropEffect`。

```
function dragEnter(e) {
  // 获取正在拖动的图片元素
  var dragID = e.dataTransfer.getData("text");
  var dragPiece = document.getElementById(dragID);

  if (dragPiece &&
      e.target.tagName === "DIV" &&
      isValidMove(dragPiece, e.target, false)) {
    e.target.classList.add('drop');
  }
}
```

## 4. 对于 `drop()` 函数，通过添加以粗体显示的代码，将执行放置操作的代码包裹在验证移动的 `if` 语句中。这次，代码向 `isValidMove()` 函数传递了 `true` 作为第三个参数。

```
if (droppedPiece &&
    e.target.tagName === "DIV" &&
    isValidMove(droppedPiece, e.target, true)) {
  // 在目标位置创建新的图片
  var newPiece = document.createElement("img");
  newPiece.src = droppedPiece.src;
  newPiece.id = droppedPiece.id.substr(0, 1) + e.target.id;
  newPiece.draggable = true;
  newPiece.classList.add("piece");
  newPiece.addEventListener("dragstart", dragStart, false);
  newPiece.addEventListener("dragend", dragEnd, false);
  e.target.appendChild(newPiece);
  // 移除之前的图片
  droppedPiece.parentNode.removeChild(droppedPiece);
  // 从目标元素移除 drop 效果
  e.target.classList.remove('drop');
}
```

## 5. 现在应用这些更改后，尝试运行应用程序。您应该只能进行合法的移动。如果您跳过一个棋子，它应该从棋盘上移除。

#### 升级为王

在跳棋中，当一个棋子一路移动到最后一排时，它会被升级为王。王的玩法与普通棋子相同，区别在于它可以向后移动。现在，您将添加代码来检查一个棋子是否需要被升级。要升级一个棋子，您将更改显示的图像以表示它是王。您还将更改前缀，使其成为大写字母 B 或 W。然后，您可以为王设置不同的规则。

您将把所有逻辑放在一个名为 `kingMe()` 的函数中，并在每次放置操作发生时调用它。

如果棋子已经是王，或者它不在最后一排，该函数将直接返回。否则，它将执行升级操作。

## 练习 24-6：添加升级功能

### 1. 将 `kingMe()` 函数添加到 `Checkers.js` 脚本中。

```
function kingMe(piece) {
  // 如果已经是王，则直接返回
  if (piece.id.substr(0, 1) === "W" || piece.id.substr(0, 1) === "B")
    return;

  var newPiece;

  // 如果是白棋且位于第 7 行
  if (piece.id.substr(0, 1) === "w" && piece.id.substr(2, 1) === "7") {
    newPiece = document.createElement("img");
    newPiece.src = "Images/WhiteKing.png";
    newPiece.id = "W" + piece.id.substr(1, 2);
  }

  // 如果是黑棋且位于第 0 行
  if (piece.id.substr(0, 1) === "b" && piece.id.substr(2, 1) === "0") {
    var newPiece = document.createElement("img");
    newPiece.src = "Images/BlackKing.png";
    newPiece.id = "B" + piece.id.substr(1, 2);
  }

  // 如果创建了新棋子，设置其属性和事件
  if (newPiece) {
    newPiece.draggable = true;
    newPiece.classList.add("piece");
    newPiece.addEventListener('dragstart', dragStart, false);
    newPiece.addEventListener('dragend', dragEnd, false);
    var parent = piece.parentNode;
    parent.removeChild(piece);
    parent.appendChild(newPiece);
  }
}
```



