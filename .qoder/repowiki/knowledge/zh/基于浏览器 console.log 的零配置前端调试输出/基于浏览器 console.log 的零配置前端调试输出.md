---
kind: logging_system
name: 基于浏览器 console.log 的零配置前端调试输出
category: logging_system
scope:
    - '**'
source_files:
    - assets/js/app-anim.js
    - assets/js/app-mini.js
    - assets/js/xenon-custom.js
    - assets/js/theia-sticky-sidebar-1.5.0.js
    - assets/js/fancybox.min-3.5.7.js
    - assets/js/popper.min.js
    - assets/fontawesome-5.15.4/attribution.js
    - index.html
---

## 1. 使用的系统/方法

本项目是纯静态 HTML/CSS/JS 站点（WebStack Hugo 主题），**没有后端日志系统、没有结构化日志框架、也没有统一的日志门面**。所有“日志”行为完全依赖浏览器原生 `console.*` API，属于最基础的客户端调试输出。

## 2. 关键文件与位置

- `assets/js/app-anim.js`：业务动画逻辑中的主要调试点，包含多处 `console.log('start trigger_lsm_mini')`、`console.log('checked=true')`、`console.log('isNoAnim=true/false')`、`console.log(sites[i].id, id)` 等，用于追踪触发器、复选框状态和站点 ID 匹配过程；同时在文件末尾输出一行带样式的品牌横幅 `console.log("\n %c WebStack-Hugo 导航主题 By ShumLab %c ...")`。
- `assets/js/app-mini.js`：`app-anim.js` 的精简版，保留了相同的 `console.log` 调用点（如 `console.log(sites[i].id, id)`、品牌横幅输出）。
- `assets/js/xenon-custom.js`：在约第 1625 行有一处 `console.log(arguments)`，用于打印函数参数。
- `assets/js/theia-sticky-sidebar-1.5.0.js`：第三方库内部使用 `console.log('TSS: Body width smaller than options.minWidth. Init is delayed.')` 提示初始化延迟原因。
- `assets/js/fancybox.min-3.5.7.js`：Fancybox 库在加载时通过 `t.console=t.console||{info:function(t){}}` 做降级兼容，并大量使用 `console.info` / `console.error` / `console.table` / `console.group` 进行冲突检测与诊断输出。
- `assets/js/popper.min.js`：Popper.js 在多个校验失败路径中使用 `console.warn` 发出警告（如 modifier 顺序警告、GPU acceleration 弃用警告等）。
- `index.html`：页面头部通过 `<script>` 内联代码调用 `fetch('https://v1.hitokoto.cn').catch(console.error)`，将网络请求错误直接抛到控制台。
- `assets/fontawesome-5.15.4/attribution.js`：Font Awesome 版权信息通过 `console.log` 输出。

## 3. 架构与约定

- **无统一 logger 模块**：不存在 `log.js`、`logger.js`、`logging/` 目录或任何可配置的日志中心。每个 JS 文件各自直接调用 `console.*`。
- **无日志级别管理**：没有 `DEBUG/INFO/WARN/ERROR` 级别的开关或分级策略，所有 `console.log` 在生产环境中都会输出到浏览器控制台。
- **无结构化字段**：日志消息以人类可读字符串为主（如 `'start trigger_lsm_mini'`、`'checked=true'`），偶尔附带变量值（如 `sites[i].id, id`），但没有统一的 JSON 结构、时间戳、上下文标识等字段。
- **无 sink/输出路由**：所有输出目标均为浏览器 DevTools 控制台，不存在写入文件、发送到远端服务、按环境切换 sink 的逻辑。
- **第三方库自带诊断输出**：Fancybox、Popper.js、Theia Sticky Sidebar、Font Awesome 等第三方库各自保留自己的 `console.*` 诊断语句，项目未对其进行屏蔽或聚合。

## 4. 约定与约束

- **开发期调试痕迹残留**：`app-anim.js` 中仍存在大量 `//console.log(...)` 形式的注释掉调试语句，说明该项目处于“边开发边遗留调试输出”的状态，没有统一的清理流程。
- **生产环境无日志过滤**：由于没有构建时移除 `console.log` 的配置（仓库中未发现 webpack/vite/esbuild 等构建脚本），这些 `console.log` 会随源码一起发布到 GitHub Pages/Vercel 等静态托管平台。
- **错误处理简单粗暴**：`index.html` 中对 `fetch` 的错误仅用 `.catch(console.error)` 兜底，没有上报或重试机制。
- **第三方库日志不可控**：项目未对第三方库的 `console.warn`/`console.error` 做全局拦截或重定向，因此 Popper.js、Fancybox 等的诊断信息也会直接出现在用户控制台中。

总结：本仓库不存在企业级或框架级的 logging system，仅依赖浏览器原生 `console.*` 作为零配置的调试输出手段，且存在多处遗留的调试 `console.log` 语句，缺乏级别、格式、sink 的统一治理。