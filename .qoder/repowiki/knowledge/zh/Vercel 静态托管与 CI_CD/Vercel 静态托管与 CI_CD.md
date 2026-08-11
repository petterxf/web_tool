---
kind: external_dependency
name: Vercel 静态托管与 CI/CD
slug: vercel
category: external_dependency
category_hints:
    - vendor_identity
    - client_constraint
scope:
    - '**'
source_files:
    - README.md
    - Readme-en.md
---

项目以纯静态站点形式部署到 Vercel（官方演示站 `web-tool-omega.vercel.app`），支持通过 GitHub 仓库导入或 `vercel` CLI 一键部署。构建命令与输出目录均留空，根目录即为发布目录；可选的 `vercel.json` 用于自定义路由与缓存头。作为推荐部署方式，无需后端即可运行。