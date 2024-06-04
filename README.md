# taophilosophy.github.io

**Please do not submit any PR to submodules of this project! It should be submitted to <https://github.com/taophilosophy/SEP-CN>**

## 架构模式

gitbook.io（可以编辑、修改、创建目录和文本内容，与 GitHub 双向同步，并且提供网页预览，有子域名） <--> GitHub A（使用 GitHub Action 推送 B） --> GitHub B（作为子目录，即 docs，使用 GitHub Action 编译推送，也可以推到自己的服务器） --> GitHub Page （B 的page）。