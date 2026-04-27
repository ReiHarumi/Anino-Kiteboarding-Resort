# PROJECT_MANIFEST: ANINO_RETREAT_V2_ARCHITECTURE
**Status:** [DRAFT_READY]
**Target:** High-Authority Luxury Portfolio + AI Logistics Engine
**Stack:** Next.js (App Router) | Supabase | PostgreSQL (pgvector) | DeepSeek V3.2

---

## 0. EXECUTIVE ARCHITECTURE SUMMARY
Transitioning the Anino Retreat digital asset from a legacy static presence to a high-conversion authority funnel. The system is engineered to eliminate "Trust Decay" through high-fidelity visual delivery (1440p) and bypass the "Logistics Wall" via a Retrieval-Augmented Generation (RAG) AI agent.

---

## 1. FRONT-END: USER REQUIREMENT DOCUMENT (URD)

### 1.1 Technical Performance Parameters
| Metric | Target | Implementation |
| :--- | :--- | :--- |
| **Visual Fidelity** | 1440p (QHD) | Adaptive Bitrate Streaming (ABR) via Mux/Cloudinary |
| **LCP (Largest Contentful Paint)** | < 1.2s | Next/Image, Edge Caching, Route Prefetching |
| **Rendering Strategy** | Hybrid (SSR/ISR) | SSR for dynamic booking; ISR (3600s) for static assets |
| **Styling** | Utility-First | Tailwind CSS with Framer Motion for micro-interactions |

### 1.2 B2B Funnel Progression Map
| Funnel Stage | Core UI Component | Primary CTA | Data Capture | Logic |
| :--- | :--- | :--- | :--- | :--- |
| **Awareness** | Hero Section (1440p Loop) | "Explore the Island" | Session Metadata | Viewport-trigger: Lazy load heavy assets |
| **Education** | Logistics Interactive Wizard | "Plan My Journey" | Origin, Dates | State-synced URL for session persistence |
| **Conversion** | Availability Calendar | "Book My Villa" | PII, Booking Dates | Real-time fetch from Supabase `villas` table |
| **Loyalty** | Member Portal (Admin) | "Manage Booking" | Auth Token | Server Component (Strict Role-Based Access) |

### 1.3 CRO & A/B Testing Matrix
| Component | Baseline (Control) | Variant A (Test) | Success Metric | Endpoint Triggered |
| :--- | :--- | :--- | :--- | :--- |
| **Hero CTA** | "Inquire Now" | "Reserve Your Window" | CTR % | Lead_Capture_Node |
| **Logistics UI** | Static Text FAQ | Interactive Logic-Step Form | Lead Yield | CRM_Profiling_Node |
| **Booking Flow** | Email Redirect | In-App Calendar Sync | Conversion Rate | Stripe_Session_Create |

---

## 2. BACK-END: REQUIREMENT ANALYSIS DOCUMENT (RAD)

### 2.1 Database Schema (Supabase/PostgreSQL)
| Table | Columns | Index Type | Purpose |
| :--- | :--- | :--- | :--- |
| **villas** | `id, name, price_base, metadata` | B-tree | Core inventory management |
| **logistics_kb** | `id, content, embedding (vector)` | **hnsw** | Vector store for RAG schedules/tides |
| **leads** | `id, email, intent_score, ai_summary` | B-tree | CRM Lead tracking & autonomous triage |
| **bookings** | `id, villa_id, user_id, date_range` | GIST (Exclude) | Prevention of double-booking/overlap |

### 2.2 LLM Pipeline Matrix (RAG Implementation)
| Agent Name | Intelligence Layer | Context Source | Core Logic |
| :--- | :--- | :--- | :--- |
| **Logistics Agent** | DeepSeek V3.2 | `logistics_kb` (pgvector) | Semantic search -> Context injection -> Route generation |
| **Lead Scorer** | Llama 4 Maverick | Interaction Logs | Behavioral analysis -> Numeric intent score (0-100) |

### 2.3 AI Resilience & Fallback Protocol
| Primary Node | Failure Trigger | Fallback Action | Alert Routing |
| :--- | :--- | :--- | :--- |
| DeepSeek (Main) | 5xx Error / >2s Latency | Gemini 2.5 Flash-Lite (Backup) | Slack Webhook (Ops-Alert) |
| Vector Search | < 0.75 Similarity Score | Static FAQ Document Delivery | Log to Engineering Dashboard |

---

## 3. CLIENT MANAGEMENT & OPERATIONS

### 3.1 Admin Dashboard Functions (CRUD)
* **Inventory Toggle:** Global calendar interface for manual blackout/release of dates.
* **Seasonal Multiplier:** Dynamic field to adjust pricing across all villa types by percentage.
* **Asset Hot-Swap:** Managed upload interface for 1440p video/stills triggering ISR revalidation.
* **AI Log Audit:** Real-time stream of RAG conversations for manual intervention and QA.

---

## 4. QUALITY ASSURANCE (QA) MATRIX
| Component | Evaluation Metric | Pass Threshold | Testing Methodology |
| :--- | :--- | :--- | :--- |
| **System Latency** | P95 Response Time | < 300ms | K6 Load Testing (50 concurrent users) |
| **RAG Accuracy** | Hallucination Rate | < 1% | Automated verification against schedule CSV |
| **Core Web Vitals** | Lighthouse Performance | > 90 | Automated CI/CD pipeline audit |

---

**[DOCUMENT_END]**
