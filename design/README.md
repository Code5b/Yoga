# Yoga Design System V3

Yoga 设计系统第三版 — 适度圆角 · 紧凑间距 · 高级专业 · 沉浸体验。
严格遵循 PRD（用户端 `docs/PRD-USER.md`、管理端 `docs/PRD-ADMIN-KMP.md`），是当前唯一的正式设计系统交付物（V1/V2 已删除）。

## 设计层级

```text
LT Design System（母系统）
        ↓
Yoga Tokens V3
        ↓
Core Components
        ↓
Yoga Business Components
        ↓
Patterns
        ↓
User / Admin Mobile UI
```

## V3 核心变化

- 业务组件圆角收敛为 4 / 6 / 8 / 12px：标签/输入 4，按钮/次要卡片 6，标准卡片 8，沉浸容器/底部抽屉 12；状态点与头像全圆。拒绝过度圆润。
- 间距 2px 起步的紧凑基线：页面安全边距 16px，核心卡片内间距 14px，常用纵向间距 6~10px，区块间距 22px，减少无意义留白。
- 暖灰中性色阶负责高级感（ink-950 `#1B1B19`、背景 `#F6F5F2`、边框 `#E9E6DF`）；品牌黄 LT `#FFD600` 仅作强调色，不大面积铺色。
- 阴影暖调、分层、克制：1px 基础描边 + 轻阴影，沉浸层才使用重阴影。
- 中文优先（PingFang SC / Noto Sans SC），数字统一 tabular-nums。
- 禁止 Emoji 图标；建立统一 SVG 图标资源库（24px 网格 · 1.8px 描边 · 64 枚），全部 UI 通过 `icons/sprite.svg` 引用。
- 用户端遵循微信小程序信息架构（首页/约课/训练/商城/我的）；管理端严格为移动端 KMP，不做 Web/Desktop。
- 沉浸式体验集中在课程摄影、训练状态、教练内容、底部抽屉、Sticky Action 与业务反馈，而非无意义大留白。

## 入口

- `index.html`：设计系统总入口（展板 + 各页导航）
- `board.html`：一站式设计展板（品牌/色板/字体/控件/业务卡片/小组件/真机预览/通知）
- `components.html`：组件与状态示例（课程 7 态 · 会员 4 态 · 任务 P0-P3）
- `user-showcase.html`：用户端微信小程序沉浸式示例（含微信胶囊）
- `subpages.html`：用户端二三四级页面合集 21 屏（课程/教练/商品/购物车/支付/订单/售后/权益/数字课程/记录/任务/打卡日历/优惠券/收藏/消息/客服/设置/资料/签到码等）
- `admin-showcase.html`：管理端移动经营工作台示例
- `admin-subpages.html`：管理端二级页面 10 屏（会员详情/新增会员/新建课程/课程详情/商品/库存/收入/报表/签到记录/门店设置）
- `tokens.css`：Design Tokens + 组件原语
- `icons/`：专业 SVG 图标资源库（sprite.svg + 64 独立图标 + 规范；各页面已内联 sprite，双击 file:// 打开亦可正常显示）

## 产品依据

用户端围绕 PRD 的用户生命周期、首页信息优先级、课程状态、预约资格校验、签到消课、会员权益、购买、订单/售后、商品关联等场景设计；管理端围绕"今天应该做什么"的运营工作台、会员画像、CRM、排课、签到、续费、沉默召回、商品和任务闭环设计。数字课程遵循 P3 路线。

## 预览

```bash
python3 -m http.server 8080 --directory design/yoga-design-v3
# 打开 http://localhost:8080
```