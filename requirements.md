# Requirements and scope (source: Sections 1–2)

**Source status:** October 2026, V1.0 Draft. The following scope is transcribed and reorganized from the provided design document; no functionality is claimed to be implemented.

## Purpose

MIH is a cloud-native digital music education platform for learners at different skill levels, intended to bridge self-directed study and formal instruction through curated lessons and immediate practice feedback.

## Version 1.0 — explicitly in scope

| ID | Capability | Notes / source section |
| --- | --- | --- |
| MIH-001 | User registration, email verification, and authentication | §2.1, §6.3.1 |
| MIH-002 | Instrument video/audio lessons | §2.1, §6.3.2 |
| MIH-003 | Real-time on-device **and** server-side ML audio feedback | §2.1 (see open questions concerning §4.4) |
| MIH-004 | Per-lesson and aggregate progress tracking | §2.1, §6.1 |
| MIH-005 | XP, badges, and practice streaks | §2.1, §5.2 |
| MIH-006 | Instructor authoring and publication portal | §2.1 |
| MIH-007 | Stripe payments and subscription plans: Free, Premium, Pro | §2.1, §6.3.3 |
| MIH-008 | Responsive React web app and iOS/Android React Native apps | §2.1, §4.2 |
| MIH-009 | Media assets (video, audio, PDF sheet music) via S3 and CDN | §2.1, §4.5 |
| MIH-010 | Role-based authorization (Learner, Instructor, Admin) | §2.1, §4.6 |

**Supported instruments mentioned in the source:** guitar, piano, violin, drums, bass, ukulele, and more. Lessons are organized by instrument, difficulty (Beginner / Intermediate / Advanced), and genre or tags.

## Explicitly out of scope for Version 1.0

| Feature | Source status |
| --- | --- |
| Specialized hardware integrations beyond basic plug-and-play MIDI controllers | Deferred |
| Third-party instrument rental services | Deferred |
| Live one-on-one tutoring | Phase 2 |
| Offline-only content packs | Future phase |
| Community social feed, peer challenges, leaderboards | Phase 2; flag defaults off |

## Functional user journeys in the source

- **Learner registration**: client registration → service validation → Cognito account → verification message → PostgreSQL user record → verified account.
- **Lesson practice**: catalog → signed media URLs → lesson start → microphone audio chunks → feedback scores → update progress → XP / badge event.
- **Subscription upgrade**: select Premium or Pro → Stripe hosted Checkout → signed webhook → database update → user notification and access updates.

See [workflows.md](workflows.md) for step-by-step sequences and [api.md](api.md) for exact published endpoint list.

## Cross-cutting requirements recorded in the draft

- JWT-based authentication and RBAC roles.
- TLS 1.3 for network transport; AES-256 for specified storage components via AWS KMS.
- Stripe handles raw card details; MIH should not store raw card information.
- Video/audio delivery via S3 + CloudFront signed URLs (specified lifetime: 3,600 seconds).
- Real-time audio PCM in 250 ms frames over an authenticated WebSocket, with pitch, timing, and rhythm outputs.
- All primary identifiers UUIDv4; timestamps stored in UTC.
- Workload scaling described in §5.3, including ECS per-service target 70% CPU.
- QA role identifies a *target* of 10,000 concurrent users (§7), but this is not a validated measured capacity.

## Not specified in the source

The document does **not** enumerate per-feature UI acceptance criteria, subscription price points or entitlements, detailed curriculum content, audio-model test baselines, detailed instructor/admin REST endpoints, or a delivery schedule. Record and approve these separately; do not assume they are fixed requirements.
