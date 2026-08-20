# Yoga Design System

基于 LT Design System 构建的瑜伽小程序业务设计系统。原则：**不另起炉灶，不改变 LT 的核心视觉 Token，只把 LT 的视觉语言映射到瑜伽业务。**

## 继承关系

```text
LT Design System
  ├─ #FFD600 主色
  ├─ #F5F5F5 页面背景
  ├─ #FFFFFF 卡片
  ├─ #1A1A1A 主文字
  ├─ 6 / 10 / 16 圆角
  ├─ 4~24 间距
  ├─ PingFang SC / 系统中文字体
  ├─ 金色实心 / 描边 / 文本按钮
  ├─ Badge / Avatar / Search / TabBar
  └─ Card / Shadow / Status
          ↓
Yoga Design System
  ├─ 课程
  ├─ 教练
  ├─ 预约
  ├─ 会员权益
  ├─ 训练记录
  ├─ 签到消课
  ├─ 订单支付退款
  ├─ 商品与训练装备
  └─ 门店运营
```

## 视觉规则

1. **黄色是品牌强调色，不大面积铺屏。**
2. 页面背景保持 LT `#F5F5F5`，业务卡片使用 `#FFFFFF`。
3. 主标题使用 20px/700，正文 14px/400，辅助 12px。
4. 业务数字、价格、次数使用 LT Price Token。
5. 课程/教练/商品允许使用真实摄影，但图片必须服务于信息层级。
6. 所有业务状态必须使用 LT Status 色语义。
7. 微信小程序导航栏保留真实胶囊安全区域；胶囊属于导航层，不是业务按钮。
8. 管理端同样只做移动端，复用同一套 Token 与组件。

## 组件层级

### Foundation
Color / Typography / Spacing / Radius / Shadow / Icon / Safe Area

### Core Components
Button / Badge / Avatar / Search / TabBar / Input / Selector / Chip / Toast / Dialog / Bottom Sheet

### Yoga Components
Course Card / Coach Card / Schedule Cell / Booking Status / Membership Card / Entitlement / Training Progress / Check-in Card / Order Card / Refund Card / Product Card / Operation Task / Member Timeline

### Patterns
Home / Course Booking / Membership Purchase / Payment / Check-in / Refund / Schedule / Member Operation / Product Purchase

## 状态矩阵

每个关键业务组件至少覆盖：Default / Pressed / Disabled / Loading / Empty / Error，以及业务状态：Booked / Full / Waitlist / Expiring / Expired / Refunding / Refunded。

## P3

数字课程不进入当前核心导航。P3 才新增 Digital Course / Training Plan / Content Progress 等组件。
