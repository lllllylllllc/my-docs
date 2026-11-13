# my-docs
# 实践项目2：个人实验报告

## 1. 基本信息
- GitHub 登录名：lllllylllllc
- 个人仓库地址：https://github.com/lllllylllllc/my-docs
- 最终网站地址：https://lllllylllllc.github.io/my-docs/

## 2. 实践过程与关键截图

### 2.1 仓库创建
在 GitHub 上创建了名为 my-docs 的公开仓库，默认分支为 main，勾选了初始化 README 文件。
images/仓库截图.png

### 2.2 项目结构与 MkDocs 配置
按照作业要求创建了完整的项目目录，配置了 mkdocs.yml 核心文件。
- `docs/`：存放网站的所有 Markdown 源文档，是网站的内容来源
- `nav`：mkdocs.yml 中的导航配置项，定义网站顶部导航菜单
- `site/`：执行构建命令后自动生成的静态网站文件，无需手动修改
images/项目目录截图.png

### 2.3 本地构建与预览
在本地创建 Python 虚拟环境，安装了 mkdocs-material 依赖，执行构建和预览命令，检查了四个页面的导航、表格、代码块显示均正常。
images/终端构建成功.png
images/浏览器本地预览.png

### 2.4 Pages 设置
在仓库设置中将 Pages 发布来源设置为 GitHub Actions，使用官方工作流进行部署。
images/配置GithubPage.png

### 2.5 Actions 首次构建和部署
首次推送代码后，GitHub Actions 自动运行，build 和 deploy 两个任务均执行成功，完成首次网站部署。
images/Action成功记录.png
images/网站访问与导航页检查.png

### 2.6 网站访问与四页导航检查
部署成功后访问公开网站，依次打开首页、关于、使用指南、API 参考四个页面，导航跳转、表格渲染、代码块显示均正常。
images/网站访问与导航页检查.png

### 2.7 第二次推送与自动更新验证
修改首页内容并推送后，自动触发了新的部署流程，网站内容同步更新，验证了自动更新功能生效。
- 对应提交链接：https://github.com/lllllylllllc/my-docs/commit/555901f2a40f2f2596b147ef7a766966314ee467
- 对应 Actions 运行链接：https://github.com/lllllylllllc/my-docs/actions/runs/43380053323

images/第二次触发Action.png
images/自动部署验证.png

## 3. 问题与解决
本次实践过程中未遇到部署失败的错误。操作过程中核对了工作流日志、文件路径、YAML 缩进格式、Pages 发布来源等配置项，确保和作业要求一致。

## 4. 原理理解
1. **docs/ 和 site/ 目录分别存储哪些内容？为什么不建议手动改动 site/？**`docs/`用来存放我们手写的 Markdown 源文档，是网站内容的原始素材；`site/`属于构建输出目录，执行构建指令之后由程序自动生成整套静态网页资源。每一次重新构建项目，`site/`目录的全部文件都会被重写覆盖，如果手动在这里修改内容，再次构建之后改动就会全部丢失，所以不建议人工编辑该目录下的文件。

2. **本次配置的工作流，哪些场景会自动执行？**
当向 main 分支推送更新，改动涉及`docs/`目录内部文件、mkdocs.yml 配置、requirements.txt 依赖文件、`.github/workflows/deploy.yml`工作流脚本时，会自动触发工作流执行；除此之外，也可以进入仓库的 Actions 页面，手动启动该工作流。

3. **build 任务和 deploy 任务二者分工有什么不同？**
build 任务主要完成代码拉取、Python 运行环境准备、安装项目依赖包，完成网站整体构建，并且把构建得到的静态产物作为工件上传保存；deploy 任务要依赖 build 执行成功才会启动，它的职责是把已经生成好的静态产物正式发布到 GitHub Pages 服务上。

4. **仅仅修改 report/README.md 文件，会不会触发网站部署？请说明原因。**
不会自动触发部署。工作流里配置了文件路径过滤规则，仅监听指定的文档与配置文件变更，report 目录不在监听路径列表之中，修改该目录下的文件不会触发构建和部署，属于预期的配置效果。

5. **这套部署方案为什么不用手动新建 gh‑pages 分支？**
本案例采用 GitHub 官方提供的 Pages Actions 实现发布，它直接把构建产出的静态资源上传用于页面发布，不再需要借助 gh‑pages 分支保存网页静态文件，因此不需要手动创建该分支。