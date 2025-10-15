# Cline Workspace Rules: OutageCR (¿Hay Luz?)

## 1. Project Overview

**Project Name**: ¿Hay Luz? (OutageCR)
**Concept**: A public, user-friendly platform designed to empower Costa Ricans to check and report electricity, water, and internet outages in real time.
**Primary Goal**: Solve the problem of unreliable utility information by leveraging crowdsourced data to provide accurate, up-to-date information through an intuitive map-based or list-based interface.
**Target Audience**: Residents, businesses, and emergency services in Costa Rica who need quick access to outage information.
**Key Value**: Mobile-optimized for accessibility, speed, and reliability, even in low-bandwidth conditions.

## 2. Core Features (From PRD)

- **Map-Based Outage Visualization**: Interactive map of Costa Rica with color-coded markers and filtering (OpenStreetMap + Leaflet.js).
- **List-Based Outage View**: Searchable, sortable list alternative to the map.
- **Anonymous Outage Reporting**: Simple form for users to submit reports without account creation (Location, Type, Description, optional photo). Includes Google reCAPTCHA.
- **Real-Time Notifications**: Optional browser push notifications for new outages in user-selected areas.
- **Community Validation System**: Users can upvote/downvote reports to validate accuracy and identify false reports.

## 3. Technical Architecture (From PRD)

### Frontend (`hayluz-outagecr` directory)

- **Framework**: React.js with TypeScript.
- **Build Tool**: Vite.
- **Styling**: Tailwind CSS.
- **State Management**: React Context API (local), React Query (server).
- **Map Integration**: Leaflet.js with OpenStreetMap.
- **PWA**: Workbox for service worker and offline caching.
- **Hosting**: Sevalla (linked to GitHub `https://github.com/marcosjsendra/outagecr`).

### Backend & Database

- **Platform**: Supabase (BaaS).
- **Database**: PostgreSQL with geospatial queries (PostGIS enabled).
- **API**: Supabase auto-generated REST API with Row Level Security (RLS).
- **Authentication**: Supabase Auth (anonymous users supported).
- **Real-time**: Supabase Realtime.

### Key Data Models (TypeScript Interfaces)

```typescript
interface OutageReport {
  id: string;
  location: { lat: number; lng: number; address?: string; canton: string; province: string; };
  type: 'electricity' | 'water' | 'internet';
  status: 'active' | 'reported' | 'resolved' | 'planned_maintenance' | 'scheduled' | 'critical';
  description: string;
  imageUrl?: string;
  reportedAt: Date;
  resolvedAt?: Date;
  reporterId: string; // anonymous user ID
  upvotes: number;
  downvotes: number;
  isVerified: boolean;
}
```

### External Services

- **Geocoding**: OpenStreetMap Nominatim API.
- **CAPTCHA**: Google reCAPTCHA v2.
- **Email**: Supabase built-in email.

## 4. Branding & UI/UX (From `OutageCR_brandguidelines.md`)

- **Brand Palette**:
  - Primary: Amber (`#FFB300`)
  - Accent: Deep Blue (`#1565C0`)
  - Neutral Dark: Charcoal Gray (`#212121`)
  - Neutral Light: Cool Gray (`#ECEFF1`)
- **Map Marker/Status Colors**:
  - Active Outage: Red (`#E53935`)
  - Reported: Amber (`#FFB300`)
  - Resolved: Green (`#43A047`)
  - Planned Maintenance: Blue (`#1E88E5`)
  - Critical: Purple (`#8E24AA`)
- **Font**: Funnel Display (Google Fonts).
- **UI/UX Goal**: Community-first, data-light, mobile-friendly. Central map, floating "Report" button, timeline/feed, simple stats.

## 5. Development Roadmap & Progress (From `OutageCR_roadmap.md`)

**Overall Timeline**:

- **Phase 1: MVP**: Weeks 1-6 (Focus: Guanacaste province, electricity/water outages).
- **Phase 2: Enhancement**: Weeks 7-12 (Full CR coverage, internet outages, PWA features).
- **Phase 3: Monetization**: Months 7-12 (Premium features, API access).

