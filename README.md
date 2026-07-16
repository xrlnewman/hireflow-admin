# HireFlow Admin

HireFlow 是免费开源的招聘协同运营后台与 Go Gin API，覆盖职位发布、候选人入库、筛选、面试、录用和入职跟进。每次写操作都会记录状态事件与审计日志，重复提交使用 `Idempotency-Key` 安全重试。

## 关联仓库

- [HireFlow Miniapp](https://github.com/xrlnewman/hireflow-miniapp)
- [个人博客项目页](https://field-notes-2fi.pages.dev/projects/hireflow-platform/)

## 技术栈与运行

Go 1.25 + Gin、MySQL 8.4、Redis 8、Vue 3 + Vite。执行 `docker compose -f deploy/docker-compose.yml up --build` 可启动完整 API、MySQL、Redis；未配置依赖时 API 自动回退内存演示数据。

```bash
go vet ./... && go test ./...
cd web && npm install && npm test && npm run build
```

## API 摘要

所有接口返回 `{ code, message, data, traceId }`，写接口支持 `Idempotency-Key`。闭环：候选人录入 → 简历筛选 → 面试安排 → 面试反馈 → 发放 Offer → 入职确认。

| Method | URL | 用途 |
| --- | --- | --- |
| GET | `/api/v1/dashboard` | 入职、筛选、面试与人才池指标 |
| GET | `/api/v1/warehouses` | 招聘渠道与职位池 |
| GET | `/api/v1/products` | 候选人分页与关键词搜索 |
| GET | `/api/v1/stocks/alerts` | 待跟进候选人提醒 |
| POST | `/api/v1/purchase-orders/:id/receive` | 候选筛选完成 |
| POST | `/api/v1/sales-orders/:id/ship` | 面试结果确认 |

默认演示管理员：`13900000000` / `demo123456`，仅用于本地预览。

MIT © xrlnewman
