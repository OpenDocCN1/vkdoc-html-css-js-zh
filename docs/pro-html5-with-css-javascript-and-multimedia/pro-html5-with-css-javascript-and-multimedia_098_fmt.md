# 第 24 章 拖放

## 轮流移动

您可能已经注意到，该应用程序并未强制玩家交替回合。现在您将实现这一逻辑。每次移动完成后（处理`drop`事件），您将为刚刚移动的颜色的所有棋子的`draggable`属性设置为`false`。这将防止您移动相同颜色的棋子。不过，这条规则有一个例外情况需要额外处理。如果您跳过了某个棋子，那么只要该棋子能再次进行跳跃，它就可以继续移动。

您将首先实现通用规则。这将通过创建一个名为`enableNextPlayer()`的新函数来完成。该函数将使用`querySelectorAll()`函数获取所有`img`元素。`draggable`属性将根据`id`前缀设置为`true`或`false`。稍后您将添加处理跳跃条件的特殊逻辑。

**练习 24-7. 轮流**

1. 在`Checkers.js`脚本中添加`enableNextPlayer()`函数。

```javascript
function enableNextPlayer(piece) {
  // Get all of the pieces
  var pieces = document.querySelectorAll('img');
  var i = 0;
  while (i < pieces.length) {
    var p = pieces[i++];
    // If this is the same color that just moved, disable dragging
    if (p.id.substr(0, 1).toUpperCase() ===
        piece.id.substr(0, 1).toUpperCase()) {
      p.draggable = false;
    }
    // Otherwise, enable dragging
    else {
      p.draggable = true;
    }
  }
}
```

2. 在`isValidMove()`函数的末尾，添加加粗显示的代码。这将在执行拖放时调用`enableNextPlayer()`函数。

```javascript
// Set the draggable attribute so the next player can take a turn
if (drop) {
  enableNextPlayer(source);
}
return true;
```

**注意**：通常将此调用放在`drop()`函数中可能更合理。然而，只有`isValidMove()`函数知道发生了跳跃，您需要在此处添加覆盖逻辑。这需要在应用通用规则之后进行。

3. `drop()`函数会创建一个新的`img`元素，并当前将其`draggable`属性设置为`true`。现在您需要根据现有棋子的`draggable`属性来使其具有条件性。将`drop()`函数中的现有行替换为以下加粗显示的代码：

```javascript
// Create a new img on the target location
var newPiece = document.createElement("img");
newPiece.src = droppedPiece.src;
newPiece.id = droppedPiece.id.substr(0, 1) + e.target.id;
newPiece.draggable = droppedPiece.draggable;
newPiece.classList.add("piece");
newPiece.addEventListener("dragstart", dragStart, false);
newPiece.addEventListener("dragend", dragEnd, false);
e.target.appendChild(newPiece);
```

4. 现在你需要修改 `dragStart` 事件处理程序，使其在元素不可拖动时忽略该事件。将以下以**粗体**显示的代码添加到 `dragStart()` 函数中：

```javascript
function dragStart(e) {
  if (e.target.draggable) {
    e.dataTransfer.effectAllowed = "move";
    e.dataTransfer.setData("text/plain", e.target.id);
    e.target.classList.add("selected");
  }
}
```

现在你需要实现特殊的跳跃逻辑。如果棋子刚刚完成了一次跳跃，你需要将 `draggable` 属性重新设为 `true`，使其可以再次移动。不过，你还需要将 `jumpOnly` 类添加到 `classList` 中，从而强制规定其唯一允许的移动是另一次跳跃。

5. 将以下以**粗体**显示的代码添加到 `isValidMove()` 函数中。这段代码会在 `classList` 中查找 `jumpOnly`，并相应设置 `jumpOnly` 标志。

```javascript
var jumpOnly = false;
if (source.classList.contains("jumpOnly")) {
  jumpOnly = true;
}

// 计算 x 和 y 坐标
var xStart = parseInt(startPos.substr(0, 1));
var yStart = parseInt(startPos.substr(1, 1));
```

6. 现在，将以下以**粗体**显示的代码添加到 `isValidMove()` 函数中。第一部分添加了一条规则，确保当 `jumpOnly` 为 `true` 时，必须执行跳跃动作。第二部分设置 `jumped` 标志，表明此移动是一次跳跃。

```javascript
// 以下规则适用于所有棋子
if (yStart === yEnd || xStart === xEnd)
  return false; // 移动必须是对角线方向

// 不能移动超过两格
if (Math.abs(yEnd - yStart) > 2 || Math.abs(xEnd - xStart) > 2)
  return false;

// 只允许跳跃
if (Math.abs(xEnd - xStart) === 1 && jumpOnly)
  return false;

var jumped = false;

// 如果移动两格，找到被跳过的格子
if (Math.abs(xEnd - xStart) === 2) {
  var pos = ((xStart + xEnd) / 2).toString() +
            ((yStart + yEnd) / 2).toString();
  var div = document.getElementById(pos);
  if (div.childElementCount === 0)
    return false; // 不能跳过空格
  var img = div.children[0];
  if (img.id.substr(0, 1).toLowerCase() === prefix.toLowerCase())
    return false; // 不能跳过己方棋子

  // 如果此函数是从 drop 事件调用的
  // 移除被跳过的棋子
  if (drop) {
    div.removeChild(img);
    jumped = true;
  }
}
```

7. 在 `isValidMove()` 函数的末尾，添加以下以**粗体**显示的代码。如果进行了跳跃，这段代码会覆盖 `draggable` 属性，并将 `jumpOnly` 添加到 `classList` 中。

```javascript
if (drop) {
  enableNextPlayer(source);
  // 如果我们跳过了一个棋子，允许再次行动
  if (jumped) {
    source.draggable = true;
    source.classList.add("jumpOnly"); // 但仅限于再次跳跃
  }
}
```

> **注：** `enableNextPlayer()` 函数禁用了当前玩家的所有棋子，并启用对方玩家的棋子。随后这段代码启用了刚刚完成跳跃的棋子。因此，两者都处于启用状态：该棋子可以再次跳跃，或者对方玩家可以移动。这两种情况都是有效的，所以我们需要同时允许它们。

8. 修改 `drop()` 函数，在创建新的 `img` 元素时，通过添加以下以**粗体**显示的代码，也将 `jumpOnly` 添加到 `classList` 中。

```
// 在目标位置创建一个新的 img
var newPiece = document.createElement("img");
newPiece.src = droppedPiece.src;
newPiece.id = droppedPiece.id.substr(0, 1) + e.target.id;
newPiece.draggable = droppedPiece.draggable;
if (droppedPiece.draggable){
  newPiece.classList.add("jumpOnly");
}
newPiece.classList.add("piece");
```

9. 现在，你需要在下一步移动完成时，从 `classList` 中清除 `jumpOnly`。你将在 `enableNextPlayer()` 函数中通过添加以下以`粗体`显示的代码来完成此操作。

```
function enableNextPlayer(piece) {
  // 获取所有棋子
  var pieces = document.querySelectorAll('img');
  var i = 0;
  while (i < pieces.length) {
    var p = pieces[i++];
    // 如果这是刚移动过的同色棋子，则禁用拖放
```