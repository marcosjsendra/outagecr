# Human Requirements for OutageCR Development

This document outlines the information, decisions, and inputs required from the product owner/stakeholder to proceed with the development of the ¿Hay Luz? (OutageCR) platform.

## 1. Technical Stack & Infrastructure Confirmation

The PRD suggests a specific technical stack. Please confirm or provide alternatives for the following:

- **Frontend Framework**: React.js with Tailwind CSS.
  - Confirmation: [ ] Yes, proceed as suggested. [ ] No, propose alternative: _________________________
  - Rationale for alternative (if any):

- **Backend & Database**: Supabase (PostgreSQL database, API, Auth).
  - Confirmation: [x] Yes, proceed with Supabase. [ ] No, propose alternative: _________________________
  - Rationale for choice: Supabase provides a scalable backend with a generous free tier, including a PostgreSQL database, instant APIs, and authentication, which aligns with our goal for a lightweight, fast, and cost-effective MVP.

- **Map Provider**: OpenStreetMap or Leaflet.
  - Confirmation: [x] Yes, proceed as suggested. [ ] No, propose alternative (e.g., Google Maps API, Mapbox): _________________________
  - Rationale for alternative (if any):

- **Hosting Provider (Frontend)**: Servall (Static Site Hosting).
  - Confirmation: [x] Yes, proceed with Servall. [ ] No, propose alternative: _________________________
  - Rationale for choice: Servall offers a generous free tier for static sites with a global CDN, auto-deploys from Git, and scalability, which is ideal for our React frontend and aligns with our cost-effective MVP strategy.

- **Authentication Service**: Supabase Authentication (for anonymous and registered users).
  - Confirmation: [x] Yes, proceed with Supabase Auth. [ ] No, propose alternative: _________________________
  - Rationale for choice: Since we are using Supabase for our backend and database, using its built-in authentication service simplifies the architecture, ensures seamless integration, and leverages its generous free tier for user management.

## 2. Design & User Experience (UX)

- **Brand Identity**:
  - Do you have existing brand guidelines (logos, color schemes, fonts)? [ ] Yes (please provide assets/links). [ ] No, needs to be created.
  - Preferred color palette (especially for map markers - e.g., red for outages, green for resolved):
  - Any specific UI/UX preferences or examples of similar platforms you like/dislike?

- **Map Interface Specifics**:
  - Default view level for the map (e.g., country overview, specific province)?
  - Level of detail for administrative boundaries (Province, Canton, District)? The PRD mentions all three; is this granularity required for MVP?
  - Specific icons or marker styles desired for different outage types (Electricity, Water)?

- **Reporting Form Details**:
  - Mandatory vs. Optional fields in the outage report form (Location, Type, Description, Photo)?
  - Consent mechanism for geolocation (explicit button, or on form load with clear disclaimer)?
  - CAPTCHA preference (e.g., Google reCAPTCHA, hCaptcha, a simple math question)?

## 3. Data & Content

- **Outage Types**:
  - The PRD mentions "electricity" and "water". Are there any other utility types to consider for MVP or future phases (e.g., internet)?
  - Statuses for an outage report: "Active", "Resolved". Any others (e.g., "Investigating", "Scheduled")?

- **Geographic Data**:
  - Source for the map data (administrative boundaries for Costa Rica - provinces, cantons, districts)? Will we need to source and import GeoJSON files, or does the chosen map provider handle this?
  - How should location be captured from users? (GPS coordinates, selecting from a list of locations, typing an address?)

- **Initial Data Seeding**:
  - Should the platform launch with any pre-populated (dummy) outage data for demonstration purposes? [ ] Yes. [ ] No.

- **Moderation & Spam Prevention**:
  - Beyond CAPTCHA and automated duplicate detection, are there specific moderation rules or workflows?
  - Details on the "community upvote/downvote system": How will it be implemented? What threshold defines a validated report?

## 4. Monetization Strategy (MVP & Future)

- **MVP Monetization**:
  - Ad types: Banner ads, sponsored content? Specific dimensions or placements?
  - Potential affiliate partners (e.g., local generator suppliers, water tank vendors)? Do you have existing contacts or should we research?
  - Any restrictions on ad content (e.g., no competitors, family-friendly only)?

- **Premium Features (Future - Phase 3)**:
  - Outage alerts: SMS/Email. Should this be a subscription model (e.g., monthly fee)? What's a reasonable price point?
  - Historical data access: What kind of data? How should it be presented (e.g., downloadable reports, visualizations)?
  - API for businesses: What kind of data would be exposed? Potential pricing model?

## 5. Legal, Privacy & Compliance

- **Privacy Policy & Terms of Service**:
  - Do you have existing templates or legal counsel for this? [ ] Yes (please provide). [ ] No, needs to be drafted.
  - Specific data retention policies for anonymous reports?

- **Data Handling**:
  - The PRD mentions GDPR/CCPA compliance. Given the focus is Costa Rica, are there local data protection laws (like Ley de Protección de la Persona frente al Tratamiento de sus Datos Personales, Ley No. 8968) that need to be prioritized?
  - How will user IP addresses be handled/stored for spam prevention vs. privacy?

- **Liability**:
  - Disclaimer regarding the accuracy of crowdsourced data? How will this be communicated to users?

## 6. Project Management & Logistics

- **Team & Roles**:
  - Confirmation of the development team structure (e.g., 1 frontend, 1 backend, 1 UI/UX designer).
  - Point of contact for approvals and clarifications.

- **Budget Allocation**:
  - The PRD states a $10,000 budget for the first 6 months. A high-level breakdown is needed:
    - Development Costs: $_______
    - Hosting/Infrastructure (6 months): $_______
    - Marketing/SEO (initial 6 months): $_______
    - Contingency (15%): $_______
  - Process for approving expenditures.

- **Timeline & Milestones**:
  - The PRD MVP timeline is 0-3 months. Are there hard deadlines (e.g., a launch event)?
  - Preferred frequency for progress reviews/sprints (e.g., weekly, bi-weekly)?

- **Testing & Deployment**:
  - Process for User Acceptance Testing (UAT). Who will be involved?
  - Deployment strategy (e.g., staging environment, production rollout plan).

## 7. Marketing & Launch

- **Target Audience Refinement**:
  - Any specific regions within Costa Rica to target initially for a soft launch?
  - Key messages for the initial marketing push?

- **SEO Strategy**:
  - Primary keywords beyond "Costa Rica outage map"?
  - Content strategy for the blog/FAQ section (if any) to support SEO?

- **Launch Plan**:
  - Planned activities for the launch (e.g., social media campaign, press release, partnerships with local influencers or communities)?

---

**Next Steps**:

1. Please review this document and provide answers/decisions where indicated.
2. For items requiring further discussion or research, please flag them.
3. Once this information is gathered, it will be used to refine the `OutageCR_roadmap.md` and initiate the development sprints.
