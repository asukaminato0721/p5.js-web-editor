# 发布

创建发布的指南。

## 背景
该项目发布指南基于以下内容：
* [git-flow](https://nvie.com/posts/a-successful-git-branching-model/)
* [语义化版本控制（SemVer）](https://semver.org/)
* [npm-version](https://docs.npmjs.com/cli/version)
* [停止使用主从词汇](https://medium.com/@mikebroberts/let-s-stop-saying-master-slave-10f1d1bf34df)

## 步骤
1. `$ git checkout develop`
2. `$ git pull origin develop`
3. `$ git checkout -b release-<newversion>`
4. 进行所有需要进行的发布分支测试。可以简单地运行`npm test:ci`，或者可能需要经过几天的用户测试。
5. `$ npm version <newversion>`（查看[npm-version](https://docs.npmjs.com/cli/version)以获取有效的<newversion>值）。
6. `$ git checkout release`
7. `$ git merge --no-ff release-<newversion>`
8. `$ git push && git push --tags`
9. `$ git checkout develop`
10. `$ git merge --no-ff release-<newversion>`
11. `$ git push origin develop`
12. [在Github上起草新的发布](https://github.com/processing/p5.js-web-editor/releases/new)。选择刚刚创建的发布版本的标签，然后将标题设为`v<newversion>`。然后点击"Generate release notes"（生成发布说明）。发布该版本，完成！

Travis CI将自动将发布部署到生产环境，并将带有生产标签的Docker镜像推送到DockerHub。