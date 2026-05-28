# 第 25 章 ■ 索引数据库

## 配置棋子

你将使用图像文件来表示国际象棋棋子。在将它们添加到画布之前，需要为每个棋子创建一个 `Image` 对象，并指定其 `src` 属性。你还需要将这些对象放入一个数组中，以便更轻松地通过编程方式选择所需的图像。

此外，你需要定义将加载到对象存储中的数据。`pieceTypes[]` 数组定义了显示每个棋子所需的属性，例如高度和宽度。它还指定了 `images[]` 数组中黑子和白子图像对应的索引。`pieces[]` 数组包含上一章中使用的相同详细信息，例如行和列，并定义了 32 个棋子的起始位置。

将清单 25-2 中显示的代码添加到新的 `Data.js` 脚本文件中。此文件也包含在源代码下载中，因此你可以直接将其复制到你的 Web 项目中，而无需重新输入这些数据。

**清单 25-2.** 定义静态数据

```
// 定义象棋棋子图像
var imgPawn = new Image();
var imgRook = new Image();
var imgKnight = new Image();
var imgBishop = new Image();
var imgQueen = new Image();
var imgKing = new Image();
var imgPawnW = new Image();
var imgRookW = new Image();
var imgKnightW = new Image();
var imgBishopW = new Image();
var imgQueenW = new Image();
var imgKingW = new Image();

// 指定每个图像的源文件
imgPawn.src = "images/pawn.png";
imgRook.src = "images/rook.png";
imgKnight.src = "images/knight.png";
imgBishop.src = "images/bishop.png";
imgQueen.src = "images/queen.png";
imgKing.src = "images/king.png";
imgPawnW.src = "images/wpawn.png";
imgRookW.src = "images/wrook.png";
imgKnightW.src = "images/wknight.png";
imgBishopW.src = "images/wbishop.png";
imgQueenW.src = "images/wqueen.png";
imgKingW.src = "images/wking.png";

// 定义 Image 对象的数组
var images = [
  imgPawn,
  imgRook,
  imgKnight,
  imgBishop,
  imgQueen,
  imgKing,
  imgPawnW,
  imgRookW,
  imgKnightW,
  imgBishopW,
  imgQueenW,
  imgKingW
];

var pieceTypes = [
  { name: "pawn", height: "50", width: "28", blackImage: 0, whiteImage: 6 },
  { name: "rook", height: "60", width: "36", blackImage: 1, whiteImage: 7 },
  { name: "knight", height: "60", width: "36", blackImage: 2, whiteImage: 8 },
  { name: "bishop", height: "65", width: "30", blackImage: 3, whiteImage: 9 },
  { name: "queen", height: "70", width: "32", blackImage: 4, whiteImage: 10 },
  { name: "king", height: "70", width: "28", blackImage: 5, whiteImage: 11 }
];

var pieces = [
  { type: "pawn", color: "white", row: 6, column: 0, pos: "a2", killed: false },
  { type: "pawn", color: "white", row: 6, column: 1, pos: "b2", killed: false },
  { type: "pawn", color: "white", row: 6, column: 2, pos: "c2", killed: false },
  { type: "pawn", color: "white", row: 6, column: 3, pos: "d2", killed: false },
  { type: "pawn", color: "white", row: 6, column: 4, pos: "e2", killed: false },
  { type: "pawn", color: "white", row: 6, column: 5, pos: "f2", killed: false },
  { type: "pawn", color: "white", row: 6, column: 6, pos: "g2", killed: false },
  { type: "pawn", color: "white", row: 6, column: 7, pos: "h2", killed: false },
  { type: "rook", color: "white", row: 7, column: 0, pos: "a1", killed: false },
  { type: "rook", color: "white", row: 7, column: 7, pos: "h1", killed: false },
];
```

```json
{
  "type": "knight",
  "color": "white",
  "row": 7,
  "column": 1,
  "pos": "b12",
  "killed": false
},
{
  "type": "knight",
  "color": "white",
  "row": 7,
  "column": 6,
  "pos": "g1",
  "killed": false
},
{
  "type": "bishop",
  "color": "white",
  "row": 7,
  "column": 2,
  "pos": "c1",
  "killed": false
},
{
  "type": "bishop",
  "color": "white",
  "row": 7,
  "column": 5,
  "pos": "f1",
  "killed": false
},
{
  "type": "queen",
  "color": "white",
  "row": 7,
  "column": 3,
  "pos": "d1",
  "killed": false
},
{
  "type": "king",
  "color": "white",
  "row": 7,
  "column": 4,
  "pos": "e1",
  "killed": false
},
{
  "type": "pawn",
  "color": "black",
  "row": 1,
  "column": 0,
  "pos": "a7",
  "killed": false
},
{
  "type": "pawn",
  "color": "black",
  "row": 1,
  "column": 1,
  "pos": "b7",
  "killed": false
},
{
  "type": "pawn",
  "color": "black",
  "row": 1,
  "column": 2,
  "pos": "c7",
  "killed": false
},
{
  "type": "pawn",
  "color": "black",
  "row": 1,
  "column": 3,
  "pos": "d7",
  "killed": false
},
{
  "type": "pawn",
  "color": "black",
  "row": 1,
  "column": 4,
  "pos": "e7",
  "killed": false
},
{
  "type": "pawn",
  "color": "black",
  "row": 1,
  "column": 5,
  "pos": "f7",
  "killed": false
},
{
  "type": "pawn",
  "color": "black",
  "row": 1,
  "column": 6,
  "pos": "g7",
  "killed": false
},
{
  "type": "pawn",
  "color": "black",
  "row": 1,
  "column": 7,
  "pos": "h7",
  "killed": false
},
{
  "type": "rook",
  "color": "black",
  "row": 0,
  "column": 0,
  "pos": "a8",
  "killed": false
},
{
  "type": "rook",
  "color": "black",
  "row": 0,
  "column": 7,
  "pos": "h8",
  "killed": false
},
{
  "type": "knight",
  "color": "black",
  "row": 0,
  "column": 1,
  "pos": "b8",
  "killed": false
},
{
  "type": "knight",
  "color": "black",
  "row": 0,
  "column": 6,
  "pos": "g8",
  "killed": false
},
{
  "type": "bishop",
  "color": "black",
  "row": 0,
  "column": 2,
  "pos": "c8",
  "killed": false
},
{
  "type": "bishop",
  "color": "black",
  "row": 0,
  "column": 5,
  "pos": "f8",
  "killed": false
},
{
  "type": "queen",
  "color": "black",
  "row": 0,
  "column": 3,
  "pos": "d8",
  "killed": false
},
{
  "type": "king",
  "color": "black",
  "row": 0,
  "column": 4,
  "pos": "e8",
  "killed": false
}
```