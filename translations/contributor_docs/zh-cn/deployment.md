# 部署

本文档包含有关如何部署到生产环境的信息，涵盖了各种不同的平台和工具以及其配置方法。

WIP（工作进行中）。
* 生产环境设置/安装
* Travis
* Docker Hub
* Kubernetes
* S3
* Mailgun
* Cloudflare
* DNS/Dreamhost
* mLab

## 部署流程

以下是应用程序部署的步骤：

1. 推送到 `develop` 分支，或合并拉取请求到 `develop` 分支。
2. 这将触发 [Travis CI](https://travis-ci.org/processing/p5.js-web-editor) 上的构建。
3. Travis CI 构建整个应用程序的（开发环境）Docker 镜像。
4. Travis CI 运行一些测试，目前仅包括 `npm run lint`。将来可以更新测试以包括更广泛的测试。如果测试失败，构建过程停止在此步骤。
5. 如果测试通过，Travis CI 构建整个应用程序的（生产环境）Docker 镜像。
6. 将该镜像推送到 [Docker Hub](https://hub.docker.com/r/catarak/p5.js-web-editor/)，使用唯一的标签名（Travis 提交）和 `latest` 标签。
7. Kubernetes 部署更新为刚刚推送到 Docker Hub 上的镜像，该镜像位于 Google Kubernetes Engine 上的集群中。

## 生产环境安装

只有在本地测试生产环境时才需要执行以下步骤。

_注意_：安装步骤假定您正在使用类 Unix 的 shell。如果您使用的是 Windows，需要使用 `copy` 而不是 `cp`。

1. 克隆该存储库并进入其中
2. `$ npm install`
3. 安装 MongoDB 并确保其正在运行
4. `$ cp .env.example .env`
5. （非可选）编辑 `.env` 并填写所有必要的值。
6. `$ npm run fetch-examples` - 这将下载示例草图到名为 'p5' 的用户目录中。
7. `$ npm run build`
8. 由于生产环境假设您的环境变量位于 shell 环境中，而不是 `.env` 文件中，您需要运行 `export $(grep -v '^#' .env | xargs)` 或类似的命令，参见这个 [Stack Overflow 回答](https://stackoverflow.com/a/20909045/4086967)。
9. `$ npm run start:prod`

## 自托管 - Heroku 部署

如果您有兴趣托管和部署自己的 p5.js Web 编辑器实例，您是可以的！它将与官方编辑器实例 editor.p5js.org 相同，只是域名不同，而且您将负责维护。我们建议使用 Heroku，因为您可以免费托管它。

1. 在 [Heroku](https://www.heroku.com/) 注册一个免费账户。
2. 点击这里：[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/processing/p5.js-web-editor/tree/develop)
3. 输入一个唯一的 *应用名称*，这将成为 URL 的一部分（例如 https://app-name.herokuapp.com/）。
4. 更新任何配置变量，或接受默认值进行快速评估（稍后可以更改以启用完整功能）。
5. 点击 "Deploy app" 按钮。
6. 完成后，点击 "View app" 按钮。
