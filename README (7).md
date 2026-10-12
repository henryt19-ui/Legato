# Musical Instrument Learning Hub (MIH)

**Team 3 · Technical design reference · Version 1.0 (Draft, October 2026)**

> **CONFIDENTIAL — INTERNAL USE ONLY.** This repository starter is derived from a draft marked internal-use-only. Create a **private** repository, limit team access, and obtain project approval before publication. The original Word document is **not** included in this archive.

The **Musical Instrument Learning Hub** is a proposed cloud-native platform for structured, personalized music education. The design targets learners of varying skill levels, providing lessons, real-time practice feedback, tracked learning progress, gamification, instructor-created content, and a tiered subscription model.

This repository is a **documentation/architecture starter**, **not a completed app**. Architectural components, endpoints, and models describe the *proposed* Version 1.0 system rather than implemented features.

## Version 1.0 capabilities

- Learner registration, email verification, authentication, and role-based access.
- Video/audio/sheet-music lessons organized by instrument, difficulty, and genre/tags.
- Real-time feedback on pitch, timing, and rhythm via microphone and TensorFlow Lite.
- Progress dashboard with completion, scores, XP, badges, and practice streaks.
- Instructor lesson authoring/publishing and content management.
- Free / Premium / Pro subscriptions integrated with Stripe.
- Web (React + TypeScript) and iOS / Android (React Native) clients.
- CDN-backed media delivery from S3 via CloudFront.

See [requirements](docs/requirements.md) for in-scope and excluded work.

## Proposed technology stack

| Layer | Source specification |
| --- | --- |
| Web | React 18, TypeScript, Tailwind CSS, Redux Toolkit, React Query; Vercel hosting |
| Mobile | React Native (iOS/Android), React Native StyleSheet + shared tokens |
| Backend | Seven independently deployable microservices, REST + SQS/SNS, WebSocket feedback |
| Identity / gateway | Amazon Cognito (OAuth 2.0 / OIDC), AWS API Gateway, JWT |
| Relational data | Amazon RDS PostgreSQL (domain-isolated services) |
| Media / caches | Amazon S3, CloudFront, ElastiCache Redis |
| Audio inference | Custom TensorFlow Lite model, 250 ms PCM chunks over WebSocket |
| Deployment / analytics | AWS ECS Fargate, Kinesis, Redshift, CloudWatch, X-Ray |
| Payments | Stripe Checkout + signature-verified webhooks |

See [architecture](docs/architecture.md), [API contracts](docs/api.md), and [data models](docs/data-models.md).

## Repository map

```text
.
├── .github/
│   ├── ISSUE_TEMPLATE/               # Feature, bug, architecture-decision intake
│   ├── workflows/repository-checks.yml
│   └── pull_request_template.md
├── apps/
│   ├── web/                          # Web app placeholder (not implemented)
│   └── mobile/                       # React Native placeholder (not implemented)
├── services/
│   ├── auth/ user/ lesson/ feedback/
│   └── progress/ notification/ payment/  # Microservice placeholders
├── packages/shared/                 # Shared types/design-token planning
├── infra/                           # AWS infrastructure planning
├── models/audio/                    # ML model documentation (no binaries)
├── docs/
│   ├── requirements.md
│   ├── architecture.md
│   ├── api.md
│   ├── data-models.md
│   ├── workflows.md
│   ├── configuration.md
│   ├── security-operations.md
│   ├── design-spec.md
│   ├── open-questions.md
│   ├── implementation-plan.md
│   ├── roles.md
│   └── source-links.md
├── .env.example
├── .gitignore
├── CONTRIBUTING.md
├── SETUP.md
└── README.md
```

## Status and next actions

1. Read the [open questions](docs/open-questions.md): several important contracts are unspecified or internally inconsistent in the *draft* document.
2. Review the [Figma design link](https://www.figma.com/design/mlWVGoH2XnbJj7UMM53Gio/Untitled?node-id=1-21); its actual frames and components **have not been verified** against this package.
3. Agree on an incremental implementation plan; [implementation-plan.md](docs/implementation-plan.md) is a **suggestion**, not a team-approved timeline.
4. Scaffold code only after language/runtime, contract, authentication boundary, and infrastructure decisions are confirmed.
5. Use [SETUP.md](SETUP.md) to create and push to a private repository owned by `henryt19-ui`.

## Source and provenance

- Primary source: **Musical Instrument Learning Hub – Design Document**, Version 1.0, October 2026 (Draft), Team 3. Original supplied in this conversation; excluded from the repository package due to confidentiality marking.
- [Original OneDrive document](https://onedrive.live.com/:w:/g/personal/608d16f081a95f66/IQC1C8MHGjVrQpSSLqknaSM3AfcLizDeC8WNjCXw4IUmF4E) — linked by the requester; the **uploaded DOCX** is the actual source used for these notes.
- [Figma prototype](https://www.figma.com/design/mlWVGoH2XnbJj7UMM53Gio/Untitled?node-id=1-21) — link supplied by the requester, not reviewed inside this package.

Markdown extracts preserve the draft's stated architecture and endpoint names. Any proposed follow-up tasks are explicitly labeled *suggested* and do not amend approved requirements.
