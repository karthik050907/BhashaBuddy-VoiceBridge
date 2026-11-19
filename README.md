# BhashaBuddy — VoiceBridge

**Live App:** https://ais-pre-aillrdoxsbkvdytfdkugku-672465744023.asia-northeast1.run.app  
**Repository:** https://github.com/karthik050907/BhashaBuddy-VoiceBridge

**BhashaBuddy — VoiceBridge** is an accessible, bilingual technical English learning web application built for students transitioning from rural and vernacular-medium schools into English-medium engineering curricula and campus placement interviews.

Using **Telugu (`తెలుగు`)**, **Hindi (`हिन्दी`)**, and **Tamil (`தமிழ்`)** as active mediums of instruction alongside English, BhashaBuddy teaches **50 core engineering and placement terms** through interactive flashcards, visual pictorial diagrams, Web Speech synthesis & recognition, fuzzy Levenshtein pronunciation scoring, bilingual quizzes, and gamified progress tracking.

---

## Key Features

### 1. Bilingual Interface & Regional Medium Support
- Instant React Context UI localization (`LocaleContext`) across **English**, **Telugu (`తెలుగు`)**, **Hindi (`हिन्दी`)**, and **Tamil (`தமிழ்`)**.
- Every technical term displays its native script translation, phonetic transliteration, concept explanation in the learner's chosen regional language, and a translated contextual engineering sentence.

### 2. 50-Term Progressive Engineering Vocabulary Deck
Organized into **5 progressive units** (10 terms each):
1. **Algorithms & Computing** (`Algorithm`, `Iteration`, `Recursion`, `Latency`, `Abstraction`, `Encapsulation`, `Polymorphism`, `Concurrency`, `Cache`, `Throughput`)
2. **Circuits, Signals & Hardware** (`Impedance`, `Voltage`, `Capacitance`, `Semiconductor`, `Amplifier`, `Oscillation`, `Bandwidth`, `Modulation`, `Transducer`, `Rectifier`)
3. **Thermodynamics & Mechanics** (`Entropy`, `Equilibrium`, `Torque`, `Viscosity`, `Momentum`, `Conductivity`, `Stress`, `Strain`, `Turbulence`, `Efficiency`)
4. **Calculus, Data & Optimization** (`Derivative`, `Integral`, `Gradient`, `Matrix`, `Convergence`, `Variance`, `Regression`, `Optimization`, `Constraint`, `Heuristic`)
5. **Placement & Technical Interviews** (`Scalability`, `Architecture`, `Trade-off`, `Bottleneck`, `Benchmark`, `Deployment`, `Debugging`, `Validation`, `Feasibility`, `Documentation`)

### 3. Speech Recognition Evaluator & Levenshtein Fuzzy Matching
- Uses Browser **Web Speech API (`SpeechRecognition`)** + **Web Audio `AnalyserNode`** for real-time 16-band acoustic waveform visualization.
- Evaluates spoken attempts with **Levenshtein edit distance + phonetic normalization** (`src/utils/levenshtein.ts` and `POST /api/evaluate-pronunciation`), accepting regional accent variations (`distance <= 2`) and displaying `Pronunciation Match: XX%`.
- Includes an accessible **text-input fallback** for browsers where microphone permissions or `SpeechRecognition` are unavailable.

### 4. Text-to-Speech Audio Model
- Uses native browser **Web Speech Synthesis (`speechSynthesis`)** at adjustable speeds (`0.75x`, `1.0x`, `1.2x`) for zero-bandwidth audio playback of English terms, individual syllables, regional meanings, and example sentences.

### 5. Daily Bilingual Quiz & Gamified Progress Tracking
- **Daily Interactive Quiz**: Supports bilingual multiple-choice, sentence completion, and mixed 5-question drills.
- **Gamified Tracker (`localStorage`)**: Tracks configurable daily word targets (`5`, `10`, or `15` words/day), daily streaks, XP & level progression, 7-day consistency bar chart, 5 engineering unit mastery rings, achievement milestones, and a Levenshtein `Pronunciation Scores Log`.

