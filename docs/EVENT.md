# V8 事件与运营规则

## 1. 模型

Yoga 的运营自动化采用：

```text
Domain Event
    ↓
Rule Evaluation
    ↓
Action
    ↓
CRM Task / Recommendation / Message
```

## 2. 核心事件

```text
MEMBER_REGISTERED
TRIAL_BOOKED
TRIAL_ATTENDED
MEMBERSHIP_PURCHASED
BOOKING_CREATED
BOOKING_CANCELLED
CLASS_ATTENDED
TRAINING_COMPLETED
TRAINING_STREAK_BROKEN
ORDER_PAID
PRODUCT_PURCHASED
MEMBERSHIP_EXPIRING
MEMBER_INACTIVE
PRIVATE_PACKAGE_LOW_BALANCE
```

事件必须包含：eventId、type、occurredAt、aggregateType、aggregateId、memberId、payload、version。

## 3. Outbox

业务事务内写入 `event_outbox`，事务提交后异步投递。消费者必须按 eventId 幂等处理。

## 4. 第一阶段规则示例

### 沉默召回

```text
WHEN member.last_training_at <= now - 14 days
AND member.status = ACTIVE
THEN create CRM task: TRAINING_RECALL
```

### 到期提醒

```text
WHEN membership.valid_to <= now + 7 days
AND membership.status = ACTIVE
THEN create CRM task: RENEWAL
```

### 私教增购

```text
WHEN private_package.remaining <= 2
THEN create CRM task: PRIVATE_RENEWAL
```

### 训练商品推荐

```text
WHEN training_goal = FLEXIBILITY
AND user.has_not_purchased(YOGA_BLOCK)
THEN recommend YOGA_BLOCK
```

## 5. 规则设计原则

- 规则可开关；
- 规则有版本；
- 每次命中保存 rule_execution；
- 同一事件不能无限重复创建任务；
- 规则产生的动作必须可追溯；
- V8 先做确定性规则，不引入 AI 黑盒推荐。

## 6. 推荐引擎

第一阶段只实现 `Next Best Action`：

```text
会员行为
 ↓
会员画像
 ↓
规则匹配
 ↓
推荐候选
 ↓
去重/频控
 ↓
推荐
```

以后有足够数据再引入模型，不提前把 AI 当核心依赖。
