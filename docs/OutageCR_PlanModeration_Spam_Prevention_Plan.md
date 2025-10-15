# Moderation & Spam Prevention Plan: ¿Hay Luz? — Costa Rica Outage Tracker

## 1. Overview

The moderation and spam prevention plan for ¿Hay Luz? (OutageCR) ensures the platform maintains high-quality, reliable crowdsourced outage data while protecting against spam, bots, and malicious activity. This multi-layered approach balances user accessibility with data integrity, fostering trust in the platform. The plan is designed to scale with user growth and adapt to emerging threats.

## 2. Objectives

- **Data Accuracy**: Ensure outage reports are legitimate and reflect real-world conditions.
- **User Trust**: Minimize false or misleading reports to maintain platform credibility.
- **Scalability**: Implement efficient, automated systems to handle increasing report volumes.
- **Accessibility**: Allow anonymous submissions while preventing abuse.

## 3. Multi-Layered Protection Strategy

The following table outlines the core components of the moderation and spam prevention system:

| **Layer** | **Description** | **Implementation Details** |
|-----------|-----------------|----------------------------|
| **CAPTCHA on Submission** | Prevents automated bots from submitting reports. | Integrate hCaptcha or Google reCAPTCHA v3 (preferred for seamless UX) on the outage report form. Configure to trigger on all submissions, with adjustable sensitivity for suspicious behavior. |
| **Duplicate Detection** | Blocks redundant or repetitive submissions from the same area/time. | Use geospatial and temporal analysis to flag duplicates (e.g., reports within 500m radius and same outage type within 15 minutes). Store reports in a database (e.g., MongoDB or Firestore) with geospatial indexing for efficient comparison. |
| **Rate Limiting** | Limits excessive submissions from a single user or IP. | Implement server-side rate limits using Supabase Edge Functions or an API Gateway (e.g., AWS API Gateway). Example: max 5 reports per hour per IP for anonymous users, 10 for logged-in users. Track via Redis or similar for high performance. |
| **Auth-Level Validation** | Encourages trusted submissions via optional user authentication. | Allow anonymous reporting with stricter validation (e.g., CAPTCHA + rate limits). Logged-in users (via Firebase Authentication or OAuth) receive higher submission quotas and weighted report credibility. |
| **Community Validation** | Leverages user feedback to validate or flag reports. | Introduce an upvote/downvote system for reports. Reports with high downvotes (e.g., >5) are flagged for review. Auto-resolve low-confidence reports after 24 hours unless validated. |
| **Automated Content Moderation** | Filters out inappropriate or irrelevant content. | Use keyword-based filtering to detect offensive or irrelevant text in report descriptions. Optionally integrate a lightweight ML model (e.g., hosted on AWS SageMaker) to score report legitimacy based on patterns. |

## 4. Implementation Details

### 4.1 CAPTCHA Integration
- **Tool**: hCaptcha or Google reCAPTCHA v3.
- **Process**:
  - Display CAPTCHA on the outage report form for all anonymous submissions.
  - For reCAPTCHA v3, use score-based verification (e.g., score < 0.5 triggers manual challenge).
  - Store CAPTCHA verification status in the database alongside each report.
- **Metrics**: Monitor CAPTCHA failure rates to adjust sensitivity and prevent user friction.

### 4.2 Duplicate Detection
- **Algorithm**:
  - Compare new report’s coordinates (latitude/longitude) with existing reports using a geospatial query.
  - Flag reports as duplicates if they fall within a 500m radius and same outage type (electricity/water) within 15 minutes.
- **Database**:
  - Use MongoDB with 2dsphere indexing or Firestore geospatial queries for efficient lookup.
  - Cache recent reports in memory (e.g., Redis) to reduce database load.
- **Action**:
  - Merge duplicates into a single report with an aggregated “report count” to reflect intensity.
  - Notify users if their report is flagged as a duplicate.

### 4.3 Rate Limiting
- **Configuration**:
  - Anonymous users: 5 reports/hour/IP.
  - Logged-in users: 10 reports/hour/user.
  - Global limit: 100 reports/hour across all users to prevent system-wide flooding.
- **Tools**:
  - Supabase Edge Functions for serverless rate limiting.
  - Redis for tracking request counts with a sliding window algorithm.
- **Fallback**:
  - If rate limits are exceeded, queue submissions for delayed processing or prompt user login.

### 4.4 Auth-Level Validation
- **Anonymous Users**:
  - Require CAPTCHA and stricter rate limits.
  - Reports weighted lower in visibility (e.g., 0.5x in map/list ranking).
- **Logged-In Users**:
  - Authenticate via Firebase (Google, email, or anonymous login).
  - Higher submission quotas and report visibility (1.0x weight).
  - Track user report history to assign trust scores (e.g., based on report validation rate).
- **Future**:
  - Introduce “trusted user” status for frequent, accurate reporters with elevated privileges.

### 4.5 Community Validation
- **Mechanism**:
  - Allow users to upvote/downvote reports for accuracy.
  - Reports with >5 downvotes or low upvote ratio (<0.3) are flagged for manual review or auto-hidden.
- **Display**:
  - Show validation status on reports (e.g., “Community Verified” for >10 upvotes).
- **Moderation**:
  - Use automated rules to hide low-confidence reports after 24 hours unless validated.

### 4.6 Automated Content Moderation
- **Keyword Filtering**:
  - Maintain a blocklist of offensive or irrelevant terms (e.g., profanity, unrelated keywords).
  - Flag reports containing blocklisted terms for manual review.
- **ML-Based Scoring** (Optional):
  - Train a lightweight model to score report legitimacy based on text, location, and user history.
  - Deploy on a scalable platform like AWS SageMaker or Google Cloud AI.
- **Action**:
  - Automatically reject reports with high spam scores (>0.9) or queue for review.

## 5. Technical Requirements
- **Frontend**:
  - React.js for dynamic CAPTCHA rendering and user feedback (upvote/downvote).
  - Real-time updates via WebSocket or Firebase Realtime Database for report status.
- **Backend**:
  - Node.js with Express or Supabase for API and rate-limiting logic.
  - MongoDB/Firestore for report storage with geospatial indexing.
  - Redis for rate limiting and caching.
- **Security**:
  - HTTPS for all data transfers.
  - IP hashing for anonymous rate limiting (GDPR/CCPA compliant).
  - Regular audits of CAPTCHA and moderation effectiveness.
- **Scalability**:
  - Serverless architecture (e.g., Supabase or AWS Lambda) to handle traffic spikes.
  - CDN (e.g., Cloudflare) for low-latency CAPTCHA and asset delivery.

## 6. Success Metrics
| **Metric** | **Target** |
|------------|------------|
| **Spam Rejection Rate** | 95% of automated spam (bots, duplicates) blocked. |
| **False Positive Rate** | <5% of legitimate reports flagged as spam. |
| **Moderation Latency** | Flagged reports reviewed or auto-resolved within 12 hours. |
| **User Trust** | >80% of users trust platform data (measured via optional surveys). |

## 7. Risks and Mitigations
| **Risk** | **Mitigation** |
|----------|---------------|
| **CAPTCHA Friction** | Use reCAPTCHA v3 for invisible verification; offer fallback to v2 for accessibility. |
| **Sophisticated Spam** | Regularly update blocklists and ML models to detect evolving spam patterns. |
| **Over-Moderation** | Monitor false positive rates and adjust thresholds to avoid blocking legitimate reports. |
| **Scalability Issues** | Use serverless infrastructure and caching to handle high report volumes. |

## 8. Roadmap
| **Phase** | **Timeline** | **Deliverables** |
|-----------|--------------|------------------|
| **Phase 1: Core Moderation** | 0–3 months | CAPTCHA, duplicate detection, rate limiting, basic keyword filtering. |
| **Phase 2: Community Validation** | 4–6 months | Upvote/downvote system, trusted user tiers, enhanced duplicate detection. |
| **Phase 3: Advanced Moderation** | 7–12 months | ML-based spam detection, automated report scoring, API for external moderation tools. |

## 9. Stakeholders
- **Product Owner**: Oversees moderation strategy and user trust metrics.
- **Development Team**: Implements CAPTCHA, rate limiting, and backend logic.
- **Community Managers**: Monitor flagged reports and handle manual reviews.
- **Users**: Benefit from accurate, spam-free outage data.