# קרנף נדל״ן CRM

מערכת CRM לניהול לידים ולקוחות בתחום הנדל״ן — כולל בוט מכירות WhatsApp, אוטומציות ותובנות AI.

## טכנולוגיות

- React 18 + TypeScript + Vite
- Supabase (PostgreSQL, Auth, Realtime, Edge Functions)
- Tailwind CSS + shadcn/ui (RTL)
- WhatsApp Cloud API + Instagram Messaging API
- Make.com (אוטומציות)

## התקנה

```sh
npm install
npm run dev
```

## פקודות

| פקודה | תיאור |
|-------|-------|
| `npm run dev` | שרת פיתוח (port 8080) |
| `npm run build` | בנייה לפרודקשן |
| `npm run lint` | ESLint |
| `npx supabase functions serve` | Edge functions מקומי |
| `npx supabase functions deploy <name>` | דיפלוי edge function |
