# H5 活动页面低代码平台：开源方案调研

> 调研日期：2026-07-24  
> 范围：面向营销落地页、互动活动页、移动端 H5 的可视化编辑器/低代码引擎。本文只采用项目官方仓库、官方文档和仓库许可证作为事实来源。

## 结论先行

如果目标是新建一个可长期演进的 H5 活动平台，建议先做三条技术路线的 PoC：

1. **TMagic**：现有开源项目里与“移动端 H5 活动搭建”定位最直接，编辑器、DSL、运行时边界清楚，且仍在发版。适合希望较快得到 H5 编辑能力、能接受 Vue 3 编辑器技术栈的团队。
2. **Puck**：React/Next.js 团队的优先候选。它提供模块化的 React 可视编辑器和渲染器，MIT 许可，数据自有；但活动平台、素材、动画、互动、发布、权限仍需自行建设。
3. **GrapesJS**：如果输出目标是可移植的 HTML/CSS 模板，而不是强绑定 React 组件树，它是更成熟、活跃的通用 Web Builder 内核。

备选路线：需要企业级插件/物料协议和源码生成时，可评估 **TinyEngine** 或 **Alibaba LowCode Engine**，但二者都不是现成的 H5 活动 SaaS。`H5-Dooring`、`luban-h5`、`mall-cook` 的产品形态很有参考价值，但许可证、技术债或维护状态使它们不适合作为未经验证就直接 fork 的新生产底座。

## 快速对比

活跃度只是维护风险的一个信号；Star 数只记录社区关注度快照，**不作为质量、稳定性或适配度结论**。

| 项目 | 类型与核心栈 | 许可证 | 截至调研日的维护信号 | 对 H5 活动的建议 |
|---|---|---|---|---|
| [TMagic](https://github.com/Tencent/tmagic-editor) | H5 可视化编辑器；Vue 3 + TypeScript；DSL；Vue 2/Vue 3/React runtime | [Apache-2.0](https://github.com/Tencent/tmagic-editor/blob/master/LICENSE) | [1.7.13，2026-04-29](https://github.com/Tencent/tmagic-editor/releases/tag/1.7.13)；约 4.9k Star | **优先 PoC**：定位最贴合 H5 |
| [Puck](https://github.com/puckeditor/puck) | React 模块化可视编辑器，可配 Next.js | [MIT](https://github.com/puckeditor/puck/blob/main/LICENSE) | [v0.21.2，2026-04-01](https://github.com/puckeditor/puck/releases/tag/v0.21.2)；约 13k Star | **React/Next.js 优先 PoC** |
| [GrapesJS](https://github.com/GrapesJS/grapesjs) | 通用 HTML/CSS Web Builder；TypeScript；插件化 | [BSD-3-Clause](https://github.com/GrapesJS/grapesjs/blob/dev/LICENSE) | [v0.23.2，2026-06-02](https://github.com/GrapesJS/grapesjs/releases/tag/v0.23.2)；约 25.9k Star | **HTML/CSS 模板路线优先 PoC** |
| [TinyEngine](https://github.com/opentiny/tiny-engine) | 可二开的低代码平台引擎；Vue/TypeScript；源码生成；可选 Java 后端 | [MIT](https://github.com/opentiny/tiny-engine/blob/develop/LICENSE) | 主干持续演进，官方仓库列有 2.x 里程碑；约 2.8k Star | 企业平台候选，H5 能力需定制 |
| [Alibaba LowCode Engine](https://github.com/alibaba/lowcode-engine) | 扩展型企业低代码内核；React/TypeScript；物料、setter、插件与协议 | [MIT](https://github.com/alibaba/lowcode-engine/blob/main/LICENSE) | 公开 PR 最新可见到 2025-08，整体明显放缓；约 15.9k Star | 能力强但工程门槛高 |
| [amis](https://github.com/baidu/amis) | JSON Schema 前端渲染框架与编辑器；React/TypeScript | [Apache-2.0](https://github.com/baidu/amis/blob/master/LICENSE) | [主干 2026-03-18 仍有提交](https://github.com/baidu/amis/commits/master/)；约 18.9k Star | 适合后台/表单，不适合自由营销画布主引擎 |
| [VvvebJs](https://github.com/givanz/VvvebJs) | Vanilla JS + Bootstrap 5 网页拖拽编辑器 | [Apache-2.0](https://github.com/givanz/VvvebJs/blob/master/LICENSE) | [2.0.5，2026-03-09](https://github.com/givanz/VvvebJs/releases/tag/2.0.5)；约 8.5k Star | 传统 HTML/CMS 场景的轻量候选 |
| [Craft.js](https://github.com/prevwong/craft.js) | React 页面编辑器底层框架；节点、拖拽、序列化 | [MIT](https://github.com/prevwong/craft.js/blob/develop/LICENSE) | 维护节奏稀疏，长期 beta 风格；约 8.7k Star | 可控性高，但平台能力几乎全自建 |
| [H5-Dooring](https://github.com/MrXujiang/h5-Dooring) | H5/PC 可视化平台；React/TypeScript，Node/Koa 示例 | [GPL-3.0](https://github.com/MrXujiang/h5-Dooring/blob/master/LICENSE) | [主干最新提交 2025-04-13](https://github.com/MrXujiang/h5-Dooring/commits/master/)；[无 GitHub Release](https://github.com/MrXujiang/h5-Dooring/releases)；约 10k Star | 功能贴合，适合参考；直接商用前先解决 GPL 与维护风险 |
| [luban-h5](https://github.com/ly525/luban-h5) | Vue 2 移动页面生成器；Strapi + SQLite | [GPL-3.0](https://github.com/ly525/luban-h5/blob/master/LICENSE) | [2026-01 有零星功能提交](https://github.com/ly525/luban-h5/commits/master/)，但[最新正式 Release 仍是 2021-02-13](https://github.com/ly525/luban-h5/releases)；约 6.2k Star | 可学习完整结构，不建议直接作为新底座 |
| [mall-cook](https://github.com/wangyuan389/mall-cook) | Vue 商城搭建器；JSON Schema 属性面板；uni-app 输出 H5/小程序 | [MIT](https://github.com/wangyuan389/mall-cook/blob/master/LICENSE) | [没有正式 Release](https://github.com/wangyuan389/mall-cook/releases)；约 5.6k Star | 适合商城/多端物料参考，不是通用活动底座 |
| [Mometa](https://github.com/imcuttle/mometa) | React/webpack 源码 AST 可视编辑辅助工具 | [MIT](https://github.com/imcuttle/mometa/blob/master/License) | [最新 Release v0.0.33，2022-02-22](https://github.com/imcuttle/mometa/releases/tag/v0.0.33)；约 4.1k Star | 不适合作为运营型在线 H5 平台 |
| PageCraft | 未找到可核验的同名开源 H5 Builder | 未发现 OSS 许可证 | [当前同名官网](https://pagecraftbuilder.com/)是商业 Beta 与订阅产品 | 从开源候选中排除，除非补充具体仓库 |

## 重点方案详评

### 1. TMagic：最贴近 H5 活动搭建的现役开源底座

官方将 `tmagic-editor` 直接定位为可视化搭建平台，并明确说明其主要目标是生成移动端 H5 页面。仓库的核心由编辑器、runtime 和 UI 包组成：编辑器产出 DSL，runtime 加载 DSL 得到活动页面；官方提供 Vue 2、Vue 3 和 React 三种 runtime 示例。编辑器自身使用 Vue 3 Composition API 与 TypeScript。[官方仓库说明](https://github.com/Tencent/tmagic-editor#项目介绍)

适合：

- 需要自由画布、属性表单、活动页 DSL 与独立运行时；
- 希望编辑器与消费者运行时解耦；
- 业务已有 Vue 或 React 物料，愿意围绕官方包继续开发。

主要局限：

- 它是“可搭建平台的底座和示例”，不是完整 SaaS；项目、模板、素材、账号权限、审批、发布、CDN、埋点和活动数据平台仍需建设；
- 编辑器技术栈是 Vue 3。即便运行时可选 React，纯 React 团队也要承担双栈维护成本；
- 官方明确文档仍在逐步完善，接入前应验证自定义组件、DSL 迁移、编辑器插件和历史页面兼容策略。[官方 README](https://github.com/Tencent/tmagic-editor)

许可证：主项目为 Apache-2.0，许可证文件同时列出 bundled dependencies 的第三方声明，分发产物时应保留适用的 notice。[官方 LICENSE](https://github.com/Tencent/tmagic-editor/blob/master/LICENSE)

### 2. Puck：React/Next.js 新项目的优先候选

Puck 是模块化的 React 可视编辑器。官方强调它本身就是 React 组件，可以嵌入包括 Next.js 在内的 React 环境，数据归使用者所有；官方示例展示 `Puck` 编辑器发布 JSON 数据，再由 `Render` 渲染，并提供 Next.js App Router + 静态生成 recipe。[官方 README](https://github.com/puckeditor/puck)

适合：

- 团队主栈是 React/Next.js；
- 希望业务物料就是普通 React 组件；
- 希望自行定义 Schema、数据存储和平台 UI，而不继承庞大的传统低代码内核。

主要局限：

- Puck 解决的是可视编辑/渲染核心，不提供 H5 活动领域能力；
- 自由定位、响应式尺寸、复杂时间轴动画、组件联动、表单提交、素材与模板、灰度发布等都要自己实现或封装；
- 相比 GrapesJS，它更偏 React 组件/结构化数据，不以导出任意独立 HTML/CSS 为核心目标。

许可证为 MIT，官方说明可用于内部系统和商业应用。[官方 LICENSE](https://github.com/puckeditor/puck/blob/main/LICENSE)

### 3. GrapesJS：通用 HTML/CSS Builder 的成熟选择

GrapesJS 的官方定位是可嵌入 CMS 的 Web Builder Framework，用来更快构建由 HTML、CSS 和变量组成的模板，可用于网站、邮件和移动应用。核心包含 Block、Style、Layer、Asset、Command、Storage 等管理器，并有官方插件及 React wrapper。[官方仓库](https://github.com/GrapesJS/grapesjs)；[官方文档](https://grapesjs.com/docs/)

适合：

- 输出物希望是框架无关的 HTML/CSS；
- 需要成熟的网页编辑器概念、插件与存储扩展点；
- H5 主要是图文、表单、营销落地页，而非高度应用化的 React 状态树。

主要局限：

- H5 业务组件、交互协议、数据源、安全沙箱、Schema 版本、移动端适配与发布平台均需自建；
- 自定义 UI/交互深度较高时，需要理解其内部 Component/CSS/Command/Storage 模型；
- 不要把开源 `grapesjs` core 与官方商业 `Studio SDK` 混为一体，评估时应逐包核对许可与功能边界。[官方 README](https://github.com/GrapesJS/grapesjs)

许可证为 BSD-3-Clause。[官方 LICENSE](https://github.com/GrapesJS/grapesjs/blob/dev/LICENSE)

### 4. TinyEngine：偏企业平台与源码生成

TinyEngine 官方定位是可定制、可二开、可集成的低代码引擎。其能力包括跨端/跨框架组件、第三方物料和插件、直接生成可部署源码、高低代码混合，以及可选的 Java 后端。官方提供 CLI 创建低代码平台，但本地 mock server 只提供基础后端能力。[官方仓库与使用说明](https://github.com/opentiny/tiny-engine)

它适合需要企业级平台骨架、物料接入和源码交付的团队。对于 H5 活动，仍需补齐移动画布、活动组件、动画/联动、轻量 runtime、发布链与运营数据；整体复杂度高于 Puck/TMagic。许可证为 MIT。[官方 LICENSE](https://github.com/opentiny/tiny-engine/blob/develop/LICENSE)

### 5. Alibaba LowCode Engine：扩展能力强，但不是成品平台

官方把它定义为“面向扩展设计的企业级低代码技术体系”，核心强调最小内核、物料、setter、插件、工具链和引擎/物料协议；TypeScript 提供完整类型。官方用法还注明 npm 包主要提供 typings，实际引擎采用 CDN 引入。[官方 README](https://github.com/alibaba/lowcode-engine)

适合：需要长期建设内部多领域低代码生态，愿意投入团队设计物料协议、设计器插件、渲染器和工程链路。对单一 H5 活动平台而言往往过重，而且活动物料、自由布局、移动端 runtime、发布与平台后端仍需自行完成。公开贡献节奏已明显放缓，应在 PoC 中验证依赖版本、文档与社区响应。[官方 Pull Requests](https://github.com/alibaba/lowcode-engine/pulls)

许可证为 MIT。[官方 LICENSE](https://github.com/alibaba/lowcode-engine/blob/main/LICENSE)

### 6. amis：强在 Schema 页面，不是自由 H5 画布

amis 通过 JSON 配置生成页面，官方描述重点是后台页面；仓库同时包含 `amis-editor`。官方明确说明 amis “只能实现前端低代码”，若需要完整平台还要采用其他产品或自行补齐。[官方 README](https://github.com/baidu/amis)

适合：表单、CRUD、后台运营配置、API 驱动的流程页面。若 H5 活动主要是问卷、报名或信息收集，可将 amis 用作某些组件或后台配置端；但营销 H5 常见的自由画布、动效编排、图层、互动玩法和轻量首屏并非其核心模型。许可证为 Apache-2.0。[官方 LICENSE](https://github.com/baidu/amis/blob/master/LICENSE)

### 7. VvvebJs：传统网页/CMS 的轻量选择

VvvebJs 是不依赖前端框架和构建工具的 Vanilla JavaScript 拖拽网页编辑器，默认围绕 Bootstrap 5，提供块/组件、Undo/Redo、文件管理、代码编辑、图片上传、HTML 或 ZIP 导出及 PHP/Node 保存示例。[官方 README](https://github.com/givanz/VvvebJs)

它适合传统 HTML 模板、CMS 内嵌和低复杂度落地页；现代组件状态模型、React/Vue 物料复用、复杂数据联动与大规模平台治理不如 Puck/TMagic/GrapesJS。许可证为 Apache-2.0。[官方 LICENSE](https://github.com/givanz/VvvebJs/blob/master/LICENSE)

### 8. Craft.js：高度可控，但只提供底层抽象

Craft.js 为 React 页面编辑器提供拖拽、节点状态、可放置区域、编辑控制和 JSON 序列化。官方明确表示它不提供开箱即用的完整编辑器 UI，而是让开发者从头控制编辑体验。[官方 README](https://github.com/prevwong/craft.js)

适合：React 团队有强定制诉求并愿意长期维护自己的编辑器内核。局限是属性面板、图层、资产、模板、预览隔离、历史版本、数据迁移和发布平台都要自建；维护信号相对稀疏，立项前应做升级与边界场景验证。许可证为 MIT。[官方 LICENSE](https://github.com/prevwong/craft.js/blob/develop/LICENSE)

## 可借鉴但不建议直接作为新生产底座

### H5-Dooring

它的功能清单与需求非常接近：多页面、基础/可视/媒体/商品组件、属性配置、表单设计、动画、交互、数据源、预览、撤销重做、模板，以及 Node/Koa 后台示例。[官方 README](https://github.com/MrXujiang/h5-Dooring)

风险在于：主干在 2025-04-13 后未见更新，没有正式 GitHub Release；开源仓库为 GPL-3.0，而 README 同时指向私有化部署和 SaaS 产品。若要 fork、修改并商业部署，应让法务根据具体交付/网络服务方式核查 GPL 义务，并向作者确认开源版与商业能力边界。它更适合作为功能拆解、交互和数据结构参考，而不是直接认定为低风险底座。[提交记录](https://github.com/MrXujiang/h5-Dooring/commits/master/)；[LICENSE](https://github.com/MrXujiang/h5-Dooring/blob/master/LICENSE)

### luban-h5

鲁班 H5 是 Vue 2 拖拽移动页面生成器，带页面管理、对齐、缩放、预览、撤销重做、表单收集、二维码预览等；官方技术栈写明 Vue、Strapi 和 SQLite。[官方 README](https://github.com/ly525/luban-h5)

2026-01 出现了组件树和拖拽排序等功能提交，不能简单称为完全停更；但正式 Release 仍停留在 2021 年，仓库仍标注 WIP，Vue 2/旧依赖与发布治理构成明显风险。加上 GPL-3.0，适合学习完整前后端结构，不建议直接作为新商业平台底座。[提交记录](https://github.com/ly525/luban-h5/commits/master/)；[Releases](https://github.com/ly525/luban-h5/releases)；[LICENSE](https://github.com/ly525/luban-h5/blob/master/LICENSE)

### mall-cook

Mall-Cook 是 Vue 商城低代码平台，支持多页面、可视化 JSON Schema 属性面板，并使用 uni-app 物料/模板输出 H5 和小程序；后端 service 使用 Node。[官方 README](https://github.com/wangyuan389/mall-cook)

它很适合参考商城物料接入、多端模板和属性配置器，但领域强绑定商城，没有正式 Release，通用营销活动所需的动画、互动、发布治理等仍需改造。MIT 许可较宽松，但维护与依赖风险仍需单独评估。[LICENSE](https://github.com/wangyuan389/mall-cook/blob/master/LICENSE)；[Releases](https://github.com/wangyuan389/mall-cook/releases)

### Mometa

Mometa 不是 Schema 驱动的在线低代码平台，而是依赖代码 AST、直接修改本地源码的研发辅助工具；官方说明其定位基于程序员本地开发模式，不推荐把本地开发环境放在远程服务，并且当前依赖 webpack 开发模式、只支持 React。[官方 README](https://github.com/imcuttle/mometa)

因此它适合存量 React 项目的可视化辅助编码，不适合运营人员、多租户、在线发布的 H5 活动平台。最新正式发布停在 2022-02，维护风险较高。许可证为 MIT。[Releases](https://github.com/imcuttle/mometa/releases)；[LICENSE](https://github.com/imcuttle/mometa/blob/master/License)

## 其他值得关注的方案

### NetEase Tango

[Tango](https://github.com/NetEase/tango) 是基于源码 AST 的低代码 Builder，强调 source in/source out、无私有 DSL，并给出移动应用、React Native、邮件等示例；使用 TypeScript/React，MIT 许可。[官方 LICENSE](https://github.com/NetEase/tango/blob/main/LICENSE)

它比 Mometa 更像可集成的设计器，但主仓库最近更新停在 2024-09 左右；更适合研究“源码生成/回写”路线，不是当前最优的 H5 Schema runtime 底座。

### Webstudio

[Webstudio](https://github.com/webstudio-is/webstudio) 是更完整、活跃的可自托管视觉开发平台，可作为大型 Web Builder 的架构与产品参考。但核心采用 [AGPL-3.0-or-later](https://github.com/webstudio-is/webstudio#license)，部分可选动画 SDK 另有专有许可。若修改后通过网络提供服务，AGPL 合规和不同包的许可边界必须优先由法务确认。对只做移动活动 H5 来说，它也可能过重。

## 明确排除或需要澄清

- **PageCraft**：未发现能与“H5 低代码开源项目”对应、且有官方仓库与开源许可证的可核验项目。当前可找到的 [PageCraft Builder 官网](https://pagecraftbuilder.com/)展示的是商业 Beta 和价格方案，不能据此认定开源。若指某个 GitHub 项目，需要补充准确 URL 后再评估。
- **完整内部工具平台**（如 Appsmith、ToolJet 等）：它们主攻数据库、后台、工作流和内部应用，不是移动营销自由画布，本轮不作为主要候选。

## 选型建议

### 推荐进入 PoC

| 团队与输出目标 | 首选 | 第二选择 | PoC 必测项 |
|---|---|---|---|
| Vue 团队，目标是移动 H5 活动 | TMagic | GrapesJS | 自定义物料、DSL 迁移、多尺寸画布、runtime 包体积、历史页面兼容 |
| React/Next.js 团队，页面由业务组件构成 | Puck | Craft.js | SSR/CSR 边界、组件懒加载、自由布局、属性 Schema、预览隔离 |
| 输出可移植 HTML/CSS，需 CMS/模板能力 | GrapesJS | VvvebJs | CSS 隔离、移动适配、脚本安全、模板导入导出、HTML 体积 |
| 企业级多领域平台、要物料/插件/源码生成 | TinyEngine | Alibaba LowCode Engine / Tango | 插件 API、源码质量、版本迁移、后端依赖、升级成本 |

### 不建议直接选作新生产底座

- Mometa：定位不同且维护弱；
- mall-cook：商城领域过重、缺少发布治理；
- luban-h5：Vue 2、GPL、正式 Release 长期停滞；
- H5-Dooring：虽然功能贴合，但 GPL、主干停更与商业版边界需先解决；
- 未确认仓库的 PageCraft。

## 许可证提醒

- MIT、Apache-2.0、BSD-3-Clause 通常便于商业二次开发，但仍有保留版权/许可证/NOTICE 等义务，应以对应项目的完整许可证和依赖清单为准。
- GPL-3.0 与 AGPL-3.0-or-later 是强 copyleft 许可证；是否触发源码提供义务取决于修改、组合、分发或网络服务方式，不能只凭“内部使用”或“私有化”标签判断。
- 编辑器、runtime、官方插件、商业 SDK 和示例后端可能采用不同许可证；最终选型应做逐包 SBOM 与许可证扫描。
- 本节是工程风险提示，不构成法律意见。

## 自研架构：先确定产品类型

本文默认需求是：企业内部或 SaaS 化的、移动端优先的营销活动页平台，运营人员可搭建落地页、报名表单、优惠券、抽奖、倒计时、商品卡片等页面；需要模板、版本、审核、发布、数据统计和微信分享；不允许普通运营人员执行任意 JavaScript。

可视化搭建大致分为三类：

| 类型 | 页面模型 | 优点 | 代价 | 适合度 |
| --- | --- | --- | --- | --- |
| 结构化组件编排 | 组件树 + props + actions 的 JSON Schema | 可控、稳定、易迁移、可做 SSR/SSG、安全边界清晰 | 自由度受组件库限制 | **最适合活动页平台** |
| 自由 HTML/CSS 编辑 | DOM/CSS 模型，允许细粒度样式 | 设计自由度高，可接近 Webflow | 响应式、数据绑定、版本兼容和安全更复杂 | 适合建站/邮件模板 |
| 绝对定位画布 | `x/y/width/height/zIndex` | 海报、长图还原快 | 多尺寸适配、可访问性、动态内容很难 | 只作为“自由画布容器”补充 |

建议采用“结构化流式布局为主，有限自由画布为辅”。栏目、网格、间距、吸顶按钮等用约束布局；只有贴纸、装饰图层等进入绝对定位容器。不要把 375px 设计稿整体按屏宽缩放作为唯一渲染方案。

## 推荐的总体架构

```text
运营/设计人员
    |
    v
[管理后台 + 可视化编辑器] -- postMessage --> [隔离的 iframe 画布]
    |                                           |
    | 保存 PageDocument                         | 复用同一组件注册表
    v                                           v
[BFF/API] ---> [页面/版本/模板/素材/权限服务]  [Preview Renderer]
    |                    |
    |                    +--> PostgreSQL
    |                    +--> Redis
    |                    +--> S3/OSS/COS
    |                    +--> Queue/Worker
    v
[发布编排器: 校验 -> 冻结版本 -> 构建 -> 上传 -> CDN -> 原子切换]
    |
    v
[CDN/静态 HTML + Runtime JS] ---> [活动 API: 表单/抽奖/领券/分享]
    |
    +--> [埋点采集 -> Kafka/队列 -> ClickHouse/数据仓库 -> 报表]
```

核心原则：

1. **编辑器、页面协议、运行时、发布器解耦。** 编辑器只生成文档；运行时只解释经过校验的文档；发布器把某个不可变版本变成发布产物。
2. **预览和线上必须使用同一套运行时与同一版本组件。** 编辑器画布可以加选中框，但组件渲染不能另写一套。
3. **草稿可变，发布版本不可变。** 线上 URL 指向一个发布别名；回滚只是把别名切回旧 artifact。
4. **组件能力白名单化。** 运营配置 props 和动作 DSL，不直接写 JS、HTML、请求头或任意 URL。
5. **MVP 先做模块化单体。** 页面、版本、模板、素材、发布、权限先在一个后端内按领域模块隔离；只有发布 Worker、图片处理、埋点采集等负载形态明显不同的部分独立部署。

## 前端架构

### Monorepo 建议

```text
apps/
  studio/             # 管理后台与编辑器
  runtime-web/        # 线上 H5 渲染应用
  api/                # BFF/业务 API
  publish-worker/     # 发布、截图、图片处理
packages/
  page-schema/        # PageDocument 类型、JSON Schema、迁移器
  component-contract/ # 组件元数据与注册协议
  components-runtime/ # 线上组件实现
  components-editor/  # 属性面板、缩略图、mock 数据
  renderer/           # 纯渲染器
  action-runtime/     # 事件/动作 DSL 解释器
  design-tokens/      # 主题与 token
  sdk/                # 表单、埋点、分享、登录 SDK
```

建议使用 pnpm workspace + Turborepo。共享“协议和渲染组件”，但不要让线上运行时依赖整个编辑器包。

### 编辑器组成

- Shell：页面列表、保存/发布、设备切换、撤销重做、预览、协作者状态。
- Component Palette：组件分类、搜索、拖入、模板区块。
- Canvas：独立 iframe；固定或响应式 viewport；与宿主通过有版本的消息协议通信。
- Outline/Layers：组件树、隐藏/锁定、重排。
- Inspector：根据组件 `fieldSchema` 动态生成属性面板。
- History：基于 command/patch 记录增量，不要每次深拷贝整棵页面树。
- Selection Overlay：选择框、间距提示、拖拽手柄应在编辑器层，不写入业务组件 DOM。
- Preview：从服务端取得一个短期 preview token，使用线上 renderer 打开独立 URL。

状态建议分层：服务器状态使用 TanStack Query；编辑会话使用 Zustand 或 Redux Toolkit；表单用 React Hook Form。编辑器文档更新采用 Immer patch 或显式 command。不要把远端页面列表、当前组件 hover、整棵页面文档全塞进一个 store。

### 画布隔离

iframe 能隔离编辑器 CSS、组件 CSS、viewport 和运行时异常，也是以后允许“高级自定义组件”时的重要安全边界。宿主与画布之间定义严格消息：

```ts
type StudioMessage =
  | { version: 1; type: 'LOAD_DOCUMENT'; document: PageDocument }
  | { version: 1; type: 'SELECT_NODE'; nodeId: string | null }
  | { version: 1; type: 'APPLY_PATCHES'; patches: JsonPatch[] }
  | { version: 1; type: 'SET_VIEWPORT'; width: number }
  | { version: 1; type: 'RUNTIME_ERROR'; nodeId?: string; message: string };
```

校验 `event.origin`、消息版本和 payload schema。预览不应使用编辑后台的管理员 Cookie。

## 页面协议与组件协议

### PageDocument 示例

```json
{
  "schemaVersion": 3,
  "pageId": "page_123",
  "componentSet": "marketing@2026.07",
  "theme": { "tokenSet": "brand-a@2", "overrides": {} },
  "root": {
    "id": "node_root",
    "type": "Page",
    "version": 1,
    "props": {},
    "children": [
      {
        "id": "node_hero",
        "type": "HeroBanner",
        "version": 2,
        "props": { "imageAssetId": "asset_x", "title": "夏日活动" },
        "style": { "paddingBlock": "space.4" },
        "events": {
          "click": [{ "type": "navigate", "params": { "target": "#form" } }]
        },
        "children": []
      }
    ]
  },
  "dataSources": [],
  "seo": {},
  "share": {},
  "experiments": []
}
```

关键要求：

- 文档、组件都带版本；迁移函数必须是确定性的：`v1 -> v2 -> v3`。
- 组件只保存稳定的 `type`，不要序列化 React 组件名、函数或 JSX。
- node ID 永久稳定，复制时重新生成；埋点和增量 patch 都依赖它。
- 资源存 `assetId`，发布阶段解析为带指纹的 CDN URL。
- 样式优先存设计 token 和受限属性；不要默认接受任意 CSS 字符串。
- 页面文档是事实来源，HTML 只是发布产物，不能用 HTML 反向恢复编辑状态。

### 组件注册表

每个物料至少包含：

```ts
interface ComponentDefinition<Props> {
  type: string;
  version: number;
  title: string;
  category: string;
  propsSchema: JsonSchema;
  defaultProps: Props;
  slots?: SlotDefinition[];
  capabilities: Array<'network' | 'form' | 'tracking' | 'auth'>;
  render: React.ComponentType<Props>;
  editor?: {
    previewImage?: string;
    fieldOverrides?: Record<string, React.ComponentType>;
    mockData?: unknown;
  };
  migrate?: (oldProps: unknown, fromVersion: number) => Props;
}
```

组件必须通过契约测试：schema 校验、默认 props 渲染、空数据/超长文本、移动端截图、事件上报、无障碍基础规则和 bundle 大小。组件下线只能“禁止新建”，不能让旧页面失去渲染器。

### 交互与数据绑定

使用有限动作 DSL，例如 `navigate`、`openModal`、`submitForm`、`track`、`requestApprovedApi`、`setState`。表达式引擎仅支持读取上下文和纯函数，不使用 `eval` / `new Function`。

数据源通过服务端 Data Gateway：组件引用已登记的数据源 ID；网关负责认证、字段裁剪、超时、缓存、限流、域名 allowlist 和审计。不要让运营人员填写任意后端 URL 并由服务器代请求，否则会形成 SSRF 面。

## 后端领域模块

| 模块 | 主要职责 |
| --- | --- |
| Identity/Tenant | SSO、用户、租户、RBAC、页面/文件夹级权限 |
| Project/Page | 项目、站点、页面、路由、草稿、锁 |
| Revision | 自动保存、版本快照、diff、恢复、审计日志 |
| Template | 页面模板、区块模板、标签、审核和可见范围 |
| Material | 组件包、版本、兼容性、灰度、下线策略 |
| Asset | 直传签名、元数据、图片转码、引用计数 |
| Publish | 校验、构建任务、环境、域名、发布记录、回滚 |
| Campaign | 表单、抽奖、优惠券、活动规则、库存/资格 |
| Analytics | 埋点定义、采集、漏斗、渠道参数、报表 |
| Audit | 谁在何时修改、审核、发布或导出数据 |

MVP 可使用一个 NestJS 应用和一个数据库，但代码目录按上述 bounded context 分开。所有关键写操作带 `tenant_id`，并在 repository 层强制租户过滤；敏感操作记录不可变审计日志。

## 存储与基础设施

| 数据 | 建议 | 原因 |
| --- | --- | --- |
| 项目/页面元数据、发布记录、权限 | PostgreSQL | 事务和关系约束清晰；页面 JSON 可先用 JSONB |
| 当前草稿 | PostgreSQL JSONB + revision 表 | MVP 简单；保存时乐观锁 `revision` 防覆盖 |
| 缓存、限流、短锁、幂等键 | Redis | 短生命周期状态 |
| 图片、视频、发布 artifact | S3 兼容对象存储 + CDN | 大文件与静态分发 |
| 异步任务 | MVP 用 BullMQ/Redis；规模化后 Kafka/RabbitMQ | 发布、转码、截图、导出解耦 |
| 行为分析 | ClickHouse 或进入现有数据仓库 | 高吞吐事件和聚合查询 |
| 搜索 | 先用 PostgreSQL；模板/素材规模大后再引入 OpenSearch | 避免过早增加系统数量 |

素材上传采用前端向对象存储直传，后端只签发短时凭证；完成后由 Worker 校验 MIME/魔数、尺寸、病毒，生成 WebP/AVIF 和多尺寸缩略图。原图与派生文件使用内容哈希或不可变 key。

## 发布与线上渲染

### 推荐发布流水线

1. 为草稿创建不可变 revision，生成 publish job。
2. 校验 PageDocument、权限、路由、资源、组件版本、动作和外链域名。
3. 运行 schema/组件迁移，但把迁移后的快照另存，不原地覆盖线上版本。
4. 冻结组件清单和资源清单，生成 manifest。
5. 预渲染 HTML（或静态 shell）并按需拆分 runtime chunk；压缩图片。
6. 自动跑 smoke test、链接检查、移动端截图和性能预算。
7. 上传带 content hash 的 artifact，CDN 预热。
8. 原子更新 `route -> artifactId`；失败不影响现网。
9. 记录发布人、revision、组件版本、构建日志；支持一键回滚。

### SSR、SSG 和 CSR 如何选

- 公共活动落地页：优先 SSG/预渲染 HTML + hydration，首屏、SEO、分享抓取更稳。
- 千人千面但外壳稳定：静态页面 + 客户端请求个性化区块，或边缘侧拼装。
- 强登录态页面：可用动态 SSR，但不要让整个页面每次都访问编辑数据库；发布版本应进入缓存/对象存储。
- 纯预览：允许 CSR，但必须与正式 renderer 共享组件和协议。

运行时按页面实际组件做 code splitting。首屏关键 CSS 内联，非首屏组件懒加载；图片必须有尺寸、响应式 `srcset` 和懒加载策略。为页面设性能预算，例如首屏 JS、最大图片、LCP 和 CLS 阈值，超限时阻止发布或至少明确告警。

## 营销活动特有能力

### 表单

- 表单定义与页面 schema 分离，提交 API 根据 form version 校验。
- CSRF/防重放、验证码/风控、IP/用户/设备限流、幂等键。
- 手机号等 PII 字段加密、脱敏展示、字段级导出权限、保留期限和删除流程。
- 导出走异步任务并生成短时下载链接；记录导出审计。

### 抽奖、优惠券与库存

- 活动规则和奖品库存不能写在页面 JSON 中，必须由活动服务托管。
- 资格检查、扣库存、中奖记录用事务或原子脚本；请求带幂等 key。
- 前端抽奖动画只展示服务端结果，不能在浏览器决定是否中奖。
- 热点活动需要排队/削峰、库存预热、熔断和降级文案。

### 微信生态

- 分享标题、描述、图片作为发布元数据；服务端生成签名，缓存 ticket。
- OAuth 回调域、业务域、静态资源域分开配置；域名白名单由管理员维护。
- 发布前提供微信内置浏览器真机预览和二维码；不要只测试桌面浏览器模拟器。

### 埋点与实验

- 自动事件：曝光、点击、页面停留、表单开始/成功；业务事件由组件契约声明。
- 每条事件包含 tenant、project、page、publishRevision、nodeId、componentType、channel、experimentVariant。
- 客户端批量、`sendBeacon`、采样和失败重试；采集端先落队列再入 ClickHouse/仓库。
- A/B 分流应由稳定 subject ID 哈希决定并记录实验版本；发布回滚不能改变已分流用户的分组语义。

## 安全模型

低代码平台的核心风险不是拖拽，而是“谁可以让什么代码和数据在终端用户浏览器里运行”。

- 默认不提供任意 JavaScript、任意 HTML、任意 iframe 和任意服务端代理 URL。
- 如必须支持自定义代码，把它作为管理员级能力：独立域名、sandbox iframe、最小 `allow` 权限、严格 CSP、短期 token、无后台 Cookie。
- 编辑后台域名、预览域名、线上活动域名、素材域名尽量分离；线上 H5 不携带管理端会话。
- CSP 通过响应头配置 `script-src`、`connect-src`、`img-src`、`frame-src`、`form-action`，同时限制谁能 iframe 嵌入页面。OWASP 建议对不可信内容使用 sandbox iframe，并说明 CSP 可限制动态资源、表单提交及嵌入来源：[CSP Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html)、[HTML5 Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/HTML5_Security_Cheat_Sheet.html)。
- 文件上传校验扩展名、真实类型、大小并改名，存储在 Web Root 外/独立对象存储，必要时查毒；这是 OWASP 文件上传指南的核心建议：[File Upload Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/File_Upload_Cheat_Sheet.html)。
- Data Gateway 禁止内网、metadata IP、非 HTTP(S) scheme 和重定向绕过，优先 allowlist；参考 [OWASP SSRF Prevention](https://cheatsheetseries.owasp.org/cheatsheets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet.html)。
- API 使用短期 access token、细粒度 RBAC、租户隔离、速率限制、审计和 secret manager；数据库不保存第三方 API 明文密钥。
- 防供应链污染：锁定依赖、生成 SBOM、组件包签名/审核，发布只引用平台登记过的物料版本。

## 推荐技术栈

### 默认 React/TypeScript 方案

| 层 | 推荐 |
| --- | --- |
| 管理后台/编辑器 | React + TypeScript + Next.js；Ant Design 或内部设计系统 |
| 编辑器内核 | **Puck 二次开发**，或自研组件树编辑内核；若要自由 HTML/CSS 则 GrapesJS |
| 拖拽/排序 | Puck 内建能力；自研时使用 dnd-kit |
| 编辑状态 | Zustand/Redux Toolkit + Immer；服务端状态 TanStack Query |
| 属性表单 | React Hook Form + JSON Schema 驱动的 field registry |
| 富文本 | Tiptap（输出受限 JSON/HTML） |
| 页面协议校验 | JSON Schema + Ajv；TypeScript 类型可由 schema 生成或统一维护 |
| 运行时 | React renderer；公共页面用 Next.js 预渲染/SSG 或独立 Vite runtime |
| API | NestJS + OpenAPI；对外事件/埋点可用更轻量独立服务 |
| 数据 | PostgreSQL、Redis、S3/OSS/COS、ClickHouse（分析） |
| 异步任务 | BullMQ 起步；高规模后 Kafka/RabbitMQ |
| 可观测性 | OpenTelemetry + Prometheus/Grafana + Loki/ELK + Sentry；OpenTelemetry 可统一生成/采集/导出 traces、metrics、logs：[官方文档](https://opentelemetry.io/docs/) |
| 测试 | Vitest、Testing Library、Playwright、Storybook、视觉回归 |
| 工程 | pnpm、Turborepo、Docker、Kubernetes（确有规模后）、GitHub Actions/GitLab CI |

如果团队主要是 Vue，可将编辑器替换为 Vue 3 + Vite + Pinia，后端和协议不变；但不要为了复用 Vue 2 时代的 H5 项目长期锁在 Vue 2。

### 为什么默认推荐 Puck，而不是直接 fork 一个完整 H5 项目

Puck 是 React 组件化、MIT、数据自持的编辑器框架，官方提供 Next.js/静态页面 recipe，并把编辑器和 `Render` 运行时分开，适合受控组件型活动平台：[官方仓库](https://github.com/puckeditor/puck)。它不会替你完成租户、权限、素材、发布、表单、审核和统计，但这恰好允许平台按自己的领域模型建设。

若产品要求任意 HTML/CSS、导出网页或邮件模板，GrapesJS 更成熟：它提供 Component、Style、Asset、Page、Storage 等模块，项目数据应以 JSON 持久化，并支持远端存储和插件扩展：[官方文档](https://grapesjs.com/docs/)、[Storage Manager](https://grapesjs.com/docs/modules/Storage.html)。代价是要把其自由 DOM/CSS 模型约束为你的业务组件、数据绑定和安全规则。

## MVP 范围与实施路线

### Phase 0：2 周，技术验证

- 用 Puck 或候选内核实现 8 个组件：容器、文本、图片、按钮、轮播、倒计时、表单、吸底栏。
- 验证 iframe 编辑、schema 保存/恢复、移动端预览、同一 renderer 发布。
- 验证一个旧组件 schema 迁移、一键回滚、页面资源按需加载。
- 用真实运营同学完成一次从模板到发布的可用性测试。

退出条件不是“能拖动”，而是：同一文档编辑/预览/线上一致；旧版本可回滚；首屏性能满足目标；表单数据可审计。

### Phase 1：6–10 周，内部 MVP

- 项目/页面/模板、20–30 个核心组件、素材库、草稿自动保存、撤销重做。
- RBAC、预览、审核（可简化为发布权限）、静态发布、CDN、域名/路由。
- 表单、微信分享、基础埋点、发布记录和回滚。
- Playwright 发布 smoke test、组件 Storybook 和视觉回归。

### Phase 2：6–8 周，生产化

- 组件版本治理、schema 迁移、设计 token/品牌主题、区块模板。
- 图片流水线、性能预算、灰度发布、监控告警、审计和数据合规。
- 表单风控、导出权限、活动高峰压测与容量预案。

### Phase 3：按业务收益扩展

- 多人协作：Yjs 提供可并发合并的共享类型，Awareness 可表达在线状态/光标且不需要持久化；可参考 [Yjs 官方文档](https://docs.yjs.dev/) 和 [Awareness 文档](https://docs.yjs.dev/getting-started/adding-awareness)。先解决单人编辑和乐观锁，再上 CRDT。
- A/B 实验、个性化、审批流、跨品牌物料市场、多语言、AI 生成初稿。
- AI 输出必须仍落到相同 PageDocument schema，经过组件白名单、资源和 URL 校验，不能绕开发布审核。

## 团队配置与粗略工作量

内部 MVP 的合理最小团队：1 名产品/业务分析、1 名 UX/交互设计、2 名偏编辑器的前端、1 名运行时/性能前端、2 名后端、1 名 QA（可部分投入）、0.5 名 DevOps/安全支持。已有统一登录、对象存储、CDN、埋点平台时，6–10 周可出内部 MVP；若还包含抽奖、优惠券、多人协作、SaaS 计费和完整审批，通常是持续 6–12 个月的平台工程。

不要用“页面组件数量”估算全部成本。真正长期的成本在：schema/组件升级兼容、发布可靠性、运营误配置防护、素材与数据合规、线上性能以及活动高峰容量。

## 建议的决策结论

在没有更多业务约束时，推荐：

1. **采用 Puck 作为编辑器底座做 PoC，平台层自研。** React/TypeScript 技术统一，组件模型可控，许可证宽松。
2. **页面采用版本化 JSON Schema，线上 renderer 与编辑器共用组件注册表。** 发布时生成不可变静态 artifact，上 CDN，别让线上页面实时读取可变草稿。
3. **流式响应式布局为主，绝对定位仅作装饰容器。** 这比复制传统“易企秀式”全绝对定位更适合长期业务组件。
4. **MVP 做模块化单体 + 独立 Worker/埋点入口。** PostgreSQL、Redis、对象存储足够；不要首期拆十几个微服务。
5. **首期不开放任意 JS/HTML/代理 URL，也不做实时协作。** 先建立组件协议、版本迁移、发布回滚和安全边界。
6. 若 PoC 证明业务必须支持高度自由的 HTML/CSS，再将候选切换为 **GrapesJS**；若只是后台表单/表格页面，则应改用 **amis**，而不是继续把营销编辑器做成通用应用低代码。
