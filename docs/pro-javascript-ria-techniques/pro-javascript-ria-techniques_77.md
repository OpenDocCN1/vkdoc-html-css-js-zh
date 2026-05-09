# 第 10 章 ■ 二进制 Ajax

```javascript
// 在十进制格式中，负数表示南半球赤道以下的点
href.push(gpsData.LatitudeRef == "S" ? "- " : "");
href.push(gpsData.Latitude.decimal);
href.push(",");

// 在十进制格式中，负数表示伦敦格林威治以西的点
href.push(gpsData.LongitudeRef == "W" ? "- " : "");
href.push(gpsData.Longitude.decimal);

link.href = href.join("");
link.innerHTML = "在谷歌地图上显示位置";

// 将此新链接添加到 DocumentFragment 对象中
miniDOM.appendChild(link);
}

// 获取 DocumentFragment 的内容，并将其添加到 HTML 页面上的 <div id="output"> 元素中。
// 在此代码中，我们只与实时 DOM 交互一次。
// 与每次添加元素时都交互实时 DOM 相比，这提供了良好的性能提升。
document.getElementById("output").appendChild( ➥
miniDOM.cloneNode(true));
}
});
```

从 Web 服务器运行清单 10-5 的 HTML 页面以查看最终结果。图像数据从二进制图像文件中提取，并以表格形式显示在页面上，如图 10-1 所示。

恭喜！你现在能够使用 Ajax 读取二进制文件数据，这应该会为你的 Web 应用程序打开一个充满更多可能性的全新世界。

### 总结

本章解释了如何使用 Ajax 技术加载二进制文件。你已经了解了如何根据已知的数据格式规范从这些文件中定位和提取信息。你将这一原理应用于数码相机拍摄的照片，从而能够从文件本身提取有关这些图像的信息，例如照片的拍摄时间和地点。现在，你应该能够应用本章所学的技术，从任何其他类型的二进制文件（包括音乐和电影文件）中提取存储的信息，以帮助你构建自己的富互联网应用程序 (RIA)。

在下一章中，你将学习如何根据页面上的数据，在浏览器中动态绘制图形，而无需使用 `<img>` 标签。

---

