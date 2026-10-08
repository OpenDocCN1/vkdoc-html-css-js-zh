# 附录 B：拖放源代码

清单 B-1 包含了第 24 章演示的拖放应用程序的最终 JavaScript 实现。

**清单 B-1.** 第 24 章源代码

```javascript
"use strict";

function createBoard() {
    var board = document.getElementById("board");
    for (var y=0; y < 8; y++) {
        var row = document.createElement("div");
        row.className = "row";
        board.appendChild(row);
        for (var x=0; x < 8; x++) {
            var square = document.createElement("div");
            square.id = x.toFixed() + y.toString();
            if ((x + y) % 2) {
                square.className = "bblack";
            } else {
                square.className = "bwhite";
            }
            // 如果方格中应该有一个棋子...
            if ((x + y) % 2 != 0 && y != 3 && y != 4) {
                var img = document.createElement("img");
                if (y < 3) {
                    img.id = "w" +square.id;
                    img.src = "Images/WhitePiece.png";
                } else {
                    img.id = "b" + square.id;
                    img.src = "Images/BlackPiece.png";
                }
                img.className = "piece";
                img.setAttribute("draggable", "true");
                square.appendChild(img);
            }
            square.setAttribute("draggable", "false");
            row.appendChild(square);
        }
    }
}

function allowDrop() {
    // 为所有黑色方块绑定目标事件
    var squares = document.querySelectorAll('.bblack');
    var i = 0;
    while (i < squares.length) {
        var s = squares[i++];
        // 添加事件监听器
        s.addEventListener('dragover', dragOver, false);
        s.addEventListener('drop', drop, false);
        s.addEventListener('dragenter', dragEnter, false);
        s.addEventListener('dragleave', dragLeave, false);
    }
    // 为所有图片绑定源事件
    i = 0;
    var pieces = document.querySelectorAll('img');
    while (i < pieces.length) {
        var p = pieces[i++];
        p.addEventListener('dragstart', dragStart, false);
        p.addEventListener('dragend', dragEnd, false);
    }
}
```

```javascript
createBoard();
allowDrop();
```

```javascript
// 处理 dragover 事件
function dragOver(e) {
    e.preventDefault();
    // 获取正在被拖动的 img 元素
    var dragID = e.dataTransfer.getData("text");
    var dragPiece = document.getElementById(dragID);
    // 变通方法 - 如果无法获取 dataTransfer，暂时不禁止移动，drop 事件会捕获此情况
    if (dragPiece) {
        if (e.target.tagName === "DIV" &&
            isValidMove(dragPiece, e.target, false)) {
            e.dataTransfer.dropEffect = "move";
        } else {
            e.dataTransfer.dropEffect = "none";
        }
    }
}
```

```javascript
// 处理 dragstart 事件
function dragStart(e) {
    if (e.target.draggable) {
        e.dataTransfer.effectAllowed = "move";
        e.dataTransfer.setData("text", e.target.id);
        e.target.classList.add("selected");
        // 使用自定义拖放图像
        var dragIcon = document.createElement("img");
        dragIcon.src = "Images/smiley.jpg";
        e.dataTransfer.setDragImage(dragIcon, 0, 0);
    }
}

// 处理 dragend 事件
function dragEnd(e) {
    e.target.classList.remove("selected");
}

// 处理 drop 事件
function drop(e) {
    e.stopPropagation();
    e.preventDefault();
    // 获取正在被拖动的 img 元素
    var droppedID = e.dataTransfer.getData("text");
    var droppedPiece = document.getElementById(droppedID);
    if (droppedPiece &&
        e.target.tagName === "DIV" &&
        isValidMove(droppedPiece, e.target, true)) {
        // 在目标位置创建一个新的 img
        var newPiece = document.createElement("img");
        newPiece.src = droppedPiece.src;
        newPiece.id = droppedPiece.id.substr(0, 1) + e.target.id;
        newPiece.draggable = droppedPiece.draggable;
        if (droppedPiece.draggable) {
            newPiece.classList.add("jumpOnly");
        }
        newPiece.classList.add("piece");
        newPiece.addEventListener("dragstart", dragStart, false);
        newPiece.addEventListener("dragend", dragEnd, false);
        e.target.appendChild(newPiece);
        // 移除之前的图像
        droppedPiece.parentNode.removeChild(droppedPiece);
        // 从目标元素移除放置效果
    }
}
```

