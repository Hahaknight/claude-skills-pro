# 真实输出示例 · What the skills actually produce

描述会说谎，输出不会。以下是几个 skill 的典型输出形态（节选）。

---

## pr-reviewer

> 输入：`review my staged changes`

```
## Review: add refund endpoint for expired subscriptions

Verdict: REQUEST CHANGES

### Blockers
- src/api/refund.ts:42 — refund amount read from request body without
  currency check; a client can refund 100 in JPY on a USD charge.
  Fix: derive amount from the stored charge, not user input.

- src/api/refund.ts:67 — no idempotency: double-click retries create
  duplicate refunds. Fix: accept Idempotency-Key, dedupe on it.

### Should-fix
- src/api/refund.ts:88 — refunds >500 rows iterated one-by-one (N+1);
  batch with a single WHERE IN query.

### Nits
- src/api/refund.ts:15 — magic number 86400 → named const REFUND_WINDOW_S

### Questions for the author
- Is the 24h refund window a business rule or a placeholder?
```

---

## bug-hunter

> 输入：`排查：用户反馈"购物车数量偶尔变 0"`

```
【复现】并发加购：两个 tab 同时加入同一商品 → 数量归零。稳定复现 3/10 次。
【定位】二分到 cart merge：读-改-写没有锁，后写覆盖前写。
【假设】merge() 先 read 后 write 非原子，并发时后者用旧快照覆盖。
【验证实验】给 merge 加一行日志打印读取时的 qty —— 复现时日志显示读到旧值 ✓ 假设成立
【最小修复】用 UPDATE ... SET qty = qty + ? 替代读改写（原子自增），不改其他逻辑。
【回归测试】并发 20 请求加购，断言最终数量 = 20。旧代码下该测试失败，新代码通过 ✓
【同类排查】grep 'read.*then.*write' 发现 wishlist.ts 有相同模式 → 3 处，已列出
```

---

## feature-spec

> 输入：`给导出功能加个"定时导出"`，先出方案

```
## Spec: Scheduled Exports

**目标**：用户可让现有导出任务每日/每周自动执行。
**非目标**：不做自定义 cron 表达式（v1 只有 daily/weekly）。

**接口**：POST /exports/{id}/schedule {freq: daily|weekly, hour: 0-23}
        → 200 {scheduleId, nextRunAt}

**行为表（节选）**
| 输入 | 结果 |
|---|---|
| freq=weekly, hour=25 | 422 ERR_INVALID_HOUR |
| 导出已被删除 | 404，不创建 schedule |
| 同一导出重复创建 | 409，返回已有 schedule |
| 任务执行时用户降级 | 跳过 + 通知，不删除 schedule |

**关键决策**
- 复用现有 export worker（省一个服务）vs 独立 scheduler 服务
  → 选复用：当前量级（<1k 任务）不需要独立部署单元

**测试场景**：边界 hour / 409 幂等 / 失败重试 3 次后告警
```

---

## commit-craft

> 一堆混杂改动 → 自动拆分

```
✓ 检测到 3 类无关改动混在一起，建议拆分：
  1. fix(auth): refresh session cookie before expiry check   ← 核心 bugfix
  2. chore: bump eslint 9.2 → 9.4（顺手，单独提交）
  3. 🔴 src/config.ts 含疑似密钥 AKIA****，已阻止提交，
     建议移入 .env 并将 .env 加入 .gitignore
```

---

**5 个免费样品** = 上面 pr-reviewer / bug-hunter / feature-spec / test-forge / commit-craft 的完整版。
**Pro 包**（15 个 + 中文手册 + 安装脚本）：见 [README](README.md)。
