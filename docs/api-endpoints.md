# Calma – API Endpoints

Base URL: `/api/v1` (Laravel). All endpoints except register/login need
`Authorization: Bearer <token>`.

Roles: 🧑 patient · 🩺 doctor · 🛡️ admin

## 1. Auth & Users

| Method | Endpoint | Role | Description |
|---|---|---|---|
| POST | `/auth/register` | public | Register a patient (doctors are created/approved by admin) |
| POST | `/auth/login` | public | Login, returns token + role |
| POST | `/auth/logout` | all | Revoke token |
| GET | `/me` | all | Current user profile |
| PUT | `/me` | all | Update profile |
| POST | `/device-tokens` | all | Save FCM token |
| GET | `/admin/users` | 🛡️ | List/filter users |
| PATCH | `/admin/doctors/{id}/approve` | 🛡️ | Approve a doctor account |

## 2. AI Chat

| Method | Endpoint | Role | Description |
|---|---|---|---|
| POST | `/chat/sessions` | 🧑 | Start a chat session |
| POST | `/chat/sessions/{id}/messages` | 🧑 | Send a message, get AI reply (crisis check runs first) |
| GET | `/chat/sessions/{id}/messages` | 🧑 | Session history |
| POST | `/chat/sessions/{id}/end` | 🧑 | End session and generate the report |
| POST | `/venting/message` | 🧑 | Private Venting: stateless, **nothing is stored** |

Example response (`POST /chat/sessions/{id}/messages`):
```json
{
  "reply": "أنا هنا معاك، تحب تحكيلي أكتر؟",
  "crisis": false,
  "phq9_progress": 3
}
```
When `crisis` is `true`, the app stops the chat and shows the emergency window.

## 3. Reports & Prescriptions

| Method | Endpoint | Role | Description |
|---|---|---|---|
| GET | `/reports` | 🩺 | Reports of the doctor's patients |
| GET | `/reports/{id}` | 🩺 🧑 | Report details (patient: own only) |
| POST | `/prescriptions` | 🩺 | Create e-prescription for a patient |
| GET | `/prescriptions` | 🧑 🩺 | List prescriptions |
| GET | `/prescriptions/{id}/pdf` | 🧑 🩺 | Printable PDF |
| POST | `/prescriptions/ocr` | 🧑 | Upload image → extracted medicines + schedule |
| POST | `/prescriptions/{id}/reminders` | 🧑 | Save confirmed reminder schedule |

## 4. Booking & Payments

| Method | Endpoint | Role | Description |
|---|---|---|---|
| GET | `/doctors` | 🧑 | List doctors |
| GET | `/doctors/{id}/slots?date=` | 🧑 | Available slots |
| POST | `/doctors/me/slots` | 🩺 | Create availability slots |
| POST | `/appointments` | 🧑 | Book a slot (`type`: online / in_person) |
| GET | `/appointments` | 🧑 🩺 | My appointments |
| PATCH | `/appointments/{id}/cancel` | 🧑 🩺 | Cancel |
| POST | `/payments/checkout` | 🧑 | Create Paymob/Fawry payment for an appointment |
| POST | `/payments/webhook` | gateway | Payment confirmation callback |

## 5. Mood, Challenges & Progress

| Method | Endpoint | Role | Description |
|---|---|---|---|
| POST | `/moods` | 🧑 | Log mood (1–5 + optional note) |
| GET | `/moods?from=&to=` | 🧑 🩺 | Mood history |
| GET | `/mood-index?patient_id=` | 🩺 🧑 | Multimodal Mood Index timeline |
| GET | `/tasks/today` | 🧑 | Today's daily challenges |
| POST | `/tasks/{id}/complete` | 🧑 | Mark task completed |
| GET | `/progress` | 🧑 🩺 | Completion rate and progress data |
| GET | `/summaries/weekly` | 🧑 🩺 | Latest weekly summary |

## 6. Content

| Method | Endpoint | Role | Description |
|---|---|---|---|
| GET | `/audio` | 🧑 | Calming audio tracks (by category) |
| GET | `/articles/recommended` | 🧑 | Top 3 articles/books based on the report tags |
| GET | `/brain-regions/{name}` | all | Info shown when a 3D region (e.g. amygdala) is tapped |

## 7. Doctor Dashboard & Alerts

| Method | Endpoint | Role | Description |
|---|---|---|---|
| GET | `/doctor/patients` | 🩺 | My patients |
| GET | `/doctor/patients/{id}/overview` | 🩺 | Pre-session summary, moods, tasks |
| GET | `/doctor/alerts` | 🩺 | Crisis alerts |
| PATCH | `/doctor/alerts/{id}/ack` | 🩺 | Acknowledge an alert |

## 8. AI Services (internal, FastAPI)

Called by the Laravel backend or the mobile app. Protect them with an internal API key.

| Method | Endpoint | Input | Output |
|---|---|---|---|
| POST | `/ai/chat` | messages, system context | reply, extracted symptoms |
| POST | `/ai/summary` | chat transcript | JSON report |
| POST | `/ai/crisis/check` | text | `{risk: low|medium|high, matched: [...]}` |
| POST | `/ai/vision/emotion` | image | `{emotion: "sadness", score: 0.85}` |
| POST | `/ai/voice/analyze` | audio | transcript, tone, speed, stress score |
| POST | `/ai/ocr/prescription` | image | `[{medicine, dose, times}]` |
| POST | `/ai/recommend` | report tags | best 3 article ids |

## 9. Common Conventions

- JSON responses; errors: `{ "message": "...", "errors": { ... } }`.
- Status codes: `200/201` success, `401` unauthenticated, `403` wrong role, `404`, `422` validation, `429` rate limit.
- Dates in ISO 8601 (UTC); the clients convert to local time.
- Pagination: `?page=1&per_page=20`.
