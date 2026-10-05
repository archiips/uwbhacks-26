# VitaQuest | UWBHacks '26

Turn everyday habits into a pixel-art adventure. VitaQuest pairs a panda companion with habit islands, XP, streaks, and social encouragement to make healthy routines feel like a game.

<p align="center">
  <img src="react%20expo/assets/login_logo.png" alt="VitaQuest logo" width="280" />
</p>

## Features

- **Habit islands:** explore a world built around walking, sleep, workouts, running, reading, cooking, and meditation.
- **Panda companion:** your avatar reflects your habit scores and progress.
- **Progress tracking:** earn XP, level up, and build streaks through habit logs.
- **Photo verification:** submit activity photos to a FastAPI service using MobileCLIP embeddings, with an Ollama fallback for ambiguous results.
- **Social motivation:** connect with friends and send nudges.
- **HealthKit integration:** native iOS code reads steps and sleep; the Expo app also supports synced health snapshots.

<p align="center">
  <img src="react%20expo/assets/home_island.png" alt="Home island artwork" width="240" />
  <img src="react%20expo/assets/sleep_island.png" alt="Sleep island artwork" width="240" />
</p>

## Repository layout

| Directory | Contents |
| --- | --- |
| [`react expo/`](react%20expo/) | React Native app using Expo Router and Supabase |
| [`UWBHacks Swift/`](UWBHacks%20Swift/) | Native SwiftUI app and HealthKit bridge |
| [`vitaquest-backend/`](vitaquest-backend/) | FastAPI verification service, model utilities, and database SQL |
| [`boilerplate/`](boilerplate/) | HTML and React design prototypes |

## Run locally

### 1. Configure Supabase

Create a Supabase project and apply [`schema.sql`](vitaquest-backend/schema.sql), followed by the `add_*.sql` migrations in `vitaquest-backend/`. Configure Google OAuth in Supabase if you want Google sign-in.

Copy the environment template:

```bash
cd "react expo"
cp .env.example .env
```

Set `EXPO_PUBLIC_SUPABASE_URL` and `EXPO_PUBLIC_SUPABASE_ANON_KEY` to your project's URL and public anon key. Keep database passwords and service-role keys out of the app's public environment variables.

### 2. Start the verification API

From the repository root:

```bash
cd vitaquest-backend
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
pip install requests
bash start_demo.sh
```

`requests` is imported by the backend but is not currently listed in `requirements.txt`, so it needs the separate install above.

Place `mobileclip_image_quantized.onnx` in `vitaquest-backend/models/` before using photo verification. The model is excluded from Git; prototype embeddings for the five supported photo habits are included in [`prototypes/`](vitaquest-backend/prototypes/).

The demo server runs on port **8000**. Check `/healthz` for service status and `/docs` for the API reference. Update `DEMO_SERVER_ENDPOINT` in [`verifyConfig.ts`](react%20expo/src/constants/verifyConfig.ts) to a backend URL reachable from your phone. For a local connection, use the laptop IP printed by the startup script and connect both devices to the same network.

The optional fallback expects Ollama at `http://localhost:11434` with the `gemma4` model available. Set `OLLAMA_URL` to use a different Ollama server.

### 3. Start the Expo app

From the repository root:

```bash
cd "react expo"
npm ci
npm start
```

Open the app using the Expo development server. The repository also provides `npm run ios` and `npm run android` scripts. Native HealthKit access requires a compatible iOS build and is unavailable in Expo Go.

### Native iOS app

Open [`UWBHacks.xcodeproj`](UWBHacks%20Swift/UWBHacks.xcodeproj/) in Xcode. Configure your signing team and HealthKit capability before running on a device.

## Development notes

This repository contains the hackathon implementation and design experiments. Photo verification uses a demo token, and the API allows all CORS origins. Review authentication and deployment settings before hosting the service for broader use.

See [`TODO.md`](TODO.md) for the original implementation checklist; some entries may reflect an earlier demo state.
