---
title: Bitmap圆角矩形
date: 2025-08-26 23:38:13、
categories:
- [存档, RM开发, 插件脚本]
tags:
- RMMZ
- 代码
---
# Bitmap类扩展 - 圆角矩形

```javascript {.line-numbers}
/*
Bitmap.strokeRoundRect(
    x, y, width, height, radius, color
)
- 绘制出圆角矩形的轮廓

Bitmap.fillRoundRect(
    x, y, width, height, radius, color
)
- 填充出圆角矩形

Bitmap.gradientFillRoundRect(
    x, y, width, height, radius, color1, color2, vertical
)
- 以渐变色填充出圆角矩形
*/

Bitmap.prototype.strokeRoundRect = function(x, y, width, height, radius, color) {
    const context = this.context;
    context.fillStyle = color;
    context.strokeStyle = color;
    context.beginPath();
    context.moveTo(x + radius, y);
    context.lineTo(x + width - radius, y);
    context.quadraticCurveTo(x + width, y, x + width, y + radius);
    context.lineTo(x + width, y + height - radius);
    context.quadraticCurveTo(x + width, y + height, x + width - radius, y + height);
    context.lineTo(x + radius, y + height);
    context.quadraticCurveTo(x, y + height, x, y + height - radius);
    context.lineTo(x, y + radius);
    context.quadraticCurveTo(x, y, x + radius, y);
    context.closePath();
    context.stroke();
};

Bitmap.prototype.fillRoundRect = function(x, y, width, height, radius, color) {
    const context = this.context;
    context.fillStyle = color;
    context.beginPath();
    context.moveTo(x + radius, y);
    context.lineTo(x + width - radius, y);
    context.quadraticCurveTo(x + width, y, x + width, y + radius);
    context.lineTo(x + width, y + height - radius);
    context.quadraticCurveTo(x + width, y + height, x + width - radius, y + height);
    context.lineTo(x + radius, y + height);
    context.quadraticCurveTo(x, y + height, x, y + height - radius);
    context.lineTo(x, y + radius);
    context.quadraticCurveTo(x, y, x + radius, y);
    context.closePath();
    context.fill();
};

Bitmap.prototype.gradientFillRoundRect = function(
    x, y, width, height, radius, color1, color2, vertical
) {
    const context = this.context;
    const x1 = vertical ? x : x + width;
    const y1 = vertical ? y + height : y;
    const grad = context.createLinearGradient(x, y, x1, y1);
    grad.addColorStop(0, color1);
    grad.addColorStop(1, color2);
    context.save();
    context.fillStyle = grad;
    context.beginPath();
    context.moveTo(x + radius, y);
    context.lineTo(x + width - radius, y);
    context.quadraticCurveTo(x + width, y, x + width, y + radius);
    context.lineTo(x + width, y + height - radius);
    context.quadraticCurveTo(x + width, y + height, x + width - radius, y + height);
    context.lineTo(x + radius, y + height);
    context.quadraticCurveTo(x, y + height, x, y + height - radius);
    context.lineTo(x, y + radius);
    context.quadraticCurveTo(x, y, x + radius, y);
    context.closePath();
    context.fill();
    context.restore();
    this._baseTexture.update();
};
```

<br>

使用实例：

<br>

```javascript {.line-numbers}
/* 将窗口选项的背景形状从矩形改为圆角矩形 */

Window_Selectable.prototype.drawBackgroundRect = function(rect) {
    const c1 = ColorManager.itemBackColor1();
    const c2 = ColorManager.itemBackColor2();
    const x = rect.x;
    const y = rect.y;
    const w = rect.width;
    const h = rect.height;
    const radius = 5;  // 圆角矩形的半径
    this.contentsBack.gradientFillRoundRect(x, y, w, h, radius, c1, c2, true);
    this.contentsBack.gradientFillRoundRect(x, y, w, h, radius, c1);
};
```