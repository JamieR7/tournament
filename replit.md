# PE Tournament Planner

## Overview

A single-page web application for managing physical education tournaments. The app supports 4, 6, or 8 team tournaments with configurable court counts (1-4 courts), offering both round-robin and group stage formats. Features include live scoring, automatic standings calculation, timer management, and an end-of-tournament celebration screen with confetti effects.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

- **Framework**: React with TypeScript, using Vite as the build tool
- **Routing**: Wouter for lightweight client-side routing
- **State Management**: React useState/useEffect hooks with local component state - no external state library
- **Styling**: Tailwind CSS with shadcn/ui component library (New York style variant)
- **Data Fetching**: TanStack React Query configured but primarily used for future API integration

**Design Decision**: The tournament logic is entirely client-side, keeping the app simple and fast without requiring backend persistence. All tournament state lives in the main tournament page component.

### Backend Architecture

- **Runtime**: Node.js with Express
- **Language**: TypeScript with ESM modules
- **Build**: Custom build script using esbuild for server bundling and Vite for client
- **API Structure**: RESTful routes under `/api` prefix (currently minimal, app is frontend-focused)

**Design Decision**: The backend is intentionally lightweight since tournament data doesn't need persistence. The server primarily serves static files and could be extended for saving/loading tournament configurations.

### Data Model

Tournament state is managed through TypeScript types defined in `client/src/lib/tournament-logic.ts`:

- **Teams**: ID, name, and group assignment (A/B for group stage format)
- **Games**: Round number, court assignment, scores, status (scheduled/active/finished), and stage (group/semi/final/placing/league)
- **Courts**: ID and customizable name
- **Standings**: Calculated from game results including played/won/drawn/lost/goals/points

### Key Features

1. **Variable Configuration**: 4/6/8 teams, 1-4 courts, round-robin or group stage formats
2. **Editable Tournament Title**: Custom title field on setup (default "PE TOURNAMENT") displayed throughout
3. **Court Rotation (8-team)**: Games alternate between courts (1, 2, 1, 2...) for balanced play
4. **Conflict-Free Scheduling**: Berger-table rotation for round-robin, interleaved groups for group stage
5. **Timer System**: Per-round countdown with start/pause/reset, 5-second final score display, optional auto-advance
6. **Coach Confirmation**: Modal confirmation before revealing final standings
7. **Live Scoring**: Score updates from court cards or fixtures table with automatic standings recalculation
8. **Finals Structure**: For 8-team group stage - semi-finals, placement games (3rd-8th), and final
9. **Celebration Screen**: Confetti animation overlay before revealing final standings

## External Dependencies

### UI Components
- **shadcn/ui**: Full component library with Radix UI primitives
- **Lucide React**: Icon library
- **canvas-confetti**: End-of-tournament celebration effects

### Database (Configured but not actively used)
- **Drizzle ORM**: PostgreSQL ORM with schema in `shared/schema.ts`
- **PostgreSQL**: Database connection via `DATABASE_URL` environment variable
- Schema includes a basic users table (for future authentication features)

### Development Tools
- **Vite**: Development server with HMR
- **Replit plugins**: Cartographer, dev banner, runtime error overlay
- **TypeScript**: Strict mode enabled with path aliases (@/, @shared/, @assets/)

### Fonts
- **Montserrat**: Primary font family loaded from Google Fonts
- **Chakra Petch, Inter, Orbitron**: Additional fonts for display elements