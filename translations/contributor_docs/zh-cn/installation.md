# 开发环境安装

按照以下说明设置开发环境，这是在开始为该项目贡献代码之前需要完成的步骤。

## 手动安装

_注意_：安装步骤假设您使用的是类Unix的Shell。如果您使用的是Windows，您将需要使用`copy`而不是`cp`。

1. 安装Node.js。推荐的方法是通过[nvm](https://github.com/nvm-sh/nvm)安装Node。您也可以直接从Node.js网站下载[node.js](https://nodejs.org/download/release/v16.14.2/)版本16.14.2。
2. 在您自己的GitHub账户中[克隆](https://help.github.com/articles/fork-a-repo) [p5.js Web Editor存储库](https://github.com/processing/p5.js-web-editor)。
3. 将您在GitHub上克隆的存储库[克隆](https://help.github.com/articles/cloning-a-repository/)到您的本地计算机上。

   ```
   $ git clone https://github.com/YOUR_USERNAME/p5.js-web-editor.git
   ```

4. 如果您使用的是nvm，请运行`$ nvm use`以将您的Node版本设置为16.14.2
5. 确保您的npm版本设置为8.5.0。如果不是，请运行`npm install -g npm@8.5.0`进行安装。
6. 进入项目文件夹并使用npm安装所有必要的依赖项。

   ```
   $ cd p5.js-web-editor
   $ npm install
   ```
7. 安装MongoDB并确保它正在运行
   * 对于带有[homebrew](http://brew.sh/)的Mac OSX：`brew tap mongodb/brew`，然后`brew install mongodb-community`，最后使用`brew services start mongodb-community`启动服务器，或者您可以访问这里的安装指南[MacOS安装指南](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)
   * 对于Windows和Linux：[MongoDB安装](https://docs.mongodb.com/manual/installation/)
8. `$ cp .env.example .env`
9. (可选) 使用所需的密钥更新`.env`，以启用特定应用程序行为，例如添加Github ID和Github Secret，如果您想能够使用Github登录。
10. 运行`$ npm run fetch-examples`以下载示例草图到名为'p5'的用户。请注意，您需要配置您的GitHub凭据，可以按照[GitHub API配置](#github-api-configuration)部分的说明进行配置。
11. 按照[这个指南](https://prettier.io/docs/en/editors.html)在您的文本编辑器中启用Prettier。
12. `$ npm start`
13. 在浏览器中打开[http://localhost:8000](http://localhost:8000)
14. 安装[React开发者工具](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en)
15. 使用`ctrl+h`打开和关闭Redux DevTools，并使用`ctrl+w`移动它们。

## Docker安装

_注意_：安装步骤假设您使用的是类Unix的Shell。如果您使用的是Windows，您将需要使用`copy`而不是`cp`。

使用Docker，您可以拥有一个完整、一致的开发环境，无需手动安装诸如Node、Mongo等依赖项。它还可以帮助隔离这些依赖项及其数据，使其与您在同一台计算机上使用不同/冲突版本的其他项目分离开来等。

请注意，这将占用您计算机上相当大的空间。确保您至少有5GB的可用空间。

1. 为您的操作系统安装Docker
   * [Mac](https://www.docker.com/docker-mac)
   * [Windows](https://www.docker.com/docker-windows)
2. 安装[Docker Desktop](https://www.docker.com/products/docker-desktop/)
3. 克隆这个存储库并进入其中
4. `$ docker-compose -f docker-compose-development.yml build`
5. `$ cp .env.example .env`
6. (可选) 使用所需的密钥更新`.env`，以启用特定应用程序行为，例如添加Github ID和Github Secret，如果您想能够使用Github登录。
7. `$ docker-compose -f docker-compose-development.yml run --rm app npm run fetch-examples` - 请注意，您需要配置您的GitHub凭据，可以按照[GitHub API配置](#github-api-configuration)部分的说明进行配置。
8. 按照[这个指南](https://prettier.io/docs/en/editors.html)在您的文本编辑器中启用Prettier。

现在，每当您希望启动具有其依赖项的服务器时，可以运行：

9. `$ docker-compose -f docker-compose-development.yml up`
10. 在浏览器中打开[http://localhost:8000](http://localhost:8000)

要在正在运行的Docker服务器中打开终端/Shell（即在运行`docker-compose up`后）：

11. `$ docker-compose -f docker-compose-development.yml exec app bash -l`

如果您没有完整的服务器环境运行，您可以启动一个一次性容器实例（在使用后自动删除）：

12. `$ docker-compose -f docker-compose-development.yml run app --rm bash -l`

## S3桶配置

请注意，除非您正在开发应用程序的一部分，允许用户上传图像、视频等，否则此步骤是可选的。请参考以下[gist](https://gist.github.com/catarak/70c9301f0fd1ac2d6b58de03f61997e3)，设置用于该项目的S3桶。

如果您的S3桶位于美国东部（弗吉尼亚州）区域（us-east-1），您需要
为其设置一个自定义URL基础，因为它不遵循其他区域的标准
命名模式。在您的environment/.env文件中添加以下内容，将`BUCKET_NAME`更改为您的存储桶名称。这是
必需的，因为当前将此覆盖视为存储桶的完整路径，而不是正确的基本URL：
`S3_BUCKET_URL_BASE=https://s3.amazonaws.com/{BUCKET_NAME}/`

如果您已配置了S3桶和DNS记录以使用自定义
域名，您还可以使用此变量设置它。例如：

`S3_BUCKET_URL_BASE=https://files.mydomain.com`

有关使用自定义域名的更多信息，请参阅以下文档链接：

http://docs.aws.amazon.com/AmazonS3/latest/dev/VirtualHosting.html#VirtualHostingCustomURLs

## GitHub API配置

在该应用程序中，GitHub凭据用于：
* 与GitHub进行身份验证
* 将p5.js示例导入到本地数据库
* 渲染404页面

如果您正在开发需要上述用途之一的应用程序的一部分，则需要获取GitHub API凭据。

当您转到GitHub帐户的开发人员设置时，您会看到可以创建两种类型的应用程序：`GitHub Apps`和`OAuth Apps`（[GitHub Apps和OAuth Apps之间的区别](https://docs.github.com/en/free-pro-team@latest/developers/apps/differences-between-github-apps-and-oauth-apps)）。这个项目要求您创建一个`OAuth App`。在点击"New OAuth App"之后，您需要填写以下字段：
- **Application name**: `p5.js Web Editor - Local`
- **Homepage URL**: `http://localhost:8000`
- **Authorization Callback URL**: `http://localhost:8000/auth/github/callback`

如果您想了解更多关于您可以使用GitHub API做什么的信息，可以查看[API文档](https://developer.github.com/v3/)。
