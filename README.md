# 🧠 Calma – Mental Health Support Platform

Graduation Project – SH.A. Academy

Calma is a mental health support platform available as a **mobile app** (patients)
and a **web dashboard** (therapists & admins). It combines an AI chatbot, licensed
doctors, and multimodal emotion analysis to make psychological support easier to access.

## ✨ Key Features
- Two-role system (Patient / Doctor / Admin) with token-based auth
- AI chatbot in Modern Standard & Egyptian Arabic (PHQ-9 / GAD-7 based)
- Confidential Private Venting mode (no data stored)
- Crisis Safety Net: detects high-risk statements, shows hotlines, alerts the doctor
- Facial emotion detection (DeepFace/OpenCV) and voice tone analysis (Whisper/Librosa)
- Multimodal Mood Index: `0.5 × Text + 0.3 × Vision + 0.2 × Voice`
- AI report handover + electronic prescription
- Prescription OCR + pill reminders + nearby pharmacy finder
- Online & in-person booking with Paymob/Fawry payments and scheduled reminders
- Mood tracker, daily challenges, weekly status summary
- Interactive 3D brain model (Three.js), calming sounds, personalized articles

## 🏗️ Tech Stack
| Layer | Technology |
|---|---|
| Mobile App | Flutter |
| Web Dashboard | React, TailwindCSS |
| Backend | PHP / Laravel, Sanctum (JWT), MySQL |
| AI Services | Python, FastAPI, DeepFace, OpenCV, Whisper, Librosa, EasyOCR |
| LLM | OpenAI GPT-4o or Llama 3, LangChain |
| Scheduling | Laravel Scheduler + Queues |
| 3D | Three.js (.glb) |
| Notifications | Firebase Cloud Messaging |
| Payments | Paymob / Fawry |
| Maps | Google Maps Places API |

## 📁 Repository Structure
| Folder | Description |
|---|---|
| `backend/` | Laravel REST API |
| `mobile-app/` | Flutter patient app |
| `frontend/` | React dashboard for doctors/admins |
| `ai-services/` | Python AI microservices |
| `docs/` | Documentation, ERD, SQL schema |
| `assets/` | 3D models and audio files |
| `data-analysis/` | Mood Index, weekly summaries, and progress charts |
| `testing/` | Test cases, bug reports, and test results |
| `chapters/` | Graduation project documentation chapters |

## 🚀 Getting Started
### Prerequisites
PHP 8.2+, Composer, MySQL, Node.js 18+, Flutter SDK, Python 3.10+

### Backend
```bash
cd backend
composer install
cp .env.example .env
php artisan key:generate
php artisan migrate --seed
php artisan serve
```

### Frontend (React)
```bash
cd frontend
npm install
npm run dev
```

### Mobile App
```bash
cd mobile-app
flutter pub get
flutter run
```

### AI Services
```bash
cd ai-services
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate
pip install -r requirements.txt
uvicorn main:app --reload
```

## 🔐 Environment Variables
Copy `.env.example` to `.env` and fill in: DB credentials, OpenAI key,
Firebase, Paymob/Fawry, Google Maps. **Never commit real keys.**
---

## 📁 Detailed Project Structure

```text
 Calma/
├── docs/                               # Project Documentation & API Specs
├── backend/                            # Laravel REST API
│   ├── app/
│   │   ├── Http/Controllers/
│   │   ├── Models/
│   │   └── Services/
│   ├── routes/
│   │   └── api.php                     # API Endpoints
│   ├── database/
│   └── tests/
├── frontend/                           # React + TailwindCSS Web Dashboard
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── hooks/
│   │   └── context/
│   └── package.json
├── mobile-app/                         # Flutter Patient Application
│   ├── lib/
│   │   ├── screens/
│   │   ├── widgets/
│   │   ├── services/
│   │   └── main.dart
│   └── pubspec.yaml
├── ai-services/                        # Python AI Microservices
│   ├── chatbot/                        # LLM (PHQ-9 / GAD-7)
│   ├── vision/                         # Facial Emotion Detection (DeepFace/OpenCV)
│   ├── voice/                          # Voice Tone Analysis (Whisper/Librosa)
│   ├── ocr/                            # Prescription OCR
│   ├── crisis-classifier/              # Crisis Safety Net
│   └── requirements.txt
├── data-analysis/                      # Mood Index & Weekly Progress Summaries
├── assets/                             # 3D Brain Models (.glb) & Audio Files
├── testing/                            # Test Cases & Bug Reports
├── database/
│   └── schema.sql                      # Database Schema
├── .env.example                        # Environment Variables Template
└── README.md                           # Main Project Documentation

## 👥 Team
1. Ahmed Yasseen Ibrahim Mohamed
2. Alaa Abdelrahman Muhamed Abdelrahman
3. Arwa Nasr Fathy Mohmamed
4. Kareem Shreef Mahmoud
5. Mahmoud Ateya Tolba
6. Mennatullah Ahmed Mohamed Khater
7. Mariem Tamer Ibrahim El naggar
8. Ahmed Salama El sayed Abdel Khalik

## ⚠️ Disclaimer
Calma is an academic project and does not replace professional medical care.
In an emergency, contact your local emergency or mental health hotline.

