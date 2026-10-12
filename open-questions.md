# Open design questions and decisions

**This file identifies ambiguities in the October 2026 draft.** The questions are not proposed corrections to the source until Team 3 reaches a decision.

| Ref | Clarification needed | Why it matters | Source |
| --- | --- | --- | --- |
| OQ-01 | Which backend language and framework will each of the seven services use? | Repo build/test/run instructions depend on runtime | §4.3, §6.5 |
| OQ-02 | What is the definitive on-device vs server-side audio inference split? | §2.1 describes on-device and server-side; §4.4 details Fargate sidecar | §2.1, §4.4 |
| OQ-03 | Is `/feedback/analyze` an HTTP POST, a WebSocket message type, or both? Specify stream URL and JSON schema. | API list and workflow describe different transports | §6.2, §6.3.2 |
| OQ-04 | How will database ownership and cross-service relationships be implemented? | Independent schemas vs FKs across User/Lesson/Progress/Subscription | §3, §6.1 |
| OQ-05 | Is `User.passwordHash` intended to be persisted locally while using Cognito? | Identity security, schema obligations, source-of-truth | §4.6, §6.1 |
| OQ-06 | What are the Instructor and Admin endpoints and authorization matrix? | Functions are in-scope but APIs are not enumerated | §2.1, §6.2 |
| OQ-07 | What exact entitlement/price rules distinguish Free, Premium and Pro? | Checkout, authorization, content restrictions | §1, §6.3.3 |
| OQ-08 | What is the strategy for Stripe webhook replay/idempotency and database consistency? | Prevent double updates, handle delayed events | §6.3.3 |
| OQ-09 | How are request path user IDs authorized and progress changes protected from spoofing? | Prevent unauthorized access/updates | §4.6, §6.2 |
| OQ-10 | What are the model training data, validation performance, frame format, and audio latency SLOs? | ML QA and measurable feedback quality | §4.4, Appendix C |
| OQ-11 | Are AWS App Mesh assumptions and dependencies still deployment-appropriate? | Architecture must be implementable on the chosen AWS account | §3, §4.1, §6.4 |
| OQ-12 | Who approves the missing architecture diagram and deployment rollback steps? | Appendices A and D remain placeholders | Appendices A, D |
| OQ-13 | What are the actual Figma frames/components, and do they cover all roles? | UI implementation cannot proceed from links alone | External Figma reference |
| OQ-14 | What are the agreed priorities, estimates, milestones, and MVP slice? | Draft defines V1.0 scope but not delivery milestones | §2, §7 |
| OQ-15 | What license, distribution, and repository visibility approvals apply? | Design is explicitly confidential, draft, internal use only | Cover and footer |

**Decision process (suggested):** create an architecture-decision issue for any high-impact question; assign a responsible reviewer; capture options, conclusion, date, implications, and changes to associated docs.
