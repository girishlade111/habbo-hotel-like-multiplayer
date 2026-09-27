# Habbo Hotel-Like Multiplayer Chat

A Habbo Hotel-style multiplayer chat room: a retro isometric pixel room where each visitor is an avatar that can walk around, sit, dance, wave, and chat in real time. Built with Next.js 15, React 19, Supabase Realtime for presence/chat sync, and Tailwind CSS + shadcn/ui. Originally generated with [v0.app](https://v0.app).

## What It Does

Enter a name and you spawn as a pixel avatar in a Habbo-style hotel room rendered in isometric tiles. Move by clicking tiles (A*-style pathfinding in `lib/pathfinding.ts`), trigger emotes (dance, sit, wave, laugh), and talk to other visitors in a live chat panel with spam-guard rate limiting and profanity filtering (`leo-profanity`). Position, emotes, and messages sync across all connected browsers via Supabase Realtime.

## Features

- **Isometric pixel-art room** — custom tile/room renderer (`components/iso-room.tsx`, `lib/iso.ts`, palette)
- **Realtime multiplayer** — avatar positions, emotes, and chat synced via Supabase Realtime channels
- **Click-to-move** — pathfinding across the isometric grid
- **Emotes** — dance, sit, wave, laugh with pixel-avatar sprites
- **Chat panel** — draggable window-style UI (`use-draggable`), spam guard + profanity filter
- **Retro window chrome** — Habbo-style draggable `window-frame` components
- **Chiptune audio** — chiptune fallback player; `/api/proxy-audio` route proxies audio from archive.org / Wikimedia (CORS-safe)
- **Offline demo mode** — if Supabase env vars are missing, the room still runs locally as a single-player demo

## Tech Stack

- [Next.js](https://nextjs.org/) 15 (App Router, one API route: `/api/proxy-audio`)
- [React](https://react.dev/) 19
- [Supabase](https://supabase.com/) (`@supabase/supabase-js`) — Realtime presence + chat
- [Tailwind CSS](https://tailwindcss.com/) + `tailwindcss-animate`
- [shadcn/ui](https://ui.shadcn.com/) scaffolding, [Lucide](https://lucide.dev/) icons
- `leo-profanity` (chat filtering), `@tanstack/react-virtual` (chat list), TypeScript

## Quick Start

```bash
npm install
npm run dev     # → http://localhost:3000
npm run build
```

Without env vars the app runs in **offline demo mode** (single player, no sync).

## Project Structure

```
app/
  page.tsx              # main room page, mode selection
  api/proxy-audio/route.ts  # CORS proxy for chiptune audio files
components/
  iso-room.tsx          # isometric room + avatar renderer
  chat-panel.tsx        # live chat UI
  window-frame.tsx      # draggable retro windows
hooks/
  use-multiplayer.ts    # Supabase Realtime sync + spam guard
  use-draggable.ts
lib/
  iso.ts                # isometric projection math
  pathfinding.ts        # click-to-move pathfinding
  palette.ts            # pixel-art palette
  supabase-client.ts    # browser client (no-op without env)
  chiptune-fallback.ts
```

## Environment Variables

| Variable | Purpose | Required |
|---|---|---|
| `NEXT_PUBLIC_SUPABASE_URL` | Supabase project URL | Yes, for multiplayer |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Supabase anon key | Yes, for multiplayer |

Without them, the app falls back to offline demo mode automatically.

## Deployment

This app needs a Node.js server (it has an API route) plus Supabase for realtime — it cannot be a static export. Deploy options:

- **Vercel** — `vercel deploy`, set the two `NEXT_PUBLIC_SUPABASE_*` env vars (zero-config; this project originated on v0/Vercel)
- **Any Node host** (Render, Railway, self-hosted VPS) — `npm run build && npm start`, set env vars

Not currently deployed as part of the static-site pipeline (static hosts like GitHub Pages can't run the API route or hold env vars).

---

Built by Girish Lade — https://ladestack.in
