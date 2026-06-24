# Architecture & Design System - WVSU-LC Kiosk

This document details the architectural layout, design principles, and visual design system of the West Visayas State University - Lambunao Campus (WVSU-LC) Student Activity Kiosk.

## Technology Stack

- **Frontend Core**: React 19 (SPA) powered by Vite.
- **Styling**: Tailwind CSS v4 & custom stylesheet (`src/index.css`) for fine-grained components.
- **Routing/State**: Local component state machine driving the multi-step kiosk wizard flows.
- **Backend API**: Express (running via `tsx` runner during local development).
- **AI Integrations**: Gemini 3.5 Flash integration via `@google/genai` SDK.
- **Persistence**: Simulated disk storage via structured JSON data files loaded into system memory.

---

## Design System

### Color Palette
The kiosk employs a premium dark-themed glassmorphism aesthetic inspired by high-end dashboard setups.
- **Primary/Accent**: WVSU Gold (`#eab308` / `bg-yellow-500`) representing campus pride.
- **Background**: Deep obsidian colors (`#09090b` to `#18181b`) to ensure readability on bright kiosk displays.
- **Cards/Surfaces**: Semi-transparent, blur-backed panels (`bg-zinc-900/60 backdrop-blur-md`) giving a clean, modern aesthetic.

### Typography
- Inter / Outfit sans-serif typeface hierarchy.
- Bold uppercase section headers mirroring physical airport/kiosk terminals.

### Iconography
- Hand-selected icons from the `lucide-react` library (e.g. `School`, `Calendar`, `Award`, `Shield`, `Coins`).

---

## System Workflows

### 1. Kiosk Walk-in Mode (Student Experience)
1. **Profile Initialization**: Sidebar allows students to set or edit their variables (Name, Student ID, Department) instantly.
2. **Module Navigation**: Greeting page offers 4 directions:
   - **Announcements**: Review details of upcoming student events.
   - **Evaluate Past Events**: Ratings, reviews, and qualitative feedback submission.
   - **Check-In Attendance**: Upload selfie proof to claim points.
   - **Rewards Store**: Exchange accumulated points for official merchandise items.

### 2. Administrative Portal (Department Console)
1. **Authenticated Login**: Access control for OSAS, SSC, and departments.
2. **Live Data Supervision**: Monitor evaluations and student check-in submissions.
3. **Gemini AI Report Generator**: Sends qualitative comments to Gemini and builds a comprehensive evaluation report featuring positive/negative themes, safety alerts, and strategic insights.