**Current Status**: All tasks are `[ ] Not Started`. The project is in the initial setup phase.

### Phase 1: MVP (Weeks 1-6) - Key Tasks

- **Milestone 1.1: Foundation (Week 1-2)**
  - [ ] 1.1.1: Initialize Supabase project, schema, auth.
  - [ ] 1.1.4: Import Guanacaste administrative boundaries.
  - [ ] 1.1.6: Create React app (TypeScript, Tailwind, Vite).
  - [ ] 1.1.9: Configure Sevalla hosting.
- **Milestone 1.2: Core Functionality (Week 3-4)**
  - [ ] 1.2.1: Integrate Leaflet.js map for Guanacaste.
  - [ ] 1.2.4: Create `outage_reports` table and CRUD ops.
  - [ ] 1.2.6: Create reporting form (Location, Type, Description, reCAPTCHA).
- **Milestone 1.3: Usable Product (Week 5-6)**
  - [ ] 1.3.1: Mobile-responsive design.
  - [ ] 1.3.8: Implement basic upvote/downvote.
- **Milestone 1.4: Testing & Launch (Week 6)**
  - [ ] 1.4.5: Production deployment via Sevalla.
  - [ ] 1.4.6: MVP Launch!

## 6. Key Decisions & Requirements (From `human_requirements_OutageCR.md`)

- **Tech Stack**: Confirmed as per PRD (React, Supabase, OpenStreetMap, Sevalla).
- **Initial Focus**: Guanacaste province only for MVP.
- **Map Detail**: Start with Cantons (82 total).
- **Location Capture**: GPS coordinates with explicit consent button.
- **Outage Types (MVP)**: Electricity, Water. (Internet added in Phase 2).
- **Statuses (MVP)**: Active, Resolved. (Full 6-status system in Phase 2).
- **Monetization (MVP)**: No ads initially. Revisit after traction.
- **Team**: Marcos Sendra (UI/UX, FE Dev), Cline with GLM-4.6 (BE, other tasks).
- **Budget**: No initial budget. In-house development.
- **Review**: CodeRabbit for code reviews.
- **Testing/Deployment**: UAT by Marcos Sendra. Direct production rollout.

## 7. Supporting Documentation (Located in `docs/`)

- **`OutageCR_PRD_Refined.md`**: Detailed Product Requirements Document.
- **`OutageCR_roadmap.md`**: Detailed development roadmap with tasks and milestones.
- **`OutageCR_brandguidelines.md`**: Logo assets, color palettes, icons, font.
- **`OutageCR_supabase_setup.md`**: Step-by-step guide for Supabase configuration.
- **`OutageCR_PlanModeration_Spam_Prevention_Plan.md`**: Strategy for ensuring data quality.
- **`OutageCR_UpVote-DownVote.md`**: Detailed design for the community validation system.
- **`OutageCR_API_and_Monetization_Strategy.md`**: Plan for future API access and revenue.
- **`human_requirements_OutageCR.md`**: Decisions and inputs from the product owner.

## 8. Performance & Security Benchmarks (From PRD)

- **Map load time**: < 3 seconds on 3G network.
- **Report submission**: < 5 seconds end-to-end.
- **Database queries**: < 100ms for common operations.
- **Mobile page load**: < 2 seconds for initial view.
- **Offline functionality**: Core features work without internet.
- **API Rate Limiting**:
  - Anonymous users: 10 reports/hour/IP.
  - Report viewing: Unlimited (cached).
  - API calls: 1000 requests/hour/IP.
  - Geocoding: 100 requests/hour/IP.
  - Notification subscriptions: 5 areas/user.

## 9. Important Considerations

- **Language**: Primary is Spanish. English support for expats/tourists.
- **Data Privacy**: Comply with Costa Rican Ley No. 8968, GDPR, CCPA. Anonymous report retention: 12 months.
- **Disclaimer**: Crowdsourced data is not guaranteed to be 100% accurate. Users should verify with service providers.
- **Initial Data**: Platform will launch with pre-populated dummy data for Guanacaste.
