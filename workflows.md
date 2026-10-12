# Key workflows (source: Section 6.3)

These steps document **planned behavior** from the source, not a verified implementation.

## Registration and verification (§6.3.1)

1. Learner submits email, password, and display name from web/mobile.
2. Client calls `POST /api/v1/auth/register` via API Gateway.
3. Auth Service checks email, password policy, and uniqueness.
4. Auth Service creates a Cognito User Pool account.
5. Cognito emails a one-time verification code/link.
6. User Service creates a PostgreSQL user with role `Learner` and tier `Free`.
7. Learner verifies email with Cognito; account becomes active.
8. Learner can sign in.

## Lesson playback and practice (§6.3.2)

1. Authenticated learner selects a lesson from the catalog.
2. Client requests `GET /api/v1/lessons/{lessonId}`.
3. Lesson Service returns metadata and CloudFront signed URLs for media.
4. Client streams lesson video directly from CloudFront.
5. Client calls `POST /api/v1/lessons/{lessonId}/start` and establishes authenticated WebSocket feedback session.
6. Microphone captures audio in 250 ms PCM frames transmitted over the stream.
7. TensorFlow Lite sidecar calculates pitch, timing, rhythm results; client renders visual overlays.
8. On completion, client submits `PUT /api/v1/progress/{lessonId}` with final scores.
9. Progress Service stores data and emits `lesson.completed` via SNS.
10. Notifications and gamification mechanisms award XP/badges and inform the learner.

## Subscription upgrade (§6.3.3)

1. Learner selects Premium or Pro in account settings/upgrade prompt.
2. Client calls `POST /api/v1/subscriptions/checkout` with plan identifier.
3. Payment Service creates Stripe Checkout session and returns redirect URL.
4. User enters payment details on Stripe-hosted page; MIH should never collect raw card numbers.
5. Stripe emits `checkout.session.completed` to the webhook endpoint.
6. Payment Service verifies HMAC signature with `STRIPE_WEBHOOK_SECRET`.
7. Subscription record and `User.subscriptionTier` are updated.
8. `subscription.upgraded` is emitted on SNS; Notification Service sends email.
9. New tier appears in a subsequent authenticated JWT session, unlocking allowed content.

## Missing workflow contracts

Exactly-once/idempotent event handling, retries across services, media authorization rules, audio latency budgets, and account verification endpoint semantics need explicit specification. See [open questions](open-questions.md).
