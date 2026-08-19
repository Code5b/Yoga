# V8 统一错误码

## 1. 格式

错误码使用稳定字符串，避免直接暴露数据库错误。

```text
DOMAIN_REASON
```

## 2. 通用

| Code | 含义 |
|---|---|
| SUCCESS | 成功 |
| INVALID_REQUEST | 请求参数错误 |
| UNAUTHORIZED | 未认证 |
| FORBIDDEN | 无权限 |
| NOT_FOUND | 资源不存在 |
| CONFLICT | 资源冲突 |
| IDEMPOTENCY_CONFLICT | 幂等键与请求不匹配 |
| INVALID_STATE_TRANSITION | 非法状态迁移 |
| RATE_LIMITED | 请求过于频繁 |
| INTERNAL_ERROR | 内部错误 |

## 3. 会员/权益

| Code | 含义 |
|---|---|
| MEMBER_DISABLED | 会员不可用 |
| ENTITLEMENT_NOT_FOUND | 权益不存在 |
| ENTITLEMENT_EXPIRED | 权益已过期 |
| ENTITLEMENT_INSUFFICIENT | 权益不足 |
| ENTITLEMENT_FROZEN | 权益被冻结 |

## 4. 预约

| Code | 含义 |
|---|---|
| SCHEDULE_FULL | 课程已满 |
| BOOKING_NOT_ALLOWED | 当前不可预约 |
| BOOKING_ALREADY_EXISTS | 已存在预约 |
| BOOKING_NOT_CANCELLABLE | 当前不可取消 |
| BOOKING_NOT_CHECKINABLE | 当前不可签到 |

## 5. 订单/支付

| Code | 含义 |
|---|---|
| ORDER_NOT_PAYABLE | 订单不可支付 |
| PAYMENT_FAILED | 支付失败 |
| PAYMENT_AMOUNT_MISMATCH | 支付金额不一致 |
| REFUND_NOT_ALLOWED | 当前不可退款 |
| REFUND_AMOUNT_EXCEEDED | 退款金额超过可退金额 |

## 6. 库存

| Code | 含义 |
|---|---|
| SKU_NOT_FOUND | SKU 不存在 |
| STOCK_INSUFFICIENT | 库存不足 |
| STOCK_RESERVATION_FAILED | 库存预占失败 |

## 7. 规则

客户端只展示用户可理解的 message；服务端日志保存 requestId、traceId 和详细异常，禁止把 SQL、堆栈或第三方支付原始报文直接返回客户端。
