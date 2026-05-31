# 代码片段分析

```javascript
if (p.id.substr(0, 1).toUpperCase() ===
    piece.id.substr(0, 1).toUpperCase()) {
    p.draggable = false;
}
// Otherwise, enable dragging
else {
    p.draggable = true;
}
p.classList.remove("jumpOnly");
```

10. 现在测试应用，确保每位玩家必须交替回合。同时，验证你可以连续跳跃。