当调用`getData()`函数时，假设请求成功，`responseData`对象将保存从服务器返回的数据。当然，您可能需要对数据进行一些处理。为此，`getResponse()`函数应调用另一个函数，该函数将使用刚刚提取的数据。

```markdown
e.target.classList.remove('drop');

// 检查棋子是否需要升变为王

kingMe(newPiece);

// 处理 dragenter 事件

function dragEnter(e) {

  // 获取正在拖拽的 img 元素

  var dragID = e.dataTransfer.getData("text");

  var dragPiece = document.getElementById(dragID);

  if (

    dragPiece &&

    e.target.tagName === "DIV" &&

    isValidMove(dragPiece, e.target, false)

  ) {

    e.target.classList.add('drop');

  }

}

// 处理 dragleave 事件

function dragLeave(e) {

  e.target.classList.remove("drop");

}

function isValidMove(source, target, drop) {

  // 获取棋子的前缀和位置

  var startPos = source.id.substr(1, 2);

  var prefix = source.id.substr(0, 1);

  // 获取目标位置，如果有前缀则去除

  var endPos = target.id;

  if (endPos.length > 2) {

    endPos = endPos.substr(1, 2);

  }

  // 不能放在当前位置

  if (startPos === endPos) {

    return false;

  }

  // 不能放在已有棋子的格子上

  if (target.childElementCount != 0) {

    return false;

  }

  var jumpOnly = false;

  if (source.classList.contains("jumpOnly")) {

    jumpOnly = true;

  }

  // 计算 x 和 y 坐标

  var xStart = parseInt(startPos.substr(0, 1));

  var yStart = parseInt(startPos.substr(1, 1));

  var xEnd = parseInt(endPos.substr(0, 1));

  var yEnd = parseInt(endPos.substr(1, 1));

  switch (prefix) {

    // 白棋规则

    case "w":

      if (yEnd <= yStart)

        return false; // 不能向后移动

      break;

    // 黑棋规则

    case "b":

      if (yEnd >= yStart)

        return false; // 不能向后移动

      break;

  }

  // 以下规则适用于所有棋子

  if (yStart === yEnd || xStart === xEnd)

    return false; // 移动必须是对角线方向

  // 不能移动超过两格

  if (Math.abs(yEnd - yStart) > 2 || Math.abs(xEnd - xStart) > 2)

    return false;

  // 仅允许跳跃移动

  if (Math.abs(xEnd - xStart) === 1 && jumpOnly)

    return false;

  var jumped = false;

  // 如果移动两格，找到被跳过的格子

  if (Math.abs(xEnd - xStart) === 2) {

    var pos =

      ((xStart + xEnd) / 2).toString() +

      ((yStart + yEnd) / 2).toString();

    var div = document.getElementById(pos);

    if (div.childElementCount === 0)

      return false; // 不能跳过空格子

    var img = div.children[0];

    if (img.id.substr(0, 1).toLowerCase() === prefix.toLowerCase())

      return false; // 不能跳过同色棋子

    // 如果该函数是从 drop 事件调用的

    // 移除被跳过的棋子

    if (drop) {

      div.removeChild(img);

      jumped = true;

    }

  }

  // 设置 draggable 属性，以便下一位玩家可以继续

  if (drop) {

    enableNextPlayer(source);

    // 如果跳过了棋子，允许再次行动

    if (jumped) {

      source.draggable = true;

      source.classList.add("jumpOnly"); // 但仅允许再次跳跃

    }

  }

  return true;

}

function kingMe(piece) {

  // 如果已经是王，直接返回

  if (piece.id.substr(0, 1) === "W" || piece.id.substr(0, 1) === "B")

    return;

  var newPiece;

  // 如果是位于第 7 行的白棋

  if (piece.id.substr(0, 1) === "w" && piece.id.substr(2, 1) === "7") {

    newPiece = document.createElement("img");

    newPiece.src = "Images/WhiteKing.png";

    newPiece.id = "W" + piece.id.substr(1, 2);

  }

  // 如果是位于第 0 行的黑棋

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

function enableNextPlayer(piece) {

  // 获取所有棋子

  var pieces = document.querySelectorAll('img');

  var i = 0;

  while (i < pieces.length) {

    var p = pieces[i++];

    // 如果是刚移动过的同一颜色，则禁用拖拽

    if (p.id.substr(0, 1).toUpperCase() ===

        piece.id.substr(0, 1).toUpperCase()) {

      p.draggable = false;

    }

    // 否则，启用拖拽

    else {

      p.draggable = true;

    }

    p.classList.remove("jumpOnly");

  }

}
```
