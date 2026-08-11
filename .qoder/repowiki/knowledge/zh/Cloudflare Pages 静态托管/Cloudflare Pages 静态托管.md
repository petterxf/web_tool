---
kind: external_dependency
name: Cloudflare Pages 静态托管
slug: cloudflare-pages
category: external_dependency
category_hints:
    - vendor_identity
scope:
    - '**'
source_files:
    - README.md
    - Readme-en.md
---

项目通过 Cloudflare Pages 提供另一个在线演示（`web-a55.pages.dev`）。部署流程为登录 Cloudflare Dashboard → 创建 Pages 项目 → 连接 GitHub 仓库 → 构建命令与发布目录留空 → 点击 Deploy。适用于需要 CDN 加速的静态站点。