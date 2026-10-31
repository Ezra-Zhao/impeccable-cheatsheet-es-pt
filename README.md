# Impeccable 速查表 | Impeccable Cheatsheet | Guía rápida de Impeccable

> 一页看懂 Impeccable：给编程 agent 装上的一套「设计词汇表」——你不再只能含糊地说「弄好看一点」，而是能下达 polish、bolder、critique、audit 这类精确指令，每条指令背后都是一份写好的操作手册；外加一个不靠大模型、纯规则的「AI 味」检测器，在出货前把模板脸拦下来。

- 上游项目：[pbakaus/impeccable](https://github.com/pbakaus/impeccable)（2026-10-31 实测 98,160 ★ / 5,787 forks，93 个 release，Apache-2.0）
- 平台：以 skill／插件形态装进编程工具（Claude Code、Cursor、Codex CLI、GitHub Copilot、Gemini CLI、OpenCode 等二十余种），另有独立 CLI 与 Chrome 扩展
- 本页结构：中文主文 → English → Español → Português

---

Apoyo y más: [GitHub Sponsors](https://github.com/sponsors/Ezra-Zhao) · [Gumroad — plantillas y cuadernos que uso](https://ezraflow28.gumroad.com) · [Ko-fi](https://ko-fi.com/ezra777zhao)

## 它要解决的问题

什么都不交代就让 agent 写页面，出来的总是同一张脸：Inter 字体打天下、紫到蓝的渐变、卡片里套卡片、彩色底上放灰字、每个标题上方一块圆角图标砖。这些不是某个模型的毛病，是所有模型的默认审美。Impeccable 的解法是给 agent 补三样东西：

| 部件 | 一句话说明 |
|---|---|
| 24 条设计命令 | 每条命令对应一个具体动作的操作手册：你点名动作和对象，agent 不再靠猜 |
| 两份项目档案 | `PRODUCT.md` 记产品定位与约束，`DESIGN.md` 记你现有的视觉系统，之后每条命令都先读它们，不跑偏 |
| AI 味检测器 | 62 条确定性规则（不调用大模型、不需要 API key）专抓上面那张「模板脸」，外加挤压的内边距、跳级的标题这类硬伤 |

## 24 条命令速查

用法统一：`/impeccable <命令> <对象>`（Codex 里用 `$impeccable` 或在 `/skills` 里打开）。对象越窄越好——审一个组件，胜过扫一整页。

| 组 | 命令 |
|---|---|
| 驾驶（9） | `bolder` 把太稳重的设计放大胆 · `quieter` 把太吵的收一收 · `distill` 删掉页面不需要的东西 · `colorize` 给灰扑扑的界面上色 · `typeset` 修字体、层级与字号 · `layout` 修间距与节奏 · `animate` 加过渡与交互反馈 · `delight` 加性格与小惊喜 · `overdrive` 冲效果上限（着色器、弹簧物理、滚动驱动效果） |
| 评审（2） | `critique` 带评分的 UX 评审，按可用性启发式排优先级 · `audit` 技术质量报告（无障碍、性能、主题、响应式），问题按 P0（阻断）到 P3（打磨）分级 |
| 收尾（6） | `polish` 出货前最后一遍 · `harden` 补错误态、国际化、溢出与边界情况 · `clarify` 重写说不清楚的文案 · `adapt` 适配各种屏幕与设备 · `optimize` 修加载与渲染性能 · `onboard` 设计首次使用流程与空状态 |
| 实时迭代（2） | `live` 在浏览器里点选元素、现场轮换变体，选定即写回源码 · `generate` 不用手点，直接给指定元素生成一组变体 |
| 规划与建档（4） | `shape` 写代码之前先规划 UX · `init` 生成 `PRODUCT.md` · `document` 生成 `DESIGN.md` · `extract` 把重复出现的模式收进设计系统 |

第 24 条 `craft` 是已弃用的旧别名（等同于普通「直接开做」请求）；只输入 `/impeccable` 不带命令，会列出全部命令。常用命令可以钉住：`/impeccable pin audit` 之后就有独立的 `/audit` 快捷入口。

## 先建档，再动手

1. 每个项目只跑一次 `/impeccable init`：它读你的代码，只问它推断不出来的（产品给谁用、要解决什么、有什么约束），写成 `PRODUCT.md`。
2. 已有视觉系统的项目再跑 `/impeccable document`，把现状固化成 `DESIGN.md`，后面的改动才不会越改越不像自家的东西。
3. 然后才是点名施工：`/impeccable critique the pricing cards`、`/impeccable polish settings` 这样一条一条来。

## 检测器三跑法

| 跑法 | 怎么用 | 说明 |
|---|---|---|
| 装进 agent（推荐） | 安装时同意装 design hook | agent 每改一个 UI 文件，检测结果在它「完工」之前就递回去——大多数 AI 味是在这一步被拦下的 |
| CLI／CI | `npx impeccable detect src/` 扫目录；`npx impeccable detect https://…` 扫渲染后的页面（借用本机 Chrome/Edge）；`--json` 出机器可读结果；`--github` 出 GitHub Actions 标注；`--list-rules` 列全部规则 | 退出码 2 = 有发现，1 = 目标扫不了。源码文件（JSX/TSX）只跑模式类规则；对比度过低、字太小这类要渲染后才看得出的，必须扫 URL（本地 dev server 也行） |
| Chrome 扩展 | 官方扩展把同样的发现以浮层形式盖在任意线上页面上 | 自家和别家的页面都能看 |

误报豁免：整条规则放行用 `npx impeccable ignores add-value overused-font Inter --reason "Brand font"`（比如 Inter 就是你的品牌字体）；只对单个文件关一条规则，在文件里加注释 `<!-- impeccable-disable overused-font: 理由 -->`。

## 安装一行版

| 方式 | 命令／入口 |
|---|---|
| 通用安装器 | 项目根目录跑 `npx impeccable install`：自动识别你的编程工具，问装到项目还是全局，能装 hook 的工具会 offer 设计 hook——选装。装完重载工具。更新用 `npx impeccable update` |
| 脚本化安装 | `npx impeccable install --providers=claude,codex,cursor --scope=project` |
| Claude Code 插件 | `/plugin marketplace add pbakaus/impeccable` 后在 `/plugin` 里安装 |
| 团队钉版 | `git submodule add https://github.com/pbakaus/impeccable vendor/impeccable` 再 `npx impeccable link --source=vendor/impeccable --providers=claude,cursor`（只装 skill，不含 hook） |
| 手工拷贝 | 从官网下对应工具的 ZIP，把 `skills/impeccable` 拷进项目的对应目录（如 Claude Code 是 `.claude/skills/`）——免 Node，但同样不含 hook 与子代理 |

只有 `npx` 需要 Node；skill 本体跑的是自带引擎二进制（首次会下载到 `~/.impeccable/bin/`）。三个工具的 hook 要多走一步：Codex 在 `/hooks` 里批准项目 hook；Grok Build 跑 `/hooks-trust` 或带 `--trust` 启动；Hermes Agent 跑一次 `hermes plugins enable impeccable-design-hook`。

## 诚实标注区

- **主场是「改」不是「造」**：官方明说它最擅长对已有页面/组件迭代；从零建新页面也能做，但那是更难的模式，官网另有专门打法说明。
- **窄目标才有效**：一个组件审透，好过整页扫一眼——这不是客气话，是它工作方式决定的。
- **live 模式只碰本地代码**：不要往已部署的站点里注入，也不要为了让它工作去削弱生产环境的 CSP；应用文案编辑时会以你的权限跑 `package.json` 里可选的校验脚本，陌生仓库里先看清再用。
- **hook 是拦截的关键**：手工拷贝和子模块方式都不带 hook，检测就变成「想起来才跑」；能装安装器就装安装器。
- **检测器只管「形」**：62 条规则抓的是设计通病与硬伤，抓不了产品逻辑对不对、文案是不是胡说——评审命令（critique/audit）也替代不了真人走查。

---

## English (summary)

Impeccable (by pbakaus, ~98.2k stars as of 2026-10-31, Apache-2.0) is the design vocabulary for your coding agent. Instead of "make it pop", you name a move and a target: `/impeccable bolder the pricing cards`. Its 24 commands fall into five groups — Steer (bolder, quieter, distill, colorize, typeset, layout, animate, delight, overdrive), Review (critique, a scored UX review; audit, a technical report with P0–P3 findings), Finish (polish, harden, clarify, adapt, optimize, onboard), Iterate live (live, generate), and Plan/set up (shape, init, document, extract) — plus `craft`, a deprecated alias. Run `/impeccable init` once per project to write PRODUCT.md, and `/impeccable document` to capture your visual system in DESIGN.md; every command reads them.

The slop detector runs 62 deterministic rules — no LLM, no API key — and works three ways: as a design hook inside your agent (Claude Code, Cursor, Codex, Copilot, Grok Build, Hermes Agent), as a CLI (`npx impeccable detect src/`, a URL, `--json`, `--github`), and as a Chrome extension overlay. Install with `npx impeccable install` from your project root and accept the hook; Node is only needed for npx.

Honest notes: it is strongest on interfaces that already exist, narrow targets beat whole-page sweeps, live mode is for local checkouts only, and manual/submodule installs skip the hook — which is where most slop actually gets caught.

---

## Español (guía rápida — traducción de estudio, pendiente de revisión por un hablante nativo)

Impeccable (de pbakaus, ~98,2 mil estrellas en octubre de 2026, Apache-2.0) es el vocabulario de diseño para tu agente de código. En lugar de decir «que se vea bonito», nombras el movimiento y el objetivo: `/impeccable bolder the pricing cards`. Cada comando carga un manual escrito para ese movimiento concreto.

**Los 24 comandos, por grupos:**

- **Dirección (9)**: `bolder` (amplificar un diseño tímido), `quieter` (calmar uno ruidoso), `distill` (quitar lo que la página no necesita), `colorize` (dar color a una interfaz gris), `typeset` (fuentes, jerarquía y tamaños), `layout` (espaciado y ritmo), `animate` (transiciones y respuesta a la interacción), `delight` (personalidad), `overdrive` (efectos avanzados: shaders, física de muelles, scroll).
- **Revisión (2)**: `critique` (revisión UX con puntuación y prioridades) y `audit` (informe técnico: accesibilidad, rendimiento, temas, responsive; hallazgos de P0 a P3).
- **Acabado (6)**: `polish` (última pasada antes de publicar), `harden` (errores, i18n, desbordes y casos borde), `clarify` (reescribir textos confusos), `adapt` (pantallas y dispositivos), `optimize` (rendimiento de carga y render), `onboard` (primer uso y estados vacíos).
- **Iteración en vivo (2)**: `live` (elegir un elemento en el navegador y rotar variantes, la elegida se escribe en el código) y `generate` (variantes sin selección manual).
- **Planificación (4)**: `shape` (planear la UX antes del código), `init` (escribe PRODUCT.md), `document` (escribe DESIGN.md), `extract` (patrones repetidos al sistema de diseño). El 24.º, `craft`, es un alias obsoleto.

**Antes de tocar nada**: ejecuta `/impeccable init` una vez por proyecto (lee tu código y escribe PRODUCT.md con lo que no puede inferir) y `/impeccable document` si ya tienes un sistema visual (DESIGN.md). Todos los comandos posteriores leen esos archivos.

**El detector de «AI slop»**: 62 reglas deterministas, sin LLM y sin clave API. Tres formas de ejecutarlo: como hook dentro del agente (escanea cada archivo de UI que el agente edita, antes de que dé el trabajo por terminado), como CLI (`npx impeccable detect src/`, o una URL para las reglas que necesitan la página renderizada; `--json`, `--github`, `--list-rules`; código de salida 2 = hay hallazgos) y como extensión de Chrome con superposición visual. Excepciones: `npx impeccable ignores add-value overused-font Inter --reason "Brand font"` o el comentario `<!-- impeccable-disable ... -->` en un archivo.

**Instalación**: `npx impeccable install` en la raíz del proyecto (detecta tu herramienta, acepta el hook), `npx impeccable update` para actualizar. Funciona con Claude Code, Cursor, Codex CLI, GitHub Copilot, Gemini CLI, OpenCode y más de veinte herramientas.

**Notas honestas**: su terreno fuerte es iterar sobre interfaces que ya existen; los objetivos estrechos dan mejores resultados que barrer una página entera; el modo live es solo para tu copia local (nada de inyectarlo en sitios desplegados); la copia manual o por submódulo no incluye el hook, que es donde se caza casi todo el slop.

---

## Português (guia rápido — tradução de estudo, aguardando revisão de um falante nativo)

O Impeccable (de pbakaus, ~98,2 mil estrelas em outubro de 2026, Apache-2.0) é o vocabulário de design para o seu agente de código. Em vez de dizer «deixa mais bonito», você nomeia o movimento e o alvo: `/impeccable bolder the pricing cards`. Cada comando carrega um manual escrito para aquele movimento específico.

**Os 24 comandos, por grupo:**

- **Direção (9)**: `bolder` (ampliar um design tímido), `quieter` (acalmar um ruidoso), `distill` (remover o que a página não precisa), `colorize` (dar cor a uma interface cinza), `typeset` (fontes, hierarquia e tamanhos), `layout` (espaçamento e ritmo), `animate` (transições e resposta à interação), `delight` (personalidade), `overdrive` (efeitos avançados: shaders, física de molas, scroll).
- **Revisão (2)**: `critique` (revisão de UX com nota e prioridades) e `audit` (relatório técnico: acessibilidade, desempenho, temas, responsivo; achados de P0 a P3).
- **Acabamento (6)**: `polish` (última passada antes de publicar), `harden` (erros, i18n, estouros e casos extremos), `clarify` (reescrever textos confusos), `adapt` (telas e dispositivos), `optimize` (desempenho de carga e renderização), `onboard` (primeiro uso e estados vazios).
- **Iteração ao vivo (2)**: `live` (escolher um elemento no navegador e alternar variantes; a escolhida é escrita no código) e `generate` (variantes sem seleção manual).
- **Planejamento (4)**: `shape` (planejar a UX antes do código), `init` (escreve o PRODUCT.md), `document` (escreve o DESIGN.md), `extract` (padrões repetidos para o sistema de design). O 24.º, `craft`, é um alias obsoleto.

**Antes de mexer em qualquer coisa**: rode `/impeccable init` uma vez por projeto (lê o seu código e escreve o PRODUCT.md com o que não consegue inferir) e `/impeccable document` se você já tem um sistema visual (DESIGN.md). Todos os comandos seguintes leem esses arquivos.

**O detector de «AI slop»**: 62 regras determinísticas, sem LLM e sem chave de API. Três jeitos de rodar: como hook dentro do agente (escaneia cada arquivo de UI que o agente edita, antes de ele dar o trabalho por pronto), como CLI (`npx impeccable detect src/`, ou uma URL para as regras que precisam da página renderizada; `--json`, `--github`, `--list-rules`; código de saída 2 = há achados) e como extensão do Chrome com sobreposição visual. Exceções: `npx impeccable ignores add-value overused-font Inter --reason "Brand font"` ou o comentário `<!-- impeccable-disable ... -->` em um arquivo.

**Instalação**: `npx impeccable install` na raiz do projeto (detecta a sua ferramenta, aceite o hook) e `npx impeccable update` para atualizar. Funciona com Claude Code, Cursor, Codex CLI, GitHub Copilot, Gemini CLI, OpenCode e mais de vinte ferramentas.

**Notas honestas**: o ponto forte é iterar sobre interfaces que já existem; alvos estreitos dão resultados melhores do que varrer uma página inteira; o modo live é só para a sua cópia local (nada de injetar em sites publicados); a cópia manual ou por submódulo não inclui o hook, que é onde quase todo o slop é capturado.

---

## 来源与声明

- 灵感来源 / Source / Fuente / Fonte: [pbakaus/impeccable](https://github.com/pbakaus/impeccable)（官网 impeccable.style）
- 本仓是个人学习整理：内容用我自己的话总结自上游公开文档，不搬运原文；命令与规则数以 2026-10-31 的上游 README 为准，上游迭代很快，请以官方文档为最终依据。
- 西语、葡语部分为学习译稿，待母语校对。Las secciones en español y portugués son traducciones de estudio pendientes de revisión nativa.
- License: MIT © 2026 Guangyi Zhao
