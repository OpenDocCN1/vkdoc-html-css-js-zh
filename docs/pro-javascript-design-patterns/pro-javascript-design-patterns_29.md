# 另一个示例

```
if (this.isbn.length === 13 && this.isbn.match(/^[0-9]{13}$/)) {
    return isbn;
}
else {
    throw new Error("invalid ISBN");
}
```

## 更多示例

```
for (var i = 0; i < 10; i++) {
    sum += isbn.charAt(i) * (10 - i);
}
if (sum % 11 === 0) {
    return true;
}
else {
    return false;
}
```

---

```
for (var i = 0; i < 13; i++) {
    sum += isbn.charAt(i) * (i % 2 === 0 ? 1 : 3);
}
if (sum % 10 === 0) {
    return true;
}
else {
    return false;
}
```

---

```
return true;
// All tests passed.
```

---

