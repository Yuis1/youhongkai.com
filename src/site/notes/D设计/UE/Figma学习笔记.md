---
{"dg-publish":true,"permalink":"/D设计/UE/Figma学习笔记/","noteIcon":"","created":"2024-08-28T17:27:38.000+08:00","updated":"2024-04-24T00:40:56.000+08:00"}
---


作者：游鱼思

以下内容是看视频学习时的笔记。

---

## 安装使用

Figma Web版：需要安装字体插件

本地安装版：不用字体插件

## 常用快捷键和操作
- 平移视图：Space + 鼠标左键
- 移动工具：V
- 缩放工具：K
- 恢复100%：shift+0
- 适应窗口：shift+1
- 左右边栏隐藏：ctrl + /
- 复制：alt + 拖拽
- 重复复制：ctrl + D
- 修改单个控制点：alt + 控制点拖拽
- 整理：先多选，再点右下角 整理 按钮
更多快捷键：[Figma 快捷键](https://www.cnblogs.com/sbgong/p/15652611.html)

缩放工具：可以等比例缩放圆角等形状

等分：多选元素，之后在选框右下腹出现等分按钮。等分之后，可以拖拽等分中线来整体调整间距，也可以拖拽元素中心点来替换元素位置。

### 编组

特性：整体移动、整体填充属性、批量缩放

快捷键：编组 Ctrl+G，取消编组 Ctrl+Shift+G

### 画框 Frame

快捷键：添加 Ctrl + Alt + G，取消 Ctrl+Shift+G

特性：首先具备编组的特性、裁剪内容、Frame自身可填充描边、嵌套Frame。弹性布局必备，子元素能够在父元素变化时保持自身相对位置和大小。

对Frame自身进行操作，不改变内容：Alt + 移动

嵌套操作：将子Frame拖拽进来，标题栏消失。强制嵌套：按住Ctrl。强制不嵌套：按住space。

### 形状工具

放置图片工具：将外部图片内置到形状中。

蒙版：上层为图像，下层为遮罩。不是真的剪切

### 钢笔

曲线：在第二个点，拖拽。

直线、曲线 的切换：ctrl + 鼠标左键

断线：Esc

闭合：如果是已经断开的线，再次用钢笔工具去连接

加点：在线上，鼠标左键

减点：delete

锚点的控制工具：按Option，可只控制一边

### 文字

Figma的字体都是用英文命名，但是可以通过安装插件，让中文字体用中文展示。

复制：

- 复制属性：可以实现文字样式的复制
- 复制文字：仅复制文字，不复制样式
Figma的文字可以加链接（链接在文字上直接粘贴），可以跳到外部，或者跳到画框（选中画框点右上角 分享，获取分享链接）。
保存文本样式：选中文字，右侧 属性-Text-右上角按钮-+，样式共享给团队使用。修改样式同步到所有引用。

### 属性调整
- 所有输入框的数值选项调整：按住 Option 鼠标左右拖动。也可以直接输入运算公式，比如 300/2
- 边距：多选之后，右下角出现等边距按钮，点击实现等边距。之后还可拖拽中间空白处统一调整边距。
- 圆角：右侧折叠了ios圆角调整功能，因为ios圆角特殊。
### 图层
- 上下移动图层：command + ] 、command + [
### 填充
- 可多重填充，从而创造复杂的填充样式
- 颜色样式：和文字样式设置方法一样，可以复用。且修改样式同步到所有引用。
导出：
- 单个导出：选中需要导出的对象，侧边栏 - 导出。导出时可选放大比例。
- 按切片导出：用切片工具选则页面的某一部分，此时导出则只导出该切片部分。
### 组件

创建的组件是母版（图标实心），通过母版复制出来的是实例（图标空心）。

实例会跟随母版一同变化，如果要不跟随母版，要对实例进行实例分离。

所有可用的组件，从左边栏 资产 Assets 里找到。

团队组件：在 Assets - Library 里可以调用和发布。

#### 组件的变体

使用场景：一类同风格的图标，但是有各种状态。比如，选择组件，有选中、未选中、启用、禁用这几种状态的组合。

创建组件变体：多选母版（不能是实例），右侧栏-变体-合并为变体。

使用组件变体：从合并变体组件中，选中需要的变体，按 Option拖拽出来。在右侧边栏-Property中，可以选择

变体组件的命名规则：采用 属性键1=属性值, 属性键=属性值, ... 的方式命名，键值都可以是英文。但只有值为 on、off、true、false 的时候，在变体实例的侧边栏才可用开关按钮（其实无所谓）。例如：版本=iOS, 启用状态=On, 开关状态 = False

## 弹性布局
### 约束

首先要选中子元素，才可对子元素设置相对于父级Frame的约束。

子元素只受直接父级元素的约束，不会跨级约束。

可以按Command，单独设置某些方向的约束，比如 左上约束，这样当页面变宽导致组件拉长时，组件内容还是维持左上对齐。

### 自动布局

逻辑和HTML DOM元素的布局相同。

调整子元素相对父元素的对齐和边距，以及子元素是否流式布局。

需要在父元素上选中以调整。

调整大小：

- 适应内容：内容变动会引发父级元素大小变动
- 填充容器：子元素大小随着父元素缩放
- 宽度固定+高度填充：对父元素调整宽度和高度，子元素的宽度不变，但是高度随着父元素缩放。