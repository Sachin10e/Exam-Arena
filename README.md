# 🎓 ExamArena

[![Live Demo](https://img.shields.io/badge/Live_Demo-examarena--in.vercel.app-6366f1?style=for-the-badge&logo=vercel)](https://examarena-in.vercel.app)
[![Next.js](https://img.shields.io/badge/Next.js_16-000000?style=for-the-badge&logo=nextdotjs&logoColor=white)](https://nextjs.org/)
[![React](https://img.shields.io/badge/React_19-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)](https://reactjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript_5-3178C6?style=for-the-badge&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Supabase](https://img.shields.io/badge/Supabase_pgvector-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white)](https://supabase.com/)
[![Google Gemini](https://img.shields.io/badge/Gemini_2.5_Flash-8E75B2?style=for-the-badge&logo=googlegemini&logoColor=white)](https://ai.google.dev/)

**ExamArena** is a production-grade, AI-native exam preparation platform that transforms raw syllabus PDFs, lecture notes, and past-year questions (PYQs) into structured, unit-by-unit study paths. Powered by a hybrid Retrieval-Augmented Generation (RAG) pipeline, Gemini 2.5 Flash, an interactive Knowledge Graph, and an SM-2 spaced repetition engine, ExamArena equips students with an intelligent, distraction-free environment to master complex academic topics.

🌐 **Live Production App:** [https://examarena-in.vercel.app](https://examarena-in.vercel.app)

---

## 📸 Key Capabilities & Architecture

```
                                    +------------------------------+
                                    |   Document Ingestion         |
                                    | (PDF, DOCX, TXT, Vision OCR) |
                                    +--------------+---------------+
                                                   |
                                                   v
                                    +------------------------------+
                                    |  Sliding Window Tokenizer    |
                                    |   (500 tokens / 100 overlap) |
                                    +--------------+---------------+
                                                   |
                                                   v
                                    +------------------------------+
                                    |  Gemini Embedding Vectorizer |
                                    | (gemini-embedding-001 768d)  |
                                    +--------------+---------------+
                                                   |
                                                   v
                                    +------------------------------+
                                    |  Supabase pgvector & Hybrid  |
                                    | (RRF Vector + Sparse FTS)    |
                                    +--------------+---------------+
                                                   |
                                                   v
                                    +------------------------------+
                                    | Multi-Pass AI Answer Engine  |
                                    | (Gemini 2.5 Flash Stream)    |
                                    +------------------------------+
```

---

## ✨ System Features

### 🧠 1. Automated Study Plan Generator
- **Unit-Isolating Pedagogy**: Generates exhaustive study units containing **Long Essay Questions** (5–10 marks), **Short Answers** (1–2 marks), and **MCQs with inline key validation**.
- **Contextual Resource Enrichment**: Injects custom Pro-Tips along with dynamic, URL-encoded **Google Web Search** and **YouTube Tutorial** links for every long-form question.
- **Streaming Response Architecture**: Real-time server-to-client streaming via Next.js Edge responses for sub-second first-byte latency.

### 💬 2. Multi-Pass AI Chat & Q&A Engine
- **3-Pass Reasoning Pipeline**: 
  1. *Factual Domain Assembly*: Extracts core syllabus concepts.
  2. *Exam Structure Normalization*: Formats responses into university scoring schemes.
  3. *Visual & Mathematical Synthesis*: Embeds LaTeX math (`$$`), comparison markdown tables, and dynamic **Mermaid.js** flowcharts.

### 🕸️ 3. Interactive Knowledge Graph & Topic Topology
- **Graph Visualization**: Renders interactive 2D/3D topic nodes and relational edges (`prerequisite`, `related`, `extension`, `example_of`).
- **Semantic Topic Explainer**: Node selection triggers RAG context extraction to generate instant markdown topic overviews.

### 🧠 4. Spaced Repetition Flashcards (SuperMemo SM-2)
- **Active Recall Engine**: Auto-extracts high-yield flashcard decks directly from syllabus materials.
- **Algorithmic Review Scheduling**: Employs the **SuperMemo SM-2 algorithm** (`ease_factor`, `interval`, `next_review_at`) to optimize memory retention over time.

### 📝 5. Automated Mock Exam Generator
- **Instant Quiz Construction**: Synthesizes 5–7 question multiple-choice tests with answer explanations and automated scoring telemetry.

### 🎨 6. Premium UI Engine & Default Light Mode
- **Zero-Flicker Light Theme**: Synchronous head injection script prevents SSR theme flicker, fully compliant with React 19 hydration policies (`suppressHydrationWarning`).
- **Focus Mode**: One-click distraction-free workspace hiding surrounding navigation shells.
- **Handwritten / Print Engine**: Custom CSS `@media print` rules formatted to export study plans as clean handwritten or print PDF documents.

---

## 🛠️ Technology Stack

| Domain | Technology | Description |
| :--- | :--- | :--- |
| **Frontend** | Next.js 16 (App Router), React 19 | SSR, Server Actions, Dynamic Streaming |
| **Language** | TypeScript 5 | Strict static typing across API & UI |
| **Styling** | Tailwind CSS v4, PostCSS | Custom utility design system & Light/Dark themes |
| **AI / LLM** | Google Gemini API (`@google/genai`) | `gemini-2.5-flash` for streaming & OCR |
| **Embeddings & Vector** | `gemini-embedding-001`, Supabase `pgvector` | 768-dimensional dense vector embeddings |
| **Database & Auth** | Supabase (Postgres), Row Level Security | Multi-tenant user isolation & JWT auth |
| **Visualizations** | Recharts, Mermaid.js, Framer Motion | Analytics charts, dynamic flowcharts, micro-animations |
| **PWA & Offline** | `next-pwa`, Web Workers | Service Worker caching & PWA support |
| **Build & Dev** | Turbopack (`next dev --turbo`), Webpack | Instant HMR development & optimized production bundles |
| **Deployment** | Vercel | Global CDN Edge Deployment |

---

## 📁 Repository Architecture

```text
exam-ai-platform/
├── app/                        # Next.js App Router
│   ├── actions/                # Server Actions (upload, flashcard reviews, subjects)
│   ├── api/                    # Serverless API Routes
│   │   ├── chat/               # Multi-pass streaming Q&A API
│   │   ├── extract-flashcards/ # AI Flashcard extraction
│   │   ├── generate-exam/      # AI Mock Exam generator
│   │   ├── generate-plan/      # Streaming Study Plan generator
│   │   ├── knowledge-graph/    # Graph topology API
│   │   └── subjects/           # Subject container management
│   ├── arena/                  # Main Study Arena Workspace page
│   ├── chat/                   # AI Tutor Chat view
│   ├── components/             # Reusable UI Components
│   │   ├── ai/                 # Mermaid diagrams, markdown renderers
│   │   ├── layout/             # TopNav, Sidebar, ThemeToggle, Shortcuts
│   │   └── study/              # Flashcard decks, Mock Exam modals
│   ├── history/                # Saved study session browser
│   ├── settings/               # Account & Theme settings
│   ├── share/[id]/             # Public study plan viewer
│   ├── globals.css             # Design tokens, Light mode perfect inversion, Print CSS
│   └── layout.tsx              # Root Layout, SSR theme injection, Providers
├── lib/                        # Core Engine Utilities
│   ├── ai/                     # Answer engine, question classifier, hybrid search
│   ├── analytics/              # Topic relationships, progress metrics, spaced repetition
│   ├── embeddings.ts           # Gemini embedding vectorizer
│   ├── spacedRepetition.ts     # SM-2 algorithm mathematical engine
│   └── supabaseAdmin.ts        # Supabase Service Role client
├── public/                     # Static assets, icons, PWA manifest, service workers
├── supabase_*.sql              # Database migrations (RLS, Graph, Multi-Tenant, Analytics)
├── next.config.ts              # Next.js configuration & package imports optimization
└── package.json                # Dependencies & scripts
```

---

## 🚀 Getting Started

### Prerequisites

- **Node.js**: `v20.x` or higher
- **Package Manager**: `npm`, `pnpm`, or `bun`
- **Database**: Active [Supabase](https://supabase.com) Project (with `pgvector` extension enabled)
- **AI Keys**: Google [Gemini API Key](https://aistudio.google.com/)

### 1. Clone & Install

```bash
git clone https://github.com/Sachin10e/exam-ai-platform.git
cd exam-ai-platform
npm install
```

### 2. Configure Environment Variables

Create a `.env.local` file in the root folder:

```env
# Supabase Configuration
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-supabase-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-supabase-service-role-key

# Google Gemini API
GEMINI_API_KEY=your_google_gemini_api_key
```

### 3. Setup Database Schema

Run the migration scripts provided in the root directory inside your Supabase SQL Editor:

1. `supabase_multi_tenant.sql` (Creates core tables, indices, and RLS policies)
2. `supabase_graph.sql` (Creates `topics` and `topic_edges` tables for Knowledge Graph)
3. `supabase_analytics.sql` (Creates analytics tracking tables)
4. `supabase_session_resume.sql` (User preference syncing)

### 4. Run Development Server (Turbopack)

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

---

## 📜 Available Scripts

| Script | Command | Purpose |
| :--- | :--- | :--- |
| **Development** | `npm run dev` | Launches Next.js dev server with **Turbopack** |
| **Production Build** | `npm run build` | Compiles optimized production bundle with Webpack PWA bundling |
| **Start Server** | `npm run start` | Starts the production server |
| **Linting** | `npm run lint` | Runs ESLint checks across code files |

---

## 🤝 Contributing

Contributions are welcome! If you'd like to improve features, add new learning tools, or enhance UI aesthetics:

1. Fork the Repository
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 👤 Author

**Sachin Ellakar**
- **GitHub**: [@Sachin10e](https://github.com/Sachin10e)
- **LinkedIn**: [Sachin Ellakar](https://linkedin.com/in/sachin-ellakar-565252288)
- **Live Demo**: [https://examarena-in.vercel.app](https://examarena-in.vercel.app)

---

<p align="center">Made with ❤️ for students preparing for high-stakes exams.</p>
