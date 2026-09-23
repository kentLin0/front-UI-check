# 视觉检查约定

对照真实运行页面与 Figma，或处理相互矛盾的审查意见时使用。

## 正式对照材料

记录 Figma 文件和节点 ID、节点是页面画板还是包含溢出同级图层的父级分组，以及浏览器 URL、视口、设备像素比、浏览器引擎、采集时间和排除的标注图层。

参考图与运行页面截图必须具有相同的像素尺寸，并展示相同的产品视口。截图前确认最终运行 URL、稳定的页面选择器、所需异步内容和 `document.fonts.ready`。如果任务要求验证运行页面，不能用粘贴的网页截图代替本地服务的新截图。

## 检查范围

直接查看原始尺寸的图片。独立检查分别覆盖：

1. 布局：固定区域、坐标、宽高、内边距、溢出与裁切。
2. 细节：图标、边框、颜色、字重、行高、菜单、控件和状态。
3. 完整性：遗漏的表格、列、行、控件、文案、弹窗和溢出内容。

请审查者指出具体元素及可观察的边界。不要把相似度分数或像素差异比例当作判定证据；浏览器渲染、字体回退、标注和抗锯齿都会影响这些数值。

## 核实争议问题

审查者报告位置偏移时：

1. 使用相同的 `x`、`y`、`width`、`height` 从两张原图裁切。
2. 按原始分辨率查看两处裁切结果。
3. 先比较组件边框和稳定的参照物，再判断文字字形。
4. 查询运行页面的 `getBoundingClientRect()` 和相关计算样式。
5. 只有裁切图或 DOM 测量确认问题后才修改代码。

可按页面中的稳定选择器调整以下浏览器代码：

```js
const rect = (selector) => {
  const value = document.querySelector(selector).getBoundingClientRect()
  return { x: value.x, y: value.y, width: value.width, height: value.height, right: value.right }
}

return ['header', 'main', '[data-testid="primary-content"]']
  .filter((selector) => document.querySelector(selector))
  .map((selector) => ({ selector, ...rect(selector) }))
```

多名审查者根据同一张缩放后的拼图得出相同结论，仍不能证明偏移存在。修改前要用原始分辨率的裁切图或运行页面布局测量核实。

## 字体限制

检查计算后的字体及实际加载的字体资源。即使组件坐标相同，平台回退字体也可能改变字形宽度、视觉字重和间距。除非所需字体在法律和技术上都可使用，否则应把这种差异记录为渲染限制。
