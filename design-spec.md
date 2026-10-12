# UI design mapping

**Figma source:** [MIH Figma file, node 1:21](https://www.figma.com/design/mlWVGoH2XnbJj7UMM53Gio/Untitled?node-id=1-21). The design file was referenced by the requester but the actual frames, measurements, screens, component library, and assets could **not** be inspected in this session. Nothing in this file claims those details have been verified.

## Planned product surfaces from the Word design (§§2.1, 4.2, 6.3)

| Product surface | Behavior specified in the document | Figma validation |
| --- | --- | --- |
| Web app | Responsive learner experience using React/TypeScript | Pending design review |
| iOS / Android apps | React Native lesson and practice flow | Pending design review |
| Instructor portal | Author/manage/publish lessons | Pending design review |
| Registration and login | Email registration, verification, JWT login | Pending design review |
| Catalog and lesson playback | Filter lessons; stream video and optional audio/PDF assets | Pending design review |
| Practice feedback | Microphone access and pitch/timing/rhythm overlays | Pending design review |
| Progress view | Completion, scores, XP, badges, streaks | Pending design review |
| Subscription/settings | Free/Premium/Pro upgrade through Stripe Checkout | Pending design review |

**Note:** This table enumerates *functional areas from the requirements document*, not Figma frame names or a confirmed screen inventory.

## Source-defined frontend conventions

- Web: React 18 + TypeScript, Tailwind CSS, Redux Toolkit, React Query.
- Mobile: React Native, StyleSheet + shared design tokens, Redux Toolkit, React Query.
- Audio: Web Audio API on web and `react-native-audio-recorder-player` on mobile.
- Video: HTML5 video for web and `react-native-video` for mobile.
- Streaming: native WebSocket on web and `react-native-websocket` on mobile (per source).

## UI verification checklist — proposed

- [ ] Inventory actual Figma frames, named components, and target screens.
- [ ] Match each frame to one or more Version 1.0 requirements.
- [ ] Document page routes, responsive breakpoints, mobile navigation, and screen states.
- [ ] Map tokens for typography, color, spacing, and interactive affordances.
- [ ] Check accessible labels, focus order, error messaging, and keyboard behavior.
- [ ] Review microphone permission prompts, audio capture UX, and fallback states.
- [ ] Record any Figma vs. document scope discrepancies in [open-questions.md](open-questions.md).
