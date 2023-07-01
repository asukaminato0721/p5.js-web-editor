## 辅助功能指南

这是一个关于如何使用无障碍编辑器的指南，并且这是一个概述了p5-accessibility.js库的[链接](https://github.com/processing/p5.accessibility)，该库可以使p5.js作品对屏幕阅读器更加友好。

p5.js网络编辑器的代码符合网络无障碍标准。以下准则将有助于确保在开发过程中继续关注无障碍性。

**代码结构**

* 屏幕阅读器是一种辅助技术，用于帮助视力受损的用户导航网页。它们能够根据HTML标签的语义意义对内容进行优先级排序。因此，重要的是使用特定的标签，如`nav`、`ul`、`li`、`section`等。`div`是最不友好的屏幕阅读器标签。例如，[这里是`body`标签的语义意义](http://html5doctor.com/element-index/#body)。
* 所有按钮/链接/窗口都需要通过键盘访问（通过按Tab键、按空格等）。
* 在标签不适合屏幕阅读器的情况下，我们可以利用[tabIndex](http://webaim.org/techniques/keyboard/tabindex)。使用tabIndex可以确保所有元素都可以通过键盘访问。[代码示例](https://github.com/processing/p5.js-web-editor/blob/edae248eede21d7ad7702945929efbcdfeb4d9ea/client/modules/IDE/components/Sidebar.jsx#L88)
* 在打开新窗口或弹出窗口时，确保键盘焦点也转移到新窗口。[代码示例](https://github.com/processing/p5.js-web-editor/blob/edae248eede21d7ad7702945929efbcdfeb4d9ea/client/modules/IDE/components/NewFileForm.jsx#L32)

**标记**

* 在创建按钮图标、图像或无文本内容（不包括HTML5的`<button>`）时，请使用[aria-labels](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-label)。[代码示例](https://github.com/processing/p5.js-web-editor/blob/edae248eede21d7ad7702945929efbcdfeb4d9ea/client/modules/IDE/components/Toolbar.jsx#L107)
* 所有`<table>`都需要具有`summary`属性。这将确保用户了解表格的上下文。[代码示例](https://github.com/processing/p5.js-web-editor/blob/edae248eede21d7ad7702945929efbcdfeb4d9ea/client/modules/IDE/components/SketchList.jsx#L491)
* `ul`和`nav`菜单需要包含一个标题。[代码示例](https://github.com/processing/p5.js-web-editor/blob/edae248eede21d7ad7702945929efbcdfeb4d9ea/client/components/Nav.jsx#L281)

欲了解更多关于无障碍性的信息，请参阅[teach access教程](https://teachaccess.github.io/tutorial/)。