# 提议的公共API扩展

这里描述了对公共API的扩展提议。这些扩展都未经确认，但记录在这里供参考和讨论。

请参考[公共API](./public_api.md)获取API的当前版本。

# 认证

- 支持通过`Authorization: Bearer {your_access_token}` HTTP头部发送令牌，而不仅仅是基本身份验证。

# API访问

- 每个访问令牌的API写入操作限制为每秒X次。
- 操作是事务性的，例如，如果文件无效，草图将不会部分上传。

# 提议的API端点

## 草图

## `GET /:user/sketches/:id`

获取草图。

### 请求格式
无请求体。

### 响应格式
返回`Sketch`。

### 示例

    GET /p5/sketches/Ckhf0APpg
    
    {
      "name": "Another title",
      "slug": "example-1",
      "files": {
        "index.html": { "<DOCTYPE html!><body>Hallo!</body></html>" },
        "something.js": { "var uselessness = 12;" }
      }
    }

### 响应

| HTTP状态码    | 描述                        |
|---------------|----------------------------|
| 200 OK        | 返回创建的草图的ID         |
| 404 Not Found | 草图不存在                 |


## `PUT /:user/sketches/:id`

用全新的草图替换原有草图，但保持相同的ID。在创建新文件之前，将删除任何现有文件。

### 请求格式
参见上面的`Sketch`。

### 响应格式
无响应体。

### 示例

    PUT /p5/sketches/Ckhf0APpg
    
    {
      "name": "Another title",
      "files": {
        "index.html": { "content": "<DOCTYPE html!><body>Hallo!</body></html>" },
        "something.js": { "content": "var uselessness = 12;" }
      }
    }

### 响应

| HTTP状态码                | 描述                                 |
|--------------------------|------------------------------------|
| 200 OK                   |                                   |
| 404 Not Found            | 草图不存在                           |
| 422 Unprocessable Entity | 文件验证失败，不支持的文件类型         |


## `PATCH /:user/sketches/:id`

在保留现有数据的同时更新草图：

- 修改名称
- 更新文件内容或添加新文件

### 请求格式
参见上面的`Sketch`。

### 响应格式
无响应体。

### 示例：修改草图名称

    PATCH /p5/sketches/Ckhf0APpg
    
    {
      "name": "My Very Lovely Sketch"
    }

### 示例：添加文件到草图，或替换现有文件

    PATCH /p5/sketches/Ckhf0APpg
    
    {
      "files": {
        "index.html": { "content": "My new content" }, // 将替换内容
        "new-file.js": { "content": "some new stuff" } // 将添加新文件
      }
    }

###

 响应

| HTTP状态码                | 描述                     |
|--------------------------|-------------------------|
| 200 OK                   | 修改已完成              |
| 404 Not Found            | 草图不存在               |
| 422 Unprocessable Entity | 文件验证错误              |


## 操作草图中的文件

可以通过其路径（例如`data/something.json`）单独访问草图中的文件。

## `GET /:user/sketches/:id/files/:path`

获取文件的内容。

### 请求格式
无请求体。

### 响应格式
返回文件内容。

### 示例

    GET /p5/sketches/Ckhf0APpg/files/assets/something.js
    
    Content-Type: application/javascript
    
    var uselessness = 12;

### 响应

| HTTP状态码     | 描述                                    |
|---------------|---------------------------------------|
| 200 OK        | 返回文件内容，内容类型由文件扩展名设置  |
| 404 Not Found | 文件不存在                              |


## `PATCH /:user/sketches/:id/files/:path`

更新文件或目录的名称或内容。

### 请求格式
参见上面的`File`和`Directory`。

### 响应格式
无响应体。

### 示例：修改文件名

    PATCH /p5/sketches/Ckhf0APpg/files/assets/something.js
    
    {
      "name": "new-name.js"
    }

将文件`assets/something.js`更名为`assets/new-name.js`。

### 示例：修改文件内容

    PATCH /p5/sketches/Ckhf0APpg/files/assets/something.js
    
    {
      "content": "var answer = 24;"
    }

将文件`assets/something.js`的内容替换为`var answer = 24;`。

### 示例：创建目录

    PATCH /p5/sketches/Ckhf0APpg/files/assets
    {
      "files": {
        "info.csv": { "content": "some,good,data" }
      }
    }

现在，`assets/data/info.csv`将存在，内容为`some,good,data`。

文件将添加到目录中，而不仅仅是存在的文件。

### 响应

| HTTP状态码                | 描述                     |
|--------------------------|-------------------------|
| 200 OK                   | 修改已完成              |
| 404 Not Found            | 路径不存在              |
| 422 Unprocessable Entity | 文件验证错误              |


## `DELETE /:user/:sketches/files/:path`

删除文件/目录及其内容。

### 请求格式
无请求体。

### 响应格式
无响应体。

### 示例：删除文件

    DELETE /p5/sketches/Ckhf0APpg/files/assets/something.js

将从草图中删除`assets/something.js`。

### 示例：删除目录


    DELETE /p5/sketches/Ckhf0APpg/files/assets

将删除`assets`目录及其包含的所有内容。

### 响应

| HTTP状态码     | 描述                |
|---------------|--------------------|
| 200 OK        | 项目已被删除         |
| 404 Not Found | 路径不存在         |
