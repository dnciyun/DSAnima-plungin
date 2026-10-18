# DSAnima 官方插件库

本目录由 DSAnima 源码里的 `tool/build_extensions_catalog.py` 生成：`catalog.json` 是货架清单（每条带 zip 的 size + sha256），`packs/*.zip` 是扩展包本体。
**不要手改这里的文件** —— 改完扩展源目录后重跑构建再推；提交前跑一遍 `--check`。

app 里用的地址（扩展页 →「官方插件库」按钮出厂填的就是这一条）：
`https://raw.githubusercontent.com/dnciyun/DSAnima-plungin/main/catalog.json`

## 加一个扩展

1. 在 DSAnima 源码的 `tool/extension-packs/<id>/` 里放 `manifest.json` + 资源文件（格式见 `docs/specs/2026-10-17-dsanima-extension-format.md`）；
2. 构建：`python tool/build_extensions_catalog.py --src tool/extension-packs --out <本目录>`；
3. 自检：同一条命令加 `--check`（zip 存在 / size / sha256 / 根 manifest.json / 逐文件哈希 / 条目白名单，全过才推）。

## 安装时会发生什么

app 下载 zip 后先按 catalog 的 `size`/`sha256` 校验整包，再按 zip 里 `manifest.json` 的白名单严格解包、逐文件比哈希（多一个文件都拒）。扩展是**资源型插件**（GLSL shader / 画面调节模板 / 数据），宿主从不执行包内文件 —— 所以这里不放可执行文件。
