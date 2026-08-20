# Yoga Commercial Design

当前设计目录分为两层：

1. `reference/lt-design-system/`：用户提供的 LT Design System 源参考，作为唯一视觉基准。
2. `yoga-ds/`：在 LT Token 和组件语义之上建立的瑜伽业务设计系统。

## 核心原则

**继承 LT，不重新发明视觉。**

- 主色：`#FFD600`
- 页面背景：`#F5F5F5`
- 卡片：`#FFFFFF`
- 主文字：`#1A1A1A`
- 状态：Success `#07C160` / Warning `#FF9500` / Error `#FF4444` / Info `#3B82F6`
- 圆角：6 / 10 / 16px
- 间距：4 / 6 / 8 / 10 / 12 / 14 / 16 / 20 / 24px
- 中文字体优先 PingFang SC / 系统字体

## Yoga Design System

查看：`yoga-ds/README.md`

Token：`yoga-ds/yoga-tokens.css`

组件注册表：`yoga-ds/component-registry.json`

组件示例：`yoga-ds/components.html`

业务 Pattern：`yoga-ds/patterns.md`

## 产品范围

### 用户端
微信小程序移动端：登录、首页、约课、课程、教练、会员、权益、训练记录、订单、支付、退款、签到、训练装备。

### 管理端
仅移动端：今日经营、排课、会员、交易、商品、库存、运营任务。

### 数字课程
P3 才进入当前产品路线，不进入现阶段核心导航。

## 微信小程序导航

导航栏必须为独立组件，并预留微信真实胶囊位置。生产实现使用 `wx.getMenuButtonBoundingClientRect()` 与状态栏高度动态计算，不把胶囊当作普通业务按钮。

## 商用设计要求

页面必须由登记在 `component-registry.json` 的组件和 Pattern 组合而成；每个关键业务组件需要覆盖正常、按压、禁用、Loading、Empty、Error 以及对应业务状态。

## 预览

```bash
python3 -m http.server 8080 --directory design
```
