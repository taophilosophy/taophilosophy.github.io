# taophilosophy.github.io

**Please do not submit any PR to submodules of this project! It should be submitted to <https://github.com/taophilosophy/SEP-CN>**

## 架构模式

gitbook.io（可以编辑、修改、创建目录和文本内容，与 GitHub 双向同步，并且提供网页预览，有子域名） <--> GitHub A（使用 GitHub Action 推送 B） --> GitHub B（作为子目录，即 docs，使用 GitHub Action 编译推送，也可以推到自己的服务器） --> GitHub Page （B 的 page）。

## 构建入口与依赖（ADR-0055，SEP-CN 侧）

本仓是**构建壳**：`docs/` 是指向 <https://github.com/taophilosophy/SEP-CN> 的子模块，站点的 `public/`、`.vitepress/`（配置层与主题层）全在子模块里；本仓只持有依赖清单、锁文件与 Pages 工作流。

```sh
git clone --recurse-submodules https://github.com/taophilosophy/taophilosophy.github.io
cd taophilosophy.github.io && bun install
bun run dev      # VitePress root = docs，http://<局域网 IP>:81
bun run build    # → docs/.vitepress/dist
bun run preview
```

* 依赖只在本仓 `package.json` 声明（VitePress 2.0.0-alpha.21、pagefind 插件、MathJax 4.1.3、霞鹜文楷字体包、littlefoot）；子模块不装依赖、不持 `package.json`。
* 构建前置由本仓脚本调用子模块里的 `docs/.vitepress/prepare-assets.mjs`（MathJax 离线资源）与 `prepare-gitstamps.mjs`（页脚时间戳），二者与 cwd 解耦。
* 工作流在构建前 `cp docs/README.md docs/index.md`：VitePress 把 `README.md` 编成 `README.html`，Pages 站点根需要 `index.html`。
* 要预览子模块的**本地工作树**：把 `docs/` 换成目录联接（Windows `mklink /J docs C:\...\SEP-CN`；Linux `ln -sfn /path/SEP-CN docs`）后再 `bun run dev`。注意 VitePress 会解析真实路径，目录联接只用于人工 dev 预览，CI 一律用真实子模块检出。