# Community Upvote/Downvote System Recommendation: ¿Hay Luz? — Costa Rica Outage Tracker

## 1. Overview

The community upvote/downvote system for ¿Hay Luz? (OutageCR) enables users to validate or flag crowdsourced outage reports, enhancing data reliability and user trust. This system leverages community feedback to prioritize accurate reports and filter out potential spam or inaccuracies, while maintaining a simple and intuitive user experience. The implementation is designed to integrate seamlessly with the existing moderation and spam prevention plan, ensuring scalability and minimal maintenance.

## 2. Objectives

- **Improve Data Quality**: Use community feedback to validate or discredit outage reports.
- **Enhance User Engagement**: Encourage users to interact with the platform by contributing to report accuracy.
- **Minimize False Positives**: Ensure legitimate reports are not unfairly downvoted.
- **Automate Moderation**: Reduce manual moderation efforts through community-driven validation.

## 3. Implementation Details

### 3.1 User Interface
- **Upvote/Downvote Buttons**:
  - Each outage report (displayed on the map or list interface) includes two buttons: an upvote (e.g., thumbs-up icon) and a downvote (e.g., thumbs-down icon).
  - Buttons are accessible in both map pop-ups and list view, styled using Tailwind CSS for consistency with the platform’s responsive design.
  - Example: `<button class="text-green-500 hover:text-green-700"><svg thumbs-up-icon /></button>` for upvote.
- **Feedback Counter**:
  - Display a net vote score (upvotes minus downvotes) next to each report.
  - Show a “Community Verified” badge for reports exceeding the validation threshold (see Section 3.3).
- **Accessibility**:
  - Ensure buttons are keyboard-navigable and include ARIA labels (e.g., `aria-label="Upvote this outage report"`).
  - Provide tooltips or hover text (e.g., “Help verify this report”) for clarity.

### 3.2 Voting Mechanics
- **Eligibility**:
  - Anonymous users can vote once per report (tracked via IP hash, GDPR/CCPA-compliant).
  - Logged-in users (via Firebase Authentication) can vote once per report, with votes weighted higher (e.g., 1.5x for trusted users).
- **Rate Limiting**:
  - Limit votes to 10 per hour per IP/user to prevent abuse, implemented via Supabase Edge Functions or Redis.
- **Vote Tracking**:
  - Store votes in the database (e.g., MongoDB or Firestore) as part of the report document:
    ```json
    {
      "report_id": "12345",
      "upvotes": 10,
      "downvotes": 2,
      "net_score": 8,
      "voters": ["ip_hash_1", "user_id_2"]
    }
    ```
  - Prevent duplicate votes by checking voter IDs or IP hashes.

### 3.3 Validation Threshold
- **Validation Criteria**:
  - A report is marked as “Community Verified” if it meets **either** of the following:
    - **Net Score Threshold**: Net score (upvotes minus downvotes) ≥ 10.
    - **Upvote Ratio Threshold**: Upvote ratio (upvotes / total votes) ≥ 0.75 with at least 5 total votes.
  - Example: A report with 12 upvotes and 3 downvotes (net score = 9, ratio = 0.8) is verified if it has ≥ 5 total votes.
- **Flagging Criteria**:
  - Reports with a net score ≤ -5 or an upvote ratio < 0.3 (with ≥ 5 total votes) are flagged for review.
  - Flagged reports are hidden from public view after 24 hours unless manually approved or validated.
- **Dynamic Adjustments**:
  - Adjust thresholds based on platform usage (e.g., increase to net score ≥ 15 in high-traffic scenarios).
  - Use A/B testing to optimize thresholds for user trust and report accuracy.

### 3.4 Backend Logic
- **Database Updates**:
  - Update vote counts in real time using Firebase Realtime Database or MongoDB.
  - Trigger WebSocket updates to reflect vote changes on the frontend instantly.
- **Moderation Integration**:
  - Combine community votes with existing spam prevention (e.g., CAPTCHA, duplicate detection).
  - Automatically reject reports with high downvote ratios and flagged keywords.
- **Scalability**:
  - Use serverless functions (e.g., Supabase or AWS Lambda) to process votes.
  - Cache vote counts in Redis to reduce database load during traffic spikes.

### 3.5 User Feedback
- **Visual Feedback**:
  - Highlight verified reports with a green badge or checkmark icon.
  - Dim or label flagged reports (e.g., “Under Review”) in the UI.
- **Notifications**:
  - Notify report submitters (if logged-in) when their report is verified or flagged.
  - Example: Browser push notification saying, “Your outage report in San José was verified by the community!”

## 4. Technical Requirements
- **Frontend**:
  - React.js with Tailwind CSS for responsive button and badge styling.
  - WebSocket or Firebase Realtime Database for real-time vote updates.
- **Backend**:
  - Node.js with Express or Supabase for vote processing and rate limiting.
  - MongoDB/Firestore for storing vote data with geospatial report data.
  - Redis for caching vote counts and tracking voter IDs.
- **Security**:
  - Hash IP addresses for anonymous vote tracking (SHA-256, GDPR-compliant).
  - Implement rate limiting to prevent vote spamming.
  - Secure WebSocket connections with HTTPS.
- **Performance**:
  - Process vote updates in <100ms for seamless user experience.
  - Optimize database queries with indexing for high-traffic scenarios.

## 5. Success Metrics
| **Metric** | **Target** |
|------------|------------|
| **Verification Rate** | 80% of legitimate reports achieve “Community Verified” status within 12 hours. |
| **False Flagging Rate** | <5% of legitimate reports flagged as spam. |
| **User Participation** | 10% of monthly active users engage with the voting system. |
| **Moderation Efficiency** | Reduce manual moderation by 50% through community validation. |

## 6. Risks and Mitigations
| **Risk** | **Mitigation** |
|----------|---------------|
| **Vote Manipulation** | Enforce rate limits and weight logged-in user votes higher. Monitor for unusual voting patterns. |
| **Low Participation** | Promote voting with UI prompts and gamification (e.g., “Trusted Contributor” badges). |
| **False Downvoting** | Require minimum total votes (≥5) for flagging and monitor downvote ratios. |
| **Performance Bottlenecks** | Use caching and serverless architecture to handle vote surges. |

## 7. Integration with Existing Systems
- **Moderation Plan**:
  - Combine with CAPTCHA, duplicate detection, and rate limiting from the existing spam prevention plan.
  - Use community votes as an additional signal for automated moderation (e.g., low vote scores trigger keyword checks).
- **Outage Visualization**:
  - Prioritize “Community Verified” reports in map and list views (e.g., higher z-index or bolded entries).
  - Filter out flagged reports from public views unless validated.

## 8. Roadmap
| **Phase** | **Timeline** | **Deliverables** |
|-----------|--------------|------------------|
| **Phase 1: Core Voting** | 0–3 months | Upvote/downvote buttons, net score display, basic validation thresholds. |
| **Phase 2: Enhancements** | 4–6 months | Community Verified badges, user notifications, weighted votes for trusted users. |
| **Phase 3: Optimization** | 7–12 months | Dynamic threshold adjustments, gamification for voting, integration with ML-based moderation.