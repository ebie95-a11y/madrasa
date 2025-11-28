# Overview

This is a Qur'an learning application designed for both children and adults. The application features an interactive educational platform with lessons, Surah reading, duas (supplications), and progress tracking. It includes a 3D exploration game environment built with React Three Fiber where users can discover secrets and earn rewards. The application uses a dual-mode interface - a colorful, playful design for children and a more refined aesthetic for adult learners.

# User Preferences

Preferred communication style: Simple, everyday language.

# System Architecture

## Frontend Architecture

**Technology Stack:**
- React with TypeScript for UI components
- Vite as the build tool and development server
- TailwindCSS for styling with a custom design system
- Radix UI for accessible component primitives
- React Three Fiber for 3D graphics and game scenes

**Design Decisions:**
- **Component-based architecture:** The UI is split into reusable components organized by feature (learning modules, game elements, UI primitives)
- **State management:** Uses Zustand stores for global state management (learning progress, audio controls, exploration game state)
- **Dual-mode interface:** Supports both kid-friendly and adult learning modes with different visual themes controlled through the learning store
- **Client-side routing:** Navigation is handled through screen state rather than URL-based routing, keeping the app simple and focused

**Key Features:**
- Interactive Arabic lessons with transliteration and translation
- Surah reader with verse-by-verse navigation
- 3D exploration game with collectible secrets
- Progress tracking with achievements and streaks
- Audio integration for background music and sound effects

## Backend Architecture

**Technology Stack:**
- Express.js for the HTTP server
- TypeScript for type safety
- ESBuild for server bundling

**Design Decisions:**
- **Minimal backend:** The current implementation uses in-memory storage (MemStorage class), designed to be easily swapped for database persistence
- **Storage interface pattern:** IStorage interface defines CRUD operations, allowing easy transition from in-memory to database-backed storage
- **API-first design:** All application routes are prefixed with `/api` for clear separation between API and static content
- **Static file serving:** Production builds serve pre-compiled client assets with SPA fallback routing

**Server Structure:**
- `server/index.ts`: Main server setup with Express middleware
- `server/routes.ts`: API route registration (currently minimal, ready for expansion)
- `server/storage.ts`: Storage interface and in-memory implementation
- `server/static.ts`: Static file serving for production builds
- `server/vite.ts`: Vite development server integration with HMR

## Data Storage

**Current Implementation:**
- In-memory storage using Map data structures
- MemStorage class implements IStorage interface for user management

**Database Schema (Drizzle ORM):**
The application defines PostgreSQL schemas but currently uses in-memory storage:

- **users:** User accounts with username, password, display name, adult mode flag
- **surahs:** Qur'anic chapters with Arabic names, translations, verse counts, difficulty levels
- **verses:** Individual Qur'anic verses with Arabic text, transliteration, translation, tajweed rules
- **lessons:** Educational content organized by category with difficulty progression
- **userProgress:** Tracks completion status, practice counts, quiz scores per user per lesson
- **achievements:** Unlockable milestones with unlock conditions
- **duas:** Collection of supplications with Arabic text and translations

**Migration Strategy:**
- Drizzle Kit configured for PostgreSQL migrations
- Schema defined in `shared/schema.ts` for type sharing between client and server
- Ready to connect to Neon Database (serverless PostgreSQL)

## External Dependencies

**Database:**
- Neon Database (configured but not currently connected)
- Drizzle ORM for type-safe database queries
- PostgreSQL dialect

**UI Libraries:**
- Radix UI component primitives for accessibility
- Tailwind CSS for utility-first styling
- React Three Fiber and Drei for 3D rendering
- Lucide React for icons

**Development Tools:**
- Vite with React plugin for fast development and HMR
- GLSL plugin for shader support in 3D scenes
- TypeScript for type safety across the stack
- ESBuild for production server bundling

**Audio:**
- Web Audio API for background music and sound effects
- Audio files expected in `/public/sounds/` directory

**3D Assets:**
- Support for GLTF/GLB 3D models
- Custom shaders via GLSL plugin

**Session Management:**
- Express session middleware (configured for future authentication)
- Connect-pg-simple for PostgreSQL session storage (when database is connected)

**Build Strategy:**
- Client builds to `dist/public` for static serving
- Server bundles to `dist/index.cjs` with selective dependency bundling
- Allowlist approach for dependencies to bundle (reduces cold start times)
- External dependencies excluded from bundle for smaller output