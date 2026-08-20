# Yoga SVG Icon Library V3

统一图标资源库。**禁止使用 Emoji 或字符图标**，所有 UI 图标必须来自本目录。

## 风格规范

| 属性 | 值 |
|---|---|
| 网格 | 24 × 24 |
| 描边 | 1.8px |
| 端点 / 连接 | round / round |
| 色彩 | 仅 `currentColor`（随上下文着色） |
| 内边距 | 2px（绘制范围 2–22） |
| 数量 | 64 |

坐标系对齐半像素网格，圆角不超过 2px，图标保持光学居中。`style-spec.json` 为机器可读规范。

## 文件

- `sprite.svg`：**唯一推荐引用方式**，所有 UI 通过 `<use>` 引用：
  ```html
  <svg class="y-icon" aria-hidden="true"><use href="icons/sprite.svg#home"/></svg>
  ```
- `*.svg`：单图标文件，供 uni-app / KMP / 动态化等需要独立资源的平台使用。
- `preview.html`：可视化预览（含深色场景与品牌金色场景）。
- `style-spec.json`：图标风格机器规范。

## 分组

| 分组 | 图标 |
|---|---|
| 导航 | home, calendar, calendar-check, activity, shopping-bag, cart, user, users, more, close, arrow-left, chevron-right, chevron-down, menu |
| 课程 / 预约 | clock, check, check-circle, alert, info, hourglass, lock, star, map-pin, badge-check |
| 训练 / 沉浸 | play, pause, flame, target, trophy, repeat, timer, heart, book, lotus, sun |
| 交易 / 商城 | wallet, card, coupon, receipt, gift, package, truck, shield, plus, minus, trash |
| 管理 / 运营 | search, filter, edit, bell, task, chart, phone, message, scan, camera, trend-up, trend-down, flag, settings |
| 其他 | share, refresh, sparkle, crown |

## 使用原则

1. 图标为语义资源：可预约、已满、候补、无权益、时间冲突等状态用 `y-icon` + 语义色表达。
2. 尺寸：底部导航 22px、列表条目 20px、卡片内 18px、强调区 24px。
3. 只做描边图标，不做实心填充（除 `more` 圆点、`target` 中心点等辅助元素）。