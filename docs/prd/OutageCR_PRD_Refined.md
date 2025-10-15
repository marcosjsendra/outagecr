<context>
# Overview  
¿Hay Luz? (OutageCR) is a public, user-friendly platform designed to empower Costa Ricans to check and report electricity, water, and internet outages in real time. The platform solves the problem of unreliable utility information by leveraging crowdsourced data to provide accurate, up-to-date information through an intuitive map-based or list-based interface. It's valuable for residents, businesses, and emergency services who need quick access to outage information, especially in areas with frequent utility disruptions. The web app is optimized for mobile use, ensuring accessibility, speed, and reliability even in low-bandwidth conditions.

# Core Features  

## Map-Based Outage Visualization

- **What it does**: Displays real-time outages on an interactive map of Costa Rica with color-coded markers and filtering capabilities
- **Why it's important**: Provides immediate visual understanding of outage situations across different geographic areas
- **How it works**: Uses OpenStreetMap as base layer with custom markers for different outage types and statuses, supports zooming by province/canton

## List-Based Outage View  

- **What it does**: Alternative interface showing outages in a searchable, sortable list format
- **Why it's important**: Ensures accessibility for users with limited map interaction capabilities or those preferring tabular data
- **How it works**: Displays outage data in table format with sorting by location, time, type, and status

## Anonymous Outage Reporting

- **What it does**: Allows users to submit outage reports without requiring account creation
- **Why it's important**: Lowers barrier to contribution, increasing data volume and platform usefulness
- **How it works**: Simple form with mandatory fields (location, type, description), optional photo upload, GPS coordinates capture with explicit consent

## Real-Time Notifications

- **What it does**: Sends optional browser push notifications for new outages in user-selected areas
- **Why it's important**: Keeps users informed about relevant outages without requiring active platform monitoring
- **How it works**: Browser-based push notification system with user-defined geographic preferences

## Community Validation System

- **What it does**: Enables users to upvote/downvote reports to validate accuracy
- **Why it's important**: Improves data reliability through community consensus and helps identify false reports
- **How it works**: Voting mechanism on each report with automated flagging of suspicious activity

# User Experience  

## User Personas

- **Primary Persona**: Costa Rican resident (18-65, Spanish-speaking) experiencing frequent utility outages, needs quick information and simple reporting process
- **Secondary Persona**: Local business owner requiring outage information for operational planning, values historical data and reliability
- **Tertiary Persona**: Emergency service personnel needing real-time outage information for response coordination

## Key User Flows

### Viewing Outages

1. User accesses platform via mobile or desktop
2. Map loads with current outage data for Costa Rica
3. User can filter by outage type, status, or geographic area
4. User clicks on markers for detailed information
5. Alternative list view available for text-based browsing

### Reporting Outages

1. User clicks "Report Outage" button
2. Form loads with mandatory fields: location (auto-filled via GPS with consent), type, description
3. Optional photo upload available
4. Google reCAPTCHA verification
5. Anonymous submission with confirmation message

### Managing Preferences

1. User accesses notification settings
2. Selects geographic areas of interest
3. Chooses notification types (new outages, status updates)
4. Preferences saved for future sessions

## UI/UX Considerations

- **Mobile-First Design**: Optimized for smartphone usage with responsive layout
- **Performance**: Lightweight assets, lazy loading, text-only mode for low bandwidth
- **Accessibility**: WCAG compliance, high contrast mode, screen reader support
- **Localization**: Spanish primary with English support for expats/tourists
- **Progressive Web App**: Offline capabilities, home-screen installation support
- **Brand Consistency**: Follows established brand guidelines for colors, icons, and typography
</context>

<PRD>
# Technical Architecture  
## System Components
### Frontend Application
- **Framework**: React.js with TypeScript for type safety and maintainability
- **Styling**: Tailwind CSS for responsive, utility-first styling
- **State Management**: React Context API for local state, React Query for server state
- **Map Integration**: Leaflet.js with OpenStreetMap tiles for interactive mapping
- **PWA Features**: Workbox for service worker registration and offline caching

### Backend & Database

- **Platform**: Supabase as Backend-as-a-Service (BaaS)
- **Database**: PostgreSQL with optimized schema for geospatial queries
- **API Layer**: Supabase auto-generated REST API with Row Level Security (RLS)
- **Authentication**: Supabase Auth supporting anonymous and registered users
- **Real-time**: Supabase Realtime for live outage updates

### External Services

- **Geocoding**: OpenStreetMap Nominatim API for address resolution
- **CAPTCHA**: Google reCAPTCHA v2 for spam prevention
- **Weather Data**: Optional integration with OpenWeatherMap API for correlation analysis
- **Email Service**: Supabase built-in email for notifications

## Data Models

### Outage Reports

```typescript
interface OutageReport {
  id: string;
  location: {
    lat: number;
    lng: number;
    address?: string;
    canton: string;
    province: string;
  };
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

### User Preferences

```typescript
interface UserPreferences {
  id: string;
  userId: string;
  notificationAreas: Array<{
    province: string;
    canton?: string;
    radius: number; // in kilometers
  }>;
  notificationTypes: ('new_outage' | 'status_update')[];
  createdAt: Date;
  updatedAt: Date;
}
```

### Geographic Boundaries

```typescript
interface AdministrativeBoundary {
  id: string;
  type: 'province' | 'canton' | 'district';
  name: string;
  parentId?: string;
  geometry: GeoJSON; // Simplified polygon data
  properties: {
    population?: number;
    area: number;
  };
}
```

## APIs and Integrations

### Internal APIs (Supabase Auto-generated)

- `POST /outage-reports` - Create new outage report
- `GET /outage-reports` - List reports with filtering and pagination
- `PATCH /outage-reports/:id` - Update report status or details
- `POST /outage-reports/:id/vote` - Upvote/downvote report
- `GET /boundaries` - Retrieve administrative boundary data

### External API Integrations

- **OpenStreetMap Nominatim**: Reverse geocoding for address resolution
- **Google reCAPTCHA**: Verification for report submissions
- **Supabase Auth**: User authentication and session management
- **Browser Push API**: Real-time notifications

## Infrastructure Requirements

### Hosting and Deployment

- **Frontend Hosting**: Sevalla static site hosting with GitHub integration
- **Database Hosting**: Supabase PostgreSQL with automated backups
- **CDN**: Global content delivery network for static assets
- **Domain Management**: Custom SSL certificate and DNS configuration

### Performance and Scalability

- **Database Indexing**: Geospatial indexes on location data, composite indexes on status and type
- **Caching Strategy**: Redis caching for frequently accessed boundary data
- **API Rate Limiting**: Prevent abuse while allowing legitimate usage
- **Asset Optimization**: Image compression, lazy loading, and CDN delivery

### Security and Compliance

- **Data Encryption**: HTTPS for all communications, encrypted database storage
- **Authentication**: Anonymous user sessions with optional account creation
- **Privacy Compliance**: GDPR, CCPA, and Costa Rican data protection law compliance
- **Data Retention**: 12-month retention for anonymous reports with automatic anonymization

# Development Roadmap  

## MVP Requirements

### Core Infrastructure

- Supabase project setup with database schema
- Authentication system for anonymous users
- Basic API endpoints with Row Level Security
- Administrative boundary data import (provinces and cantons)
- Initial dummy outage data seeding

### Essential Frontend Features

- React application setup with TypeScript and Tailwind CSS
- Basic map interface using Leaflet and OpenStreetMap
- Outage marker display with basic styling by type
- Simple list view as alternative to map
- Anonymous outage reporting form with CAPTCHA
- Mobile-responsive design and basic PWA features

### Minimum Viable Functionality

- View outages on map (Guanacaste Province only)
- Submit new outage reports (electricity and water only)
- Basic filtering by outage type
- Simple status management (active/resolved only)
- Email notifications for status updates

## Phase 2 Enhancements

### Expanded Geographic Coverage

- Full Costa Rica coverage with all 82 cantons
- District-level boundary data and zooming
- Improved geocoding and address resolution
- Location search and autocomplete functionality

### Enhanced User Experience

- Complete outage status system (all 6 statuses)
- Internet outage type added
- Photo upload for reports
- Community voting system implementation
- Push notification system
- Advanced filtering and search capabilities

### Performance and Reliability

- Offline mode with PWA features
- Performance optimization for low-bandwidth scenarios
- Improved spam detection and moderation
- Data validation and quality checks

## Phase 3 Advanced Features

### Premium Features

- SMS alert subscription system ($2/month)
- Historical data access and export functionality
- Business API for outage data integration
- Advanced analytics and reporting dashboard

### Platform Maturity

- Weather correlation analysis
- Predictive outage patterns
- Integration with utility providers (if possible)
- Advanced moderation tools
- Multi-language support expansion

### Community and Growth

- User feedback and rating system
- Social sharing features
- Community management tools
- Affiliate partnership integration

# Logical Dependency Chain

## Foundation Layer (Week 1-2)

### 1. Project Setup and Infrastructure

- Initialize Supabase project with proper configuration
- Set up database schema with migrations
- Configure authentication for anonymous users
- Implement Row Level Security policies
- Set up GitHub repository with CI/CD pipeline

### 2. Geographic Data Foundation

- Source and import Costa Rica administrative boundaries
- Process and optimize GeoJSON files for performance
- Create database tables for provinces, cantons, and districts
- Implement basic geospatial queries
- Seed initial boundary data

### 3. Core Frontend Structure

- Create React application with TypeScript setup
- Configure Tailwind CSS and design system
- Implement basic routing and layout structure
- Set up state management with React Query
- Configure PWA manifest and service worker basics

## Core Functionality Layer (Week 3-4)

### 4. Map Interface Foundation

- Integrate Leaflet.js with OpenStreetMap
- Implement basic map controls and interactions
- Create outage marker component with basic styling
- Implement map clustering for performance
- Add basic layer controls for outage types

### 5. Data Management Core

- Create outage reports table with proper schema
- Implement CRUD operations for outage reports
- Add basic validation and error handling
- Create real-time subscriptions for live updates
- Implement basic data seeding for demonstration

### 6. User Interaction Basics

- Create outage reporting form component
- Implement geolocation capture with explicit consent
- Add Google reCAPTCHA integration
- Create basic list view component
- Implement simple status management interface

## Usable Product Layer (Week 5-6)

### 7. Essential User Experience

- Implement mobile-responsive design
- Create user preference management system
- Add basic filtering and search functionality
- Implement email notification system
- Create basic user onboarding flow

### 8. Initial Launch Features

- Focus on Guanacaste Province only
- Support electricity and water outage types
- Implement basic active/resolved status system
- Add community feedback mechanism
- Create basic analytics and monitoring

### 9. Performance and Polish

- Optimize map loading performance
- Implement proper error boundaries
- Add loading states and user feedback
- Create basic documentation and help system
- Implement basic SEO optimization

## Enhancement Layer (Week 7-8)

### 10. Expanded Coverage

- Add full Costa Rica coverage
- Implement all 82 cantons with district data
- Add internet outage type support
- Implement complete 6-status system
- Add advanced filtering capabilities

### 11. Advanced Features

- Implement community voting system
- Add photo upload functionality
- Create push notification system
- Implement offline PWA features
- Add advanced moderation tools

### 12. Platform Maturity

- Performance optimization for low-bandwidth
- Implement advanced spam detection
- Create comprehensive analytics
- Add user feedback and rating system
- Prepare for Phase 3 premium features

# Risks and Mitigations  

## Technical Challenges

### Risk: Complex Geospatial Data Performance

- **Mitigation**: Use simplified GeoJSON shapes, implement server-side clustering, add proper database indexing, and consider tile-based rendering for large datasets

### Risk: Real-time Data Synchronization

- **Mitigation**: Implement optimistic UI updates, add conflict resolution strategies, use Supabase Realtime with proper error handling, and create fallback mechanisms

### Risk: Mobile Performance on Low-End Devices

- **Mitigation**: Implement code splitting, use lightweight map libraries, add progressive loading, create text-only mode, and optimize image assets

## MVP Definition Challenges

### Risk: Scope Creep in MVP

- **Mitigation**: Strict focus on Guanacaste Province only, limit to electricity/water outages, implement basic active/resolved status, and defer advanced features to Phase 2

### Risk: Over-Engineering Initial Solution

- **Mitigation**: Use existing Supabase features instead of custom backend, leverage OpenStreetMap instead of custom mapping solution, implement simple UI first, and add complexity incrementally

### Risk: Data Quality and Validation

- **Mitigation**: Implement CAPTCHA and voting system from start, add automated duplicate detection, create clear data validation rules, and plan for manual moderation oversight

## Resource Constraints

### Risk: Limited Development Team

- **Mitigation**: Prioritize features that deliver maximum user value, use no-code/low-code solutions where possible (Supabase), focus on automation, and implement comprehensive testing

### Risk: Third-Party Service Limitations

- **Mitigation**: Implement fallback mechanisms for external APIs, design for service degradation, monitor usage limits, and have alternative providers ready

### Risk: Geographic Data Availability

- **Mitigation**: Source boundary data from multiple reliable sources, implement data validation scripts, create manual override capabilities, and plan for community data correction

## User Adoption Risks

### Risk: Low Initial User Engagement

- **Mitigation**: Pre-populate with realistic demo data, implement easy sharing features, target specific communities in Guanacaste, and create incentive mechanisms for early adopters

### Risk: Data Accuracy Concerns

- **Mitigation**: Implement clear data accuracy disclaimers, add verification systems, create user education materials, and build trust through transparency

# Appendix  

## Research Findings

### Costa Rican Utility Landscape

- ICE (Instituto Costarricense de Electricidad) dominates electricity sector
- AyA (Instituto Costarricense de Acueductos y Alcantarillados) handles water services
- Private internet providers include ICE, Claro, Movistar, and smaller regional providers
- Outage patterns vary by region with coastal areas experiencing more weather-related disruptions

### Geographic Data Sources

- OpenStreetMap provides comprehensive boundary data for Costa Rica
- GeoJSON files available through various open data repositories
- Costa Rican government publishes official boundary data through IGN (Instituto Geográfico Nacional)
- Performance optimization requires simplification of complex boundary shapes

### Competitive Analysis

- No comprehensive, real-time outage tracking platform currently exists for Costa Rica
- Utility providers offer limited outage information through their own channels
- Social media serves as primary source for outage information currently
- Opportunity exists to become the authoritative source for outage information

## Technical Specifications

### Database Schema Details

```sql
-- Outage Reports Table
CREATE TABLE outage_reports (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  location GEOGRAPHY(POINT, 4326) NOT NULL,
  address TEXT,
  canton TEXT NOT NULL,
  province TEXT NOT NULL,
  outage_type TEXT NOT NULL CHECK (outage_type IN ('electricity', 'water', 'internet')),
  status TEXT NOT NULL CHECK (status IN ('active', 'reported', 'resolved', 'planned_maintenance', 'scheduled', 'critical')),
  description TEXT NOT NULL,
  image_url TEXT,
  reported_at TIMESTAMPTZ DEFAULT NOW(),
  resolved_at TIMESTAMPTZ,
  reporter_id TEXT NOT NULL,
  upvotes INTEGER DEFAULT 0,
  downvotes INTEGER DEFAULT 0,
  is_verified BOOLEAN DEFAULT FALSE
);

-- Administrative Boundaries Table
CREATE TABLE administrative_boundaries (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  boundary_type TEXT NOT NULL CHECK (boundary_type IN ('province', 'canton', 'district')),
  name TEXT NOT NULL,
  parent_id UUID REFERENCES administrative_boundaries(id),
  geometry GEOGRAPHY(POLYGON, 4326) NOT NULL,
  properties JSONB DEFAULT '{}'
);

-- User Preferences Table
CREATE TABLE user_preferences (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id TEXT NOT NULL,
  notification_areas JSONB NOT NULL,
  notification_types TEXT[] DEFAULT '{}',
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

### API Rate Limiting Strategy

- Anonymous users: 10 reports per hour per IP address
- Report viewing: Unlimited with caching
- API calls: 1000 requests per hour per IP
- Geocoding requests: 100 per hour per IP
- Notification subscriptions: 5 areas per user

### Performance Benchmarks

- Map load time: < 3 seconds on 3G network
- Report submission: < 5 seconds end-to-end
- Database queries: < 100ms for common operations
- Mobile page load: < 2 seconds for initial view
- Offline functionality: Core features work without internet

## Supporting Documentation

- **Brand Guidelines**: `docs/OutageCR_brandguidelines.md`
- **Moderation & Spam Prevention Plan**: `docs/OutageCR_PlanModeration_Spam_Prevention_Plan.md`
- **Up Vote/Down Vote System**: `docs/OutageCR_UpVote-DownVote.md`
- **API and Monetization Strategy**: `docs/OutageCR_API_and_Monetization_Strategy.md`
- **Supabase Setup**: `docs/OutageCR_supabase_setup.md`
- **Roadmap**: `docs/OutageCR_roadmap.md`

## Legal and Compliance References

- Costa Rican Data Protection Law: Ley de Protección de la Persona frente al Tratamiento de sus Datos Personales, Ley No. 8968
- GDPR Compliance Requirements for EU users
- CCPA Compliance for California residents
- OpenStreetMap Tile Usage Terms and Conditions
- Google reCAPTCHA Terms of Service
</PRD>
