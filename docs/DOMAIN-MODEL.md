# V8 领域模型

## 1. 核心原则

Yoga 的核心不是“会员卡”，而是 **用户拥有什么权益、这些权益如何被消费和履约**。

```text
User
 ↓
Member
 ↓
Order / Membership / Booking / Training
 ↓
Entitlement
 ↓
EntitlementTransaction
```

## 2. 核心聚合

### User

身份主体。负责登录身份、微信 OpenID/UnionID 等外部身份映射。

### Member

门店会员经营主体。保存会员状态、目标、标签、教练关系、来源等经营信息。

### Product

可销售的统一产品抽象：

```text
MEMBERSHIP
GROUP_CLASS
PRIVATE_PACKAGE
SMALL_GROUP
DIGITAL_COURSE
TRAINING_PLAN
LIVE
PHYSICAL_PRODUCT
BUNDLE
```

### Order

所有付费行为统一进入 Order → OrderItem。不要创建“课程订单”“商城订单”“会员订单”三套体系。

### Entitlement

订单履约后产生的权益。示例：10 次团课、5 次私教、21 天训练计划访问权、数字课程访问权。

核心字段概念：owner、type、source、quantity、used_quantity、valid_from、valid_to、status。

### Booking

用户对课程/私教/小班时段的预约。Booking 不直接代表扣课，真正的消费通过 EntitlementTransaction 完成。

### TrainingPlan

训练计划定义；发布后生成不可变版本 TrainingPlanVersion。用户购买后绑定具体版本。

### Content

视频/图文等数字内容。ContentVersion 用于保证历史购买内容稳定。

### Inventory

实物 SKU 的库存余额和不可逆库存流水。

### CRMTask

经营动作，不是会员属性。任务来源必须可以追溯到事件和规则。

## 3. 关键关系

```text
User 1 ── 1 Member
Member 1 ── N Order
Order 1 ── N OrderItem
Order 1 ── N Entitlement
Member 1 ── N Booking
Booking 1 ── N EntitlementTransaction
Member 1 ── N TrainingRecord
TrainingPlan 1 ── N TrainingPlanVersion
Product 1 ── N SKU
SKU 1 ── N InventoryTransaction
Member 1 ── N CRMTask
Event 1 ── N RuleExecution
RuleExecution 1 ── N CRMTask
```

## 4. 聚合规则

- Order 内部状态只能由 Order Application Service 修改。
- Entitlement 数量只能通过 EntitlementTransaction 改变，不允许直接更新 used_quantity。
- Inventory 只能通过 InventoryTransaction 形成变化。
- TrainingPlanVersion 发布后不可原地修改。
- Booking 的名额和状态变更必须带幂等键。
- Member 是经营画像，不承担交易状态。

## 5. 为什么使用 Entitlement

它统一解决：

```text
会员卡
次卡
私教包
小班包
数字课程
训练计划
赠送权益
套餐权益
```

因此未来新增工作坊、Retreat 或企业产品时，不需要重写订单和履约体系。
