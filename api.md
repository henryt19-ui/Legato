# Proposed API endpoints (source: Section 6.2)

The source specifies the **`/api/v1`** base path. All authenticated routes require a JWT bearer token in the `Authorization` header. These are **design contracts only**; no implementation or deployed service is provided.

| Method | Endpoint | Description | Auth Required |
| --- | --- | --- | --- |
| POST | /auth/register | Register a new user account; sends verification email | No |
| POST | /auth/login | Authenticate with credentials; returns JWT access and refresh tokens | No |
| POST | /auth/refresh | Exchange a valid refresh token for a new access token | No (refresh token) |
| GET | /lessons | List all available lessons; filterable by instrument, difficulty, and tags | Yes |
| GET | /lessons/{lessonId} | Retrieve full lesson detail including signed CloudFront media URLs | Yes |
| POST | /lessons/{lessonId}/start | Initialize a lesson session; opens WebSocket channel for audio feedback | Yes |
| POST | /feedback/analyze | Submit a raw audio chunk for real-time pitch and timing analysis (WebSocket) | Yes |
| GET | /progress/{userId} | Retrieve aggregate progress summary and XP total for a user | Yes |
| GET | /progress/{userId}/{lessonId} | Retrieve per-lesson progress detail including scores and badges | Yes |
| PUT | /progress/{lessonId} | Update progress record for a lesson upon session completion | Yes |
| GET | /users/{userId}/profile | Retrieve user profile details and subscription tier | Yes |
| PUT | /users/{userId}/profile | Update user profile fields (displayName, avatar, preferences) | Yes |
| POST | /subscriptions/checkout | Create a Stripe Checkout Session for plan upgrade; returns redirect URL | Yes |
| POST | /subscriptions/webhook | Stripe webhook receiver; verifies HMAC signature; updates subscription state | No (HMAC verified) |
| GET | /subscriptions/{userId} | Retrieve current subscription status and renewal details for a user | Yes |

## API conventions and draft gaps

- The table above preserves the source endpoint names *without* `/api/v1` to match the source; prepend `/api/v1` to form a complete path, e.g. `GET /api/v1/lessons`.
- The source lists `POST /feedback/analyze` but calls it a **WebSocket** endpoint. A transport-level agreement is still needed for the actual WebSocket URL, session setup, auth strategy, frame payload, and return schema.
- The audio pipeline description specifies 250 ms PCM audio frames and structured JSON feedback; it does **not** define sample rate, channel count, frame schema, protocol version, or examples.
- Instructor content management and administrative functions are in-scope but **instructor/admin API routes are not enumerated** in §6.2.
- Stripe webhook is unauthenticated by JWT but must verify its provider HMAC signature.
- Authentication/authorization must protect ownership when `userId` or `lessonId` is supplied in the path; details are not specified.

## Error response standard (source: Section 6.4)

| HTTP Status Code | Error Code | Meaning | Common Cause |
| --- | --- | --- | --- |
| 400 | BAD_REQUEST | Malformed request syntax or invalid parameters | Missing required fields, invalid format |
| 401 | UNAUTHORIZED | Authentication required or token invalid/expired | Missing or expired JWT bearer token |
| 403 | FORBIDDEN | Authenticated but insufficient permissions | Learner accessing Instructor-only endpoint |
| 404 | NOT_FOUND | Requested resource does not exist | Invalid lessonId or userId in path |
| 422 | UNPROCESSABLE_ENTITY | Request is syntactically valid but semantically incorrect | Business rule violation (e.g., duplicate subscription) |
| 429 | RATE_LIMITED | Too many requests from this client within the time window | API Gateway rate limit exceeded |
| 500 | INTERNAL_ERROR | Unexpected server-side error | Unhandled exception; logged with trace ID |

Example from the source (illustrative only):

```json
{
  "error": {
    "code": "NOT_FOUND",
    "message": "Lesson with ID '3fa85f64' was not found.",
    "traceId": "f47ac10b-58cc-4372-a567-0e02b2c3d479"
  }
}
```

All errors should have a UUID `traceId` for correlation in AWS X-Ray. The source also discusses a 503 response from circuit breakers but does not include it in the standard status table.

## Before implementing any endpoint

Approve request/response JSON schemas, validation, pagination/filter contracts, error details, access-control matrix, rate limits, and integration-test cases. The current design document does not fully specify these items.
