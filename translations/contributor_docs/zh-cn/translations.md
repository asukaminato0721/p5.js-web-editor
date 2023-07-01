# 翻译指南


*如何为p5 web编辑器贡献翻译*

## 翻译的一般规则

为了简化翻译过程，采用了以下一般规则：

## 技术部分

* 有一个文件用于翻译特定语言的所有文本，该文件位于目录下的相应区域[子目录](https://github.com/processing/p5.js-web-editor/tree/develop/translations/locales)
* 必须将新的语言代码添加到[client/i18n.js](https://github.com/processing/p5.js-web-editor/blob/edae248eede21d7ad7702945929efbcdfeb4d9ea/client/i18n.js#L22)
* 需要在`.env`中添加`TRANSLATIONS_ENABLED=true`以激活语言的下拉菜单。

#### 语言代码
我们使用标准的[IETF语言代码](https://en.wikipedia.org/wiki/IETF_language_tag)来标识语言。在大多数情况下，代码要么是表示语言的两个小写字母（如日语的`ja`），要么是语言代码后面跟着连字符和两个大写字母的国家（如美式英语的`en-US`）。

#### i18n.js
在`i18n.js`方面，您需要更新以下四个内容：

1. 将您的语言代码添加到`availableLanguages`数组中。
```js
const availableLanguages = ['en-US', 'es-419', 'ja', 'newLanguage'];
```

2. 从`date-fns/locale`导入您的语言区域。这用于翻译日期和时间。
```js
import { enUS, es, ja, newLanguage } from 'date-fns/locale';
```

3. 通过将其添加到`languageKeyToDateLocale`函数的映射中，将语言区域与国家代码关联起来。
```js
export function languageKeyToDateLocale(lang) {
  const languageMap = {
    'en-US': enUS,
    'es-419': es,
    'ja': ja,
    'newLanguage': newLanguage
  };
  return languageMap[lang];
}
```

4. 将您的语言名称添加到`languageKeyToLabel`函数的映射中。这将确定下拉菜单中显示的内容。
```js
export function languageKeyToLabel(lang) {
  const languageMap = {
    'en-US': 'English',
    'es-419': 'Español',
    ja: '日本語',
    'newLanguage': 'New Language Name'
  };
  return languageMap[lang];
}
```

## 翻译

* 每个组件应在名为组件名称的字典内引入自己的键集。
   例如：如果要翻译AssetList.jsx，需要在translations.json中引入以下命名空间：
```json
 "AssetList": {
    "Title": "p5.js Web Editor | My assets",
    "ToggleOpenCloseARIA": "Toggle Open/Close Asset Options",
    "OpenNewTab": "Open in New Tab",
    "HeaderName": "Name",
  }
```
* 一些常见的文本以`Common`前缀存在，请首先检查是否已经存在相同的标签。
* 每个键都遵循PascalCase命名风格。
* 在用于ARIA文本的键中，每个键都应以后缀`ARIA`结尾。
* 外观中的键的顺序应按照它们在源代码中出现的顺序进行排序。

## 语言使用

Processing Foundation致力于将技术和艺术的社区扩大到包括并支持那些由于种族、性别、阶级、性取向和/或残疾而无法获得平等机会的人。我们将软件视为一种媒介，它连接了两件事物。我们将其视为思考和创造的手段。我们相信软件及其学习工具应该对每个人都可访问。

基于这些原则，我们强烈建议尽可能使用无性别的术语。如果无法避免，使用最包容的版本。
例如，在西班牙语翻译中，我们采用了特定的规则：
避免使用男性化的词语：而是使用字母'e'。我们选择使用'e'而不是其他方法，如'x'或'@'，是因为您还需要考虑屏幕阅读器如何朗读文本。

## 背景

您想了解背景吗？
* 针对西班牙语的Alpha Editor的原始想法在[此问题](https://github.com/processing/p5.js-web-editor/issues/595)中得到了解决。
* 关于使用哪个库的讨论在[Library](https://github.com/processing/p5.js-web-editor/issues/1447)中得到了解决。
* [UI设计](https://github.com/processing/p5.js-web-editor/issues/1434)
* [语言使用](https://github.com/processing/p5.js-web-editor/issues/1509)



谢谢！

P5.js Web Editor 社区
