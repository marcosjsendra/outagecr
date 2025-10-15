# Human Requirements for OutageCR Development

This document outlines the information, decisions, and inputs required from the product owner/stakeholder to proceed with the development of the ¿Hay Luz? (OutageCR) platform.

## 1. Technical Stack & Infrastructure Confirmation

The PRD suggests a specific technical stack. Please confirm or provide alternatives for the following:

- **Frontend Framework**: React.js with Tailwind CSS.
  - Confirmation: [x] Yes, proceed as suggested. [ ] No, propose alternative: _________________________
  - Rationale for alternative (if any):

- **Backend & Database**: Supabase (PostgreSQL database, API, Auth).
  - Confirmation: [x] Yes, proceed with Supabase. [ ] No, propose alternative: _________________________
  - Rationale for choice: Supabase provides a scalable backend with a generous free tier, including a PostgreSQL database, instant APIs, and authentication, which aligns with our goal for a lightweight, fast, and cost-effective MVP.

- **Map Provider**: OpenStreetMap or Leaflet.
  - Confirmation: [x] Yes, proceed as suggested **Open Street Map**. [ ] No, propose alternative (e.g., Google Maps API, Mapbox): _________________________
  - Rationale for alternative (if any):

- **Hosting Provider (Frontend)**: Sevalla (Works with Github) (Static Site Hosting).
  - Confirmation: [x] Yes, proceed with Sevalla. [ ] No, propose alternative: _________________________
  - Rationale for choice: Sevalla offers a generous free tier for static sites with a global CDN, auto-deploys from Git, and scalability, which is ideal for our React frontend and aligns with our cost-effective MVP strategy.
  - **Marcos Sendra: [Sevalla](https://sevalla.com/)**

- **Authentication Service**: Supabase Authentication (for anonymous and registered users).
  - Confirmation: [x] Yes, proceed with Supabase Auth. [ ] No, propose alternative: _________________________
  - Rationale for choice: Since we are using Supabase for our backend and database, using its built-in authentication service simplifies the architecture, ensures seamless integration, and leverages its generous free tier for user management.

## 2. Design & User Experience (UX)

- **Brand Identity**:
  - Do you have existing brand guidelines (logos, color schemes, fonts)? [x] Yes (please provide assets/links).
  - Preferred color palette (especially for map markers - e.g., red for outages, green for resolved):
  - Any specific UI/UX preferences or examples of similar platforms you like/dislike
  - **Marcos Sendra: I've created a Markdown file including all the brand identity assets here [Brand Guidelines](..\docs\OutageCR_brandguidelines.md)**

- **Map Interface Specifics**:
  - Default view level for the map (e.g., country overview, specific province)? **Specific Province**
  - Level of detail for administrative boundaries (Province, Canton, District)? The PRD mentions all three; is this granularity required for MVP? **Let's start with Cantones which there are 82**
  - Specific icons or marker styles desired for different outage types (Electricity, Water)? Yes, **Marcos Sendra: I've created a Markdown file including the icons for specific markers here: [Brand Guidelines](..\docs\OutageCR_brandguidelines.md) @### Status Icon with Colors**

- **Reporting Form Details**:
  - Mandatory vs. Optional fields in the outage report form (Location, Type, Description, Photo)? **Location, Type and Description**
  - Consent mechanism for geolocation (explicit button, or on form load with clear disclaimer)? **explicit button**
  - CAPTCHA preference (e.g., Google reCAPTCHA, hCaptcha, a simple math question)? **Google reCAPTCHA - Simple Math Question**

## 3. Data & Content

- **Outage Types**:
  - The PRD mentions "electricity" and "water". Are there any other utility types to consider for MVP or future phases (e.g., internet)? **Yes, internet**
  - Statuses for an outage report: "Active", "Resolved". Any others (e.g., "Investigating", "Scheduled")? **Active Outage, Reported, Resolved, Planned Maintenance, Scheduled outages, Critical / Large Area**

- **Geographic Data**:
  - Source for the map data (administrative boundaries for Costa Rica - provinces, cantons, districts)? Will we need to source and import GeoJSON files, or does the chosen map provider handle this? **1. Yes — OpenStreetMap does have boundary data for provinces, cantons, districts in Costa Rica. 2. We’ll likely need to source GeoJSON or TopoJSON versions yourself (via GitHub repos or services like Overpass, GeoBoundaries). 3. For performance, we’ll want to simplify shapes and load only what’s needed (e.g. only district boundaries of a given province when zoomed in).**
  - How should location be captured from users? (GPS coordinates, selecting from a list of locations, typing an address?) **GPS coordinates**

- **Initial Data Seeding**:
  - Should the platform launch with any pre-populated (dummy) outage data for demonstration purposes? [x] Yes. [ ] No.

- **Moderation & Spam Prevention**:
  - Beyond CAPTCHA and automated duplicate detection, are there specific moderation rules or workflows? **Marcos Sendra: Analyze the following: [Moderation & Spam Prevention Plan](..\docs\OutageCR_PlanModeration_Spam_Prevention_Plan.md)**
  - Details on the "community upvote/downvote system": How will it be implemented? What threshold defines a validated report? **Marcos Sendra: Analyze the following: [Up Vote and Down Vote System](..\docs\OutageCR_UpVote-DownVote.md)**

## 4. Monetization Strategy (MVP & Future)

- **MVP Monetization**:
  - Ad types: Banner ads, sponsored content? Specific dimensions or placements? **Mobile: Bottom banner (320×50 responsive). Desktop: Footer banner (728×90). Google ADs or Other similar type of ADs would be Revisted after getting some tracktion on our website, so no ads for now**
  - Potential affiliate partners (e.g., local generator suppliers, water tank vendors)? Do you have existing contacts or should we research? **Yeah we can implement potential affiliate partners, I do not have any existing contact, I could contact them later if this app increases in usage and users, as a leverage.**
  - Any restrictions on ad content (e.g., no competitors, family-friendly only)? **Yes, no competitors, family-friendly only - Google ADs or Other similar type of ADs would be Revisted after getting some tracktion on our website**

- **Premium Features (Future - Phase 3)**:
  - Outage alerts: SMS/Email. Should this be a subscription model (e.g., monthly fee)? What's a reasonable price point? **We should start free with email only, Later implement SMS with a subcription model - price point: $2 Mounthly**
  - Historical data access: What kind of data? How should it be presented (e.g., downloadable reports, visualizations)? **Downloable Reports**
  - API for businesses: What kind of data would be exposed? Potential pricing model? **Marcos Sendra: Refer to this: [API and Monetization Strategy](..\docs\OutageCR_UpVote-DownVote.md)**

## 5. Legal, Privacy & Compliance

- **Privacy Policy & Terms of Service**:
  - Do you have existing templates or legal counsel for this? [ ] Yes (please provide). [x] No, needs to be drafted.
  - Specific data retention policies for anonymous reports? **We retain anonymous outage reports for up to 12 months to improve data quality and analytics. After this period, reports are anonymized further or deleted.**

- **Data Handling**:
  - The PRD mentions GDPR/CCPA compliance. Given the focus is Costa Rica, are there local data protection laws (like Ley de Protección de la Persona frente al Tratamiento de sus Datos Personales, Ley No. 8968) that need to be prioritized? **Yes, we would need to prompt users of this requirement; they should agree to it if they would like to use this app/page**
  - How will user IP addresses be handled/stored for spam prevention vs. privacy? **handled/stored for spam prevention**

- **Liability**:
  - Disclaimer regarding the accuracy of crowdsourced data? How will this be communicated to users? **Disclaimer on Data Accuracy: OutageCR relies on user-submitted reports and publicly available information. While we strive to maintain accurate and up-to-date data, we cannot guarantee the completeness, reliability, or accuracy of all outage information displayed. Users are encouraged to verify information directly with their service providers.**

## 6. Project Management & Logistics

- **Team & Roles**:
  - Confirmation of the development team structure (e.g., 1 frontend, 1 backend, 1 UI/UX designer). **Marcos Sendra: The team is: Marcos Sendra (Designer UI/UX and Frontend developer) and VSCode Cline with GLM-4.6 for everything else.**
  - Point of contact for approvals and clarifications. **Marcos Sendra**

- **Budget Allocation**:
  - The PRD states a $10,000 budget for the first 6 months. A high-level breakdown is needed:
    - Development Costs: $_______
    - Hosting/Infrastructure (6 months): $_______
    - Marketing/SEO (initial 6 months): $_______
    - Contingency (15%): $_______
  - Process for approving expenditures.
  - **Marcos Sendra: This app will be done using by me (Marcos Sendra) and Cline with GLM-4.6. No budget Allocations would be neede.**

- **Timeline & Milestones**:
  - The PRD MVP timeline is 0-3 months. Are there hard deadlines (e.g., a launch event)? **As soon as possible**
  - Preferred frequency for progress reviews/sprints (e.g., weekly, bi-weekly)? **For Reviews we will use [CodeRabbit](https://www.coderabbit.ai/)**
- **Testing & Deployment**:
  - Process for User Acceptance Testing (UAT). Who will be involved? **Me (Marcos Sendra) Solo Developer**
  - Deployment strategy (e.g., staging environment, production rollout plan). **production rollout plan**

## 7. Marketing & Launch

- **Target Audience Refinement**:
  - Any specific regions within Costa Rica to target initially for a soft launch? **Guanacaste Province**
  - Key messages for the initial marketing push? **Report and View Outages of Costa Rica**

- **SEO Strategy**:
  - Primary keywords beyond "Costa Rica outage map"? **Costa Rica outage map**
  - Content strategy for the blog/FAQ section (if any) to support SEO? **OutageCR will include a FAQ section focused on utility reliability, outage prevention, and local service updates to build SEO authority and user trust.**

- **Launch Plan**:
  - Planned activities for the launch (e.g., social media campaign, press release, partnerships with local influencers or communities)? **social media campaign**

---

**Next Steps**:

1. Please review this document and provide answers/decisions where indicated. **Done**
2. For items requiring further discussion or research, please flag them.
3. Once this information is gathered, it will be used to refine the `OutageCR_roadmap.md` and initiate the development sprints.
