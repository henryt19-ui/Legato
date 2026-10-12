# Environment configuration and feature flags (source: Section 5)

**Keep credentials out of git.** The source explicitly requires secrets to be stored in AWS Secrets Manager and injected into ECS tasks. The root `.env.example` contains **only empty examples** for local planning.

## Named variables in the source

| Variable Name | Description | Example Value | Required |
| --- | --- | --- | --- |
| DATABASE_URL | PostgreSQL connection string including credentials, host, port, and database name | postgresql://user:pass@host:5432/mih_db | Required |
| COGNITO_USER_POOL_ID | AWS Cognito User Pool identifier for token validation | us-east-1_AbCdEfGhI | Required |
| COGNITO_CLIENT_ID | AWS Cognito App Client ID for the MIH application | 1a2b3c4d5e6f7g8h9i0j | Required |
| STRIPE_SECRET_KEY | Stripe API secret key for server-side payment operations | sk_live_xxxxxxxxxxxx | Required |
| STRIPE_WEBHOOK_SECRET | Stripe webhook endpoint signing secret for HMAC verification | whsec_xxxxxxxxxxxx | Required |
| CDN_BASE_URL | CloudFront distribution base URL for signed media asset URLs | https://cdn.mih.example.com | Required |
| REDIS_HOST | Hostname of the ElastiCache Redis primary endpoint | mih-cache.abc123.use1.cache.amazonaws.com | Required |
| REDIS_PORT | Port for Redis connection | 6379 | Required |
| AUDIO_MODEL_PATH | Filesystem path to the TFLite model binary loaded by the Feedback Service sidecar | /models/mih_audio_v2.tflite | Required |
| AWS_REGION | AWS region for SDK clients (S3, SQS, SNS, Kinesis) | us-east-1 | Required |
| LOG_LEVEL | Application logging verbosity: debug, info, warn, or error | info | Optional (default: info) |
| MAX_UPLOAD_SIZE_MB | Maximum permitted size for audio file uploads to the Feedback Service in megabytes | 50 | Optional (default: 50) |
| CORS_ALLOWED_ORIGINS | Comma-separated list of permitted CORS origins for API Gateway | https://app.mih.example.com | Optional |

The examples above are **illustrative values quoted from the design document** and are not working secrets. Never paste actual credentials into documentation, tickets, workflow logs, commits, or pull requests.

## Feature flags (source: §5.2)

| Flag Name | Description | Default Value | Phase |
| --- | --- | --- | --- |
| ENABLE_GAMIFICATION | Activates XP points, achievement badges, and practice streak tracking for all learner accounts | true | 1.0 |
| ENABLE_AUDIO_FEEDBACK | Enables real-time audio analysis and feedback during lesson practice sessions | true | 1.0 |
| ENABLE_SOCIAL_FEATURES | Enables community feed, learner leaderboards, and peer challenge invitations | false | Phase 2 |
| ENABLE_OFFLINE_MODE | Enables local content caching for offline lesson playback on mobile clients | false | Future |
| ENABLE_LIVE_TUTORING | Enables live one-on-one video session booking with verified instructors | false | Phase 2 |

The draft describes AWS AppConfig for feature flags, refreshing the configuration every 60 seconds after startup.

## Operational configuration

ECS task scaling targets, Redis failover topology, read replicas, and CloudFront URL expiry are documented in [architecture.md](architecture.md). See [security-operations.md](security-operations.md) for alerts, error handling, and logging.
