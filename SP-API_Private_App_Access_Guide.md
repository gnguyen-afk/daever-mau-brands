# SP-API Private App Access — Client Onboarding Guide

**Model:** Brand owns and self-authorizes the private app (private apps can't be shared across companies). Developer (DAEVER) drafts text and receives final credentials.

---

## 1. Confirm Professional Account
**Who: Brand**
- Confirm Professional Selling Plan (not Individual)
- Confirm logging in as **primary account user** (not invited/sub-user)
- Confirm active **Brand Registry** enrollment (required later for Brand Analytics)
- ⏱ No wait — instant check in Seller Central → Account Info

**Prepare beforehand:** Brand Registry number, business license/trademark docs on hand in case Amazon asks for verification.

---

## 2. Register Developer Profile
**Who: Brand**, use-case text drafted by **Developer**
- Seller Central → Apps and Services → Develop Apps → Proceed to Developer Profile
- Choose **Private Developer**
- Fill Contact Info + Use Case (≤500 words) + Security Control
- Complete **Identity Verification**: ID document, proof of address, phone, live video call
- Accept AUP / DPP / Solution Provider Portal Agreement → Submit
- 🔗 developer-docs.amazon.com/sp-api/docs/register-as-a-private-developer

**⏱ Wait time:** Identity verification ~20 min if slots available, but scheduling the video call can add **2–5 days**. Profile review by Amazon: typically a few business days; case closes if no response within 5 days.

**Prepare beforehand:** Government ID, proof of business address, phone ready for verification call, and the use-case text from Developer.

---

## 3. Register Private App with Common Roles
**Who: Brand**, role list supplied by **Developer**
- Solution Provider Portal (or Seller Central Develop Apps) → Add new app client
- Select common roles: Selling Partner Insights, Inventory and Order Tracking, Amazon Fulfillment, Pricing, Product Listing
- Generates **LWA Client ID / Client Secret** (no IAM ARN — deprecated Oct 2023)
- 🔗 developer-docs.amazon.com/sp-api/docs/registering-your-application

**⏱ Wait time:** Instant once developer profile is approved.

**Prepare beforehand:** Exact role list from Developer (don't let Brand guess roles).

---

## 4. Request Brand Analytics Role
**Who: Brand**, justification drafted by **Developer**
- Brand Analytics is a **restricted role** — separate approval, on top of common roles
- Requires active Brand Registry enrollment to qualify
- Add to Developer Profile / app role request with a clear use-case justification (why you need search terms / market basket / review data)
- 🔗 developer-docs.amazon.com/sp-api/docs/brand-analytics-role
- 🔗 How to request a role: developer-docs.amazon.com/sp-api/docs/how-do-i-request-and-qualify-for-a-role

**⏱ Wait time:** Longer than common roles — restricted roles go through additional Amazon review, commonly **1–2+ weeks**. Plan client timelines around this, not the common-role approval.

**Prepare beforehand:** Written justification tying the role to Brand's own data use (Developer should draft this in advance to avoid back-and-forth delays).

---

## 5. Verify SP-API Access
**Who: Brand self-authorizes → Developer verifies**
- Brand: Develop Apps → Edit App → **Authorize** → click **Authorize app** → refresh token generated (save immediately, shown once)
- Brand shares Client ID, Client Secret, Refresh Token with Developer via secure channel
- Developer: exchange refresh token for access token, make a test call (e.g. getMarketplaceParticipations) to confirm roles are active, including a Brand Analytics report request to confirm that role specifically
- 🔗 developer-docs.amazon.com/sp-api/docs/self-authorization

**⏱ Wait time:** Instant for self-authorization; allow a short buffer after Brand Analytics approval before the role is fully active on calls.

**Prepare beforehand:** Secure credential-sharing channel agreed with Brand in advance (not email/chat).

---

## Summary Timeline
| Step | Typical Wait |
|---|---|
| 1. Confirm account | Instant |
| 2. Developer profile | 2–5+ days (verification + review) |
| 3. Register app, common roles | Instant after step 2 |
| 4. Brand Analytics role | 1–2+ weeks (separate restricted review) |
| 5. Self-authorize & verify | Instant, plus short activation buffer |

**Total realistic lead time to full access: ~2–3 weeks**, driven mainly by Brand Analytics approval — start that request as early as possible, in parallel with app registration.