### 6. Stretch Features Included
- **BhashaBuddy Tutor (`Bhasha Tutor`)**: Vernacular-to-English conversational mentor powered by `@google/genai` (`gemini-4-hydrogen`) via `POST /api/tutor`, with automatic offline 50-term glossary fallback.
- **Visual Pictorial Dictionary**: Inline vector engineering diagrams (`flowchart`, `circuit`, `wave`, `gear`, `graph`, `architecture`) on every flashcard.
- **Offline Cache & Standalone HTML**: Service Worker (`public/sw.js`) + Cache API pre-warming and a single-file standalone bundle (`public/BhashaBuddy-VoiceBridge.html`).

### 7. Accessibility (WCAG AA / AAA Focused)
- Skip-to-main-content link, semantic landmarks (`header`, `nav`, `main`, `section`, `aside`, `footer`), and `lang` attributes on multilingual scripts.
- `aria-live="polite"` announcements for XP gains, badge unlocks, and waveform states.
- Scalable root typography (`100%`, `115%`, `130%`), Light/Dark & High-Contrast modes, `>= 44px` touch targets, high-visibility `:focus-visible` outlines, `prefers-reduced-motion` / Calm Motion toggle, and keyboard shortcuts (`Alt+M`, `Alt+P`, `Alt+Left/Right`, `Escape`).

---

## Tech Stack

- **Frontend**: React 19, TypeScript, Tailwind CSS v4, `motion/react`, `lucide-react`
- **Backend**: Node.js (`v22+`), Express (`server.ts`), `@google/genai` (`gemini-4-hydrogen`)
- **Browser APIs**: Web Speech API (`SpeechRecognition`, `SpeechSynthesis`), Web Audio API (`AnalyserNode`), LocalStorage, Service Worker & Cache API

---

## Getting Started

### Prerequisites
- **Node.js** `>= 20` (tested on `v22.23.2`)

### Local Setup

1. **Install dependencies**
   ```bash
   npm install
   ```

2. **Configure environment variables (optional for AI Tutor & tips)**
   Copy `.env.example` to `.env` and add your Gemini API key:
   ```env
   GEMINI_API_KEY="your_gemini_api_key"
   ```
   *(All 50 flashcards, synthetic speech, Levenshtein scoring, quizzes, and offline progress work even without an API key.)*

3. **Run in development mode**
   ```bash
   npm run dev
   ```
   Open [http://localhost:3000](http://localhost:3000).

4. **Build and run in production**
   ```bash
   npm run build
   npm start
   ```

5. **Run TypeScript validation**
   ```bash
   npm run lint
   ```

---

## Standalone Single-File HTML

A self-contained single-file version (`public/BhashaBuddy-VoiceBridge.html`) is available directly from the running app's **Download Standalone HTML** link (or at `/BhashaBuddy-VoiceBridge.html`) and can be opened directly in any modern browser (`file://`) without running a server.

---

## Project Structure

```text
├── server.ts                          # Express + Vite server & Gemini/Levenshtein API routes
├── index.html                         # HTML entry point & typography setup
├── metadata.json                      # App metadata & microphone permission declaration
├── public/
│   ├── sw.js                          # Offline Service Worker & Cache API handler
│   └── BhashaBuddy-VoiceBridge.html   # Self-contained standalone HTML bundle
└── src/
    ├── App.tsx                        # Root layout, navigation, accessibility & XP toast shell
    ├── index.css                      # Tailwind v4 config, font scaling, focus & reduced-motion rules
    ├── types.ts                       # TypeScript interfaces
    ├── context/LocaleContext.tsx      # Bilingual UI locale provider (en / te / hi / ta)
    ├── data/
    │   ├── technicalVocab.ts          # 50 engineering terms across 5 units + badges & levels
    │   └── uiStrings.ts               # Localized UI strings (English, Telugu, Hindi, Tamil)
    ├── hooks/
    │   ├── useAccessibility.ts        # Theme, contrast, font scale, reduced motion, speech rate
    │   ├── useGamification.ts         # XP, daily goal, streaks, quests, pronunciation logs
    │   └── useSpeechBridge.ts         # Web Speech recognition/synthesis & AudioContext meter
    ├── utils/levenshtein.ts           # Levenshtein distance & phonetic fuzzy matcher
    └── components/
        ├── Header.tsx
        ├── AccessibilityToolbar.tsx
        ├── WaveformVisualizer.tsx
        ├── ConceptDiagram.tsx
        ├── FlashcardStudio.tsx
        ├── DailyQuizView.tsx
        ├── BhashaTutorView.tsx
        ├── ProgressDashboard.tsx
        └── KeyboardGuideModal.tsx
```
