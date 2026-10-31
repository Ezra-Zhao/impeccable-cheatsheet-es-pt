# 出货前四步演练 | Pre-ship design pass

一个已有页面准备发布前，按这个顺序走一遍（目标都点名到组件，不要整页扫）：

1. 建档（只做一次）：项目根目录 `npx impeccable install` 并同意装 hook；进 agent 跑 `/impeccable init`，已有视觉系统再跑 `/impeccable document`。
2. 先评审后动手：`/impeccable critique the landing hero` 看评分与优先级清单，P0/P1 先记下，不急着全改。
3. 点名施工：按清单逐条下令，例如 `/impeccable typeset the pricing cards`、`/impeccable quieter the landing page`；一次只改一个对象，改完看一眼再下一条。
4. 收尾双检：`/impeccable polish the checkout flow` 做最后一遍；再在终端跑 `npx impeccable detect http://localhost:5173` 扫渲染后的页面（对比度、字号这类规则只有扫 URL 才跑得到），退出码 2 就回清单逐条处理；品牌字体之类的正当例外用 `ignores add-value` 登记理由，不要直接关规则。
