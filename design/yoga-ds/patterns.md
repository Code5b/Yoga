# Yoga Patterns 示例

## 用户端：首页

```text
微信状态栏
  ↓
自定义导航栏：Yoga + 微信胶囊安全区
  ↓
问候 / 会员状态
  ↓
今日课程 Hero Card
  ↓
快速约课
  ↓
我的练习：连续训练 / 剩余权益 / 本周训练
  ↓
推荐课程 + 教练
  ↓
Bottom TabBar
```

## 用户端：预约

```text
Course Card
 → Course Detail
 → 日期/场次选择
 → Entitlement 检查
 → Booking Confirm
 → Payment（如需支付）
 → Success
```

## 用户端：会员购买

```text
Membership Product
 → 权益明细
 → 价格 / 优惠
 → Order Confirm
 → WeChat Pay
 → Purchase Success
```

## 用户端：签到

```text
预约成功
 → 到店
 → Check-in
 → Entitlement Consume
 → Booking Completed
```

## 用户端：退款

```text
Order Detail
 → Refund Apply
 → Refund Reason
 → Refund Amount
 → Pending Review / Processing
 → Refunded / Rejected
```

## 管理端：今日经营

```text
Today KPI
 → Today Tasks
 → Course Schedule
 → Member Follow-up
 → Revenue / Orders
```

## 管理端：会员经营

```text
Member List
 → Member Detail
 → Training / Entitlement / Orders / Timeline
 → Next Best Action
 → Follow-up Task
 → Renewal / Course Recommendation
```

## 管理端：排课

```text
Schedule
 → Course Detail
 → Booking List
 → Change Coach / Capacity / Time
 → Notify Members
```

## 管理端：商品

```text
Product List
 → Product Editor
 → Training Goal Tags
 → Inventory
 → Save
```

> 所有 Pattern 只组合已登记组件；页面禁止自行创造与 Design System 冲突的视觉样式。
