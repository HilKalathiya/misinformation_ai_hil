# Misinformation AI frontend

Next.js 14 + TypeScript app for the misinformation analyser. See the [main README](../README.md) for how the system works.

```bash
npm install
npm run dev        # http://localhost:3000
npm run build
npm test           # Jest + React Testing Library
```

It talks to the FastAPI backend at `NEXT_PUBLIC_API_URL` (default `http://127.0.0.1:8000`).

| Path | Purpose |
| --- | --- |
| `app/page.tsx` | Landing page and text input |
| `app/analyze/page.tsx` | Analysis results |
| `components/` | Input, result cards, header, footer, theme toggle, UI primitives |
| `lib/api.ts` | Calls `POST /api/analyze` |
