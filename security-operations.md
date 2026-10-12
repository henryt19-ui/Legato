# Security, error handling, and operations (source: Sections 4.6, 6.4–6.5)

## Security architecture

| Security Domain | Implementation | Standard / Framework |
| --- | --- | --- |
| Authentication | OAuth 2.0 / OpenID Connect via Amazon Cognito; JWT bearer tokens with 1-hour expiry; refresh token rotation | RFC 6749, OpenID Connect 1.0 |
| Authorization | Role-Based Access Control (RBAC): Learner, Instructor, Admin. All API Gateway routes enforce role claims from JWT. | NIST SP 800-162 |
| Transport Security | TLS 1.3 enforced on all client-to-gateway and service-to-service communications; HSTS headers on all web responses | RFC 8446 |
| Data at Rest | AES-256 encryption for all RDS volumes, S3 buckets, and ElastiCache nodes via AWS KMS-managed keys | FIPS 140-2 |
| Payment Security | PCI-DSS compliant payment processing delegated entirely to Stripe; MIH never stores raw card data | PCI-DSS v4.0 |
| API Security | Rate limiting at API Gateway; input validation at service layer; parameterized queries only; no raw SQL interpolation | OWASP API Security Top 10 |
| Secrets Management | All secrets (API keys, DB credentials) stored in AWS Secrets Manager; injected at runtime via ECS task definitions | AWS Security Best Practices |

## Error handling conventions

The source specifies standardized JSON errors with `{ "error": { "code", "message", "traceId" } }`; `traceId` should correlate with AWS X-Ray. Standard HTTP errors are listed in [api.md](api.md).

## Resilience patterns in §6.4

- **Circuit breaker:** 5 consecutive inter-service REST failures within a 30-second window; then 503 fast-fail for 60 seconds before recovery attempt, implemented in the source using App Mesh Envoy.
- **Dead-letter queues:** 3 unsuccessful SQS message delivery attempts trigger DLQ routing; alert when DLQ depth exceeds 0.
- **Retries:** exponential backoff with jitter for transient asynchronous failures; maximum 3 retries.

## Observability specification

| Observability Pillar | Technology | Details |
| --- | --- | --- |
| Structured Logging | Winston (Node.js) | All log entries emitted as JSON with fields: timestamp, level, service, traceId, userId (where applicable), message |
| Log Aggregation | Amazon CloudWatch Logs | Log groups per service; retention policy: 30 days (INFO), 90 days (WARN/ERROR); log insights queries for ad-hoc analysis |
| Distributed Tracing | AWS X-Ray | End-to-end request traces across all services; trace sampling at 5%; 100% sampling for error traces |
| Metrics | Amazon CloudWatch Metrics | Custom metrics: lesson session count, audio feedback latency (p50/p95/p99), payment conversion rate, error rate per service |
| Dashboards | Amazon CloudWatch Dashboards | Operational dashboard per service team; executive summary dashboard for stakeholders |
| Alerting | CloudWatch Alarms → SNS → PagerDuty | P1 alerts (service down, error rate > 5%): immediate page; P2 alerts (latency degradation, DLQ depth): 5-minute delay; P3: ticket only |

The source assigns Winston (Node.js) for JSON logging with `timestamp`, `level`, `service`, `traceId`, optional `userId`, and `message`. This does **not** constitute a complete backend language/framework selection.

## Operational readiness not yet provided

- No runnable deployment pipeline, Terraform modules, service Dockerfiles, or database migrations appear in the design document.
- Appendix D is a placeholder for a deployment/rollback runbook.
- Appendix C is a placeholder for ML model training details and benchmark results.
- Appendix A specifies desired architecture diagram but is currently a placeholder.
- Do not claim a functioning production environment, PCI compliance, or proven scaling before audits and tests.
