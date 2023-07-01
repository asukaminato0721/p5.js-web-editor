# 公共API

该API提供了一种以编程方式将数据导入p5.js Web编辑器的方法。

# 认证

访问API需要使用与现有编辑器用户帐户关联的个人访问令牌。可以通过登录用户的设置页面创建和删除令牌。

在与API进行通信时，用户名和令牌必须在每个请求中使用基本身份验证发送。

这涉及将Base64编码的`${username}:${personalAccessToken}`发送到`Authorization`标头中。例如：
  `Authorization: Basic cDU6YWJjMTIzYWJj`

# API访问

除非另有说明，所有请求发送和接收`Content-Type: application/json`。

# 版本控制

API进行了版本控制，版本号位于根URL路径中，例如API的第1个版本可以在`http://editor.p5js.org/api/v1`找到。

访问API时必须提供版本号。

| 版本    | 发布日期    |
|--------|------------|
| v1     | 未发布     |

# 模型

API接受并返回以下模型对象，以JSON格式。

## Sketch（草图）

| 属性    | 类型              | 描述                     |
|--------|-------------------|-------------------------|
| name   | 字符串            | 草图的标题                |
| files  | DirectoryContents | 草图中的文件和目录。请参见`DirectoryContents`以了解其结构。 |
| slug   | 字符串            | 可用于访问草图的路径       |


    {
      "id": 字符串, // 不透明ID
      "name: 字符串,
      "files": DirectoryContents,
      "slug": 字符串 // 可选项
    }

### 验证

- `files`必须具有一个扩展名为`.html`的顶级文件。如果未提供文件，则会自动创建默认的`index.html`和关联的`style.css`文件。
- `slug`必须是URL安全的字符串。
- `slug`在用户的所有草图中必须是唯一的。

## DirectoryContents（目录内容）

文件名到`File`或`Directory`的映射。每个条目的键用作文件名。使用映射可以确保目录中的文件名是唯一的。


    {
      [字符串]:  File | Directory
    }


    {
      "sketch.js": { "content": "var answer = 42;" },
      "index.html" { "content": "..." }
    }

## DirectFile（直接文件）

该文件可以在编辑器用户界面中进行编辑，并存储在编辑器的数据库中。

| 属性      | 类型           | 描述                        |
|-----------|----------------|----------------------------|
| content   | UTF-8字符串    | 文件的内容，UTF-8编码的字符串 |

    {
      "content": 字符串
    }

## ReferencedFile（引用文件）

该文件托管在互联网的其他位置。它会显示在编辑器的列表中，并可以使用编辑器中的代理URL引用。


| 属性    | 类型 | 描述                             |
|--------|------|---------------------------------|
| url    | URL  | 指向托管在其他位置的文件的有效URL |

    {
      "url": URL
    }

## File（文件）

`File`可以是`DirectFile`或`ReferencedFile`。API在所有地方都支持这两种类型。

## Directory（目录）

| 属性    | 类型              | 描述                     |
|---------|-------------------|-------------------------|
| files   | DirectoryContents | 目录内容的映射               |

    {
      "files": DirectoryContents
    }

# API端点

## 草图（Sketches）

## `GET /:user/sketches`

列出用户的草图。

此操作不会返回草图中的文件，仅返回草图的元数据。

### 请求格式
无请求体。

### 响应格式
    {
      "sketches": 数组<Sketch>
    }

### 示例

    GET /p5/sketches
    
    {
      "sketches": [
        { "id": "H1PLJg8_", "name": "我的可爱草图" },
        { "id": "Bkhf0APpg", "name":  "我的可爱草图2" }
      ]
    }


## `POST /:user/sketches`

创建一个新的草图。

草图必须至少包含一个具有`.html`扩展名的文件。如果在有效载荷中未提供文件，则将自动创建默认的`index.html`文件和链接的`style.css`文件。

### 请求格式
参见上面的“Sketch”（草图）中的内容。

### 响应格式
    {
      "id": 字符串
    }

### 示例

    POST /p5/sketches
    
    {
      "name": "我的可爱草图",
      "files": {
        "index.html": { "content": "<DOCTYPE html!><body>你好！</body></html>" },
        "sketch.js": { "content": "var useless = true;" }
      }
    }

`files`可以嵌套以表示文件夹结构。例如，以下内容将在草图中创建一个空的“data”目录：

    POST /p5/sketches
    
    {
      "name": "我的可爱草图2",
      "files": [
        {
           "name": "assets",
           "type": "",
           "files": {
            "index.html": { "content": "<DOCTYPE html!><body>你好！</body></html>" },
            "data": {
              "files": {}
            }
          }
       }
    }

### 响应

| HTTP代码   | 响应体                                                                 |
|------------|-----------------------------------------------------------------------|
| 201 已创建 | 草图的ID                                                              |
| 422 不可处理的实体 | 文件验证失败、不支持的文件类型、slug已存在 |

### 示例

    201 已创建
    
    {
      "id": "Ckhf0APpg"
    }

## `DELETE /:user/sketches/:id`

删除草图及其关联的所有文件。

### 请求格式
无请求体。

### 响应格式
无响应体。

### 示例

    DELETE /p5

/sketches/Ckhf0APpg

### 响应

| HTTP代码       | 描述              |
|----------------|------------------|
| 200 OK         | 草图已被删除      |
| 404 未找到      | 草图不存在        |
