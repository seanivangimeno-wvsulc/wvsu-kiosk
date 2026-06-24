# AI Agents - WVSU-LC Student Activity Kiosk

The WVSU-LC Student Activity Kiosk integrates an intelligent agentic workflow powered by Gemini 3.5 Flash to automatically process, analyze, audit, and summarize student qualitative feedback.

## Agent Persona: Student Feedback Analyst & Quality Assurance Auditor

### Core Responsibility
The agent parses large volumes of student evaluations submitted through the kiosk terminal and provides actionable quality assurance feedback to campus administrators. It acts as an objective, unbiased analyst focused on improving student campus experiences.

### Key Capabilities
1. **Qualitative Sentiment & Theme Extraction**: Identifies top positive themes and areas for improvement from student comments (Q7 & Q8).
2. **Automated Safety & Harassment Audit (Guardrails)**: Scans student remarks for safety risks (injuries, lack of first aid) and student conduct concerns (harassment, slurs, discrimination).
3. **Structured Strategic Planning**: Formulates exactly 5 highly specific administrative recommendations (REC-1 to REC-5) and generates a concise Strategic Insights summary (under 100 words).

---

## Technical Specifications

- **Model**: `gemini-3.5-flash`
- **SDK**: `@google/genai` (v2.4.0)
- **File Location**: Integration resides in [server.ts](file:///c:/Gimeno/wvsu-kiosk/server.ts#L727-L830)
- **Response Format**: `application/json` (strictly matching a structured JSON schema)

---

## Agent Resiliency (Mathematical Fallback)

If the Gemini API key is missing or calls fail due to network/rate issues, the kiosk backend activates a local **Mathematical Fallback Database** ([server.ts](file:///c:/Gimeno/wvsu-kiosk/server.ts#L889-L1041)):
- Formulates quantitative means of event rating fields.
- Retrieves pre-loaded qualitative responses mapped to common event concerns.
- Re-assembles a structured evaluation report without breaking the admin dashboard interface.
