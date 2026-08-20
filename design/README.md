# Yoga Commercial Design V2

当前 `design/` 已完成一次彻底视觉重构。

## 设计层级

```text
LT Design System（母系统）
        ↓
Yoga Tokens V2
        ↓
Core Components
        ↓
Yoga Business Components
        ↓
Patterns
        ↓
User / Admin Mobile UI
```

## V2 核心变化

- 业务组件圆角从过度圆润调整为 6 / 8 / 10 / 12px；仅沉浸式容器使用 16px，胶囊保持 999px。
- 页面安全边距统一 16px，核心卡片内间距 12~16px，常用纵向间距 8~12px，减少无意义留白。
- LT `#FFD600` 保留为品牌强调色，不再大面积铺色；中性灰阶、细边框和轻阴影负责高级感。
- 中文优先，禁止 Emoji 图标。
- 建立统一 SVG 图标资源库，所有 UI 通过 `icons/sprite.svg` 引用。
- 用户端遵循微信小程序信息架构；管理端严格为移动端 KMP，不做 Web/Desktop。
- 沉浸式体验集中在课程摄影、训练状态、教练内容、底部抽屉、Sticky Action 与业务反馈，而不是无意义的大留白。

## V2 入口

- `yoga-ds-v2/index.html`：设计系统入口
- `yoga-ds-v2/components.html`：组件与状态示例
- `yoga-ds-v2/user-showcase.html`：用户端微信小程序示例
- `yoga-ds-v2/admin-showcase.html`：管理端移动经营工作台示例
- `yoga-ds-v2/tokens.css`：Design Tokens
- `yoga-ds-v2/icons/`：专业 SVG 图标资源
- `yoga-ds-v2/patterns.md`：业务 Pattern

## 产品依据

用户端严格围绕 PRD 的用户生命周期、首页信息优先级、课程状态、预约资格校验、签到消课、会员权益、购买、订单/售后、商品关联等场景设计；管理端围绕“今天应该做什么”的运营工作台、会员画像、CRM、排课、签到、续费、沉默召回、商品和任务闭环设计。数字课程继续遵循 P3 路线。

## 预览

```bash
python3 -m http.server 8080 --directory design/yoga-ds-v2
```

然后访问 `http://localhost:8080/`。
