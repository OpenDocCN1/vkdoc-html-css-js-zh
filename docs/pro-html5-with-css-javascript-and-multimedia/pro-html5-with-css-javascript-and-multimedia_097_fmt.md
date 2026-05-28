# 第 24 章 拖放

2. 修改`drop()`函数，在拖放完成后调用`kingMe()`函数，添加加粗显示的代码行。

```javascript
// Remove the previous image
droppedPiece.parentNode.removeChild(droppedPiece);
// Remove the drop effect from the target element
e.target.classList.remove('drop');
// See if the piece needs to be promoted
kingMe(newPiece);
```

**提示**：在实现`isValidMove()`函数时，防止棋子后退的规则仅适用于`b`和`w`前缀。由于王拥有大写字母`B`或`W`，因此该规则不适用，王可以后退。此外，在跳棋时，比较操作是在转换为小写后进行的。

这将允许白棋跳过黑棋或黑王。

3. 尝试移动棋子，直到将棋子移动到最后一行。您应该会看到图像发生变化，表示该棋子已成为王，如图 24-5 所示。

***图 24-5.** 带有王的棋盘*

4. 拥有王之后，尝试让其后退，并尝试使用它跳棋。