# Prompt Engineering Guide - WVSU-LC Student Activity Kiosk

The qualitative feedback analysis on the admin console is powered by a structured prompt sent to `gemini-3.5-flash`. This file documents the prompt design, payload structure, and output requirements.

## System Prompt Definition

The prompt is generated dynamically in [server.ts](file:///c:/Gimeno/wvsu-kiosk/server.ts#L739-L780) when an administrator clicks "Generate AI Report".

### Prompt Text
```text
You are the AI engine for the WVSU-LC Student Activity Kiosk.
Perform qualitative semantic, sentiment, and safety reviews for campus student evaluations of the event "{{event.title}}".
The event had a total of {{totalSubmissions}} submissions.

Here is a JSON of student evaluations to analyze:
{{textPayloadForGemini}}

COLLEGE ENROLLED COUNTS:
BSINFO TECH: 350, BSED: 400, BSIT: 300, BSHM: 250

YOUR TASKS:
1. Extract top 3 positive themes from Q7 responses. Write beautiful, specific, action-oriented themes.
2. Extract top 3 improvement areas from Q8 responses. Write concrete physical/logistical critiques.
3. Sieve through comments on safety concerns (like injuries, no first aid blocks) or harassment/discrimination (slurs, exclusionary language, bias). Flag any such submission. Provide its ID, college, the student's exact relevant text, and why you flagged it.
4. Draft exactly 5 recommendations in this structure:
   REC-1: [Short Title] — [2-sentence explanation advising administrative actions, fully incorporating the qualitative and quantitative feedback]
   REC-2: ...
5. Write a Strategic Insights paragraph. This must be exactly 1 paragraph (max 100 words) summarizing the core event triumph, identifying which college was most or least engaged, and pinpointing the single most urgent strategic reform administrators must enforce ahead of the next event.

Return a JSON matching this exact structure:
{
  "positiveThemes": ["Theme 1", "Theme 2", "Theme 3"],
  "improvementAreas": ["Theme 1", "Theme 2", "Theme 3"],
  "flaggedResponses": [
    {
      "submissionId": "...",
      "college": "...",
      "excerpt": "...",
      "reason": "..."
    }
  ],
  "recommendations": [
    {"id": "REC-1", "title": "...", "body": "..."},
    {"id": "REC-2", "title": "...", "body": "..."},
    {"id": "REC-3", "title": "...", "body": "..."},
    {"id": "REC-4", "title": "...", "body": "..."},
    {"id": "REC-5", "title": "...", "body": "..."}
  ],
  "strategicInsights": "..."
}
```

---

## Output Enforcement (JSON Schema)

To guarantee the response matches the application's React state interfaces, we pass a `responseSchema` during the API call:

```typescript
responseSchema: {
  type: Type.OBJECT,
  properties: {
    positiveThemes: {
      type: Type.ARRAY,
      items: { type: Type.STRING }
    },
    improvementAreas: {
      type: Type.ARRAY,
      items: { type: Type.STRING }
    },
    flaggedResponses: {
      type: Type.ARRAY,
      items: {
        type: Type.OBJECT,
        properties: {
          submissionId: { type: Type.STRING },
          college: { type: Type.STRING },
          excerpt: { type: Type.STRING },
          reason: { type: Type.STRING }
        },
        required: ["submissionId", "college", "excerpt", "reason"]
      }
    },
    recommendations: {
      type: Type.ARRAY,
      items: {
        type: Type.OBJECT,
        properties: {
          id: { type: Type.STRING },
          title: { type: Type.STRING },
          body: { type: Type.STRING }
        },
        required: ["id", "title", "body"]
      }
    },
    strategicInsights: { type: Type.STRING }
  },
  required: ["positiveThemes", "improvementAreas", "flaggedResponses", "recommendations", "strategicInsights"]
}
```
