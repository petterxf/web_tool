---
kind: external_dependency
name: GitHub Pages 静态托管
slug: github-pages
category: external_dependency
category_hints:
    - vendor_identity
scope:
    - '**'
source_files:
    - README.md
    - Readme-en.md
---

项目同时托管于 GitHub Pages（演示站 `geeeeeeeek.github.io/web_tool/`）。部署方式为在 GitHub 仓库 Settings → Pages 中启用，指定分支（通常为 main）与根目录 `/`，提交后自动构建并发布。适合零配置的静态站点托管。