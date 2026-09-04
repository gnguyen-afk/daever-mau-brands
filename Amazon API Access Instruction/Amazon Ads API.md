# Amazon Ads API — Client Onboarding Guide (Detailed)

**Goal:** DAEVER (gnguyen@daever.com) obtains a refresh_token to call the Ads API on behalf of the Brand's advertiser account.

**Total estimated timeline: ~1–2 business days** (driven almost entirely by Amazon's application review in Step 3, officially up to 1 business day).

---

## Step 1: Brand invites DAEVER as an Ads account user

**Who:** Brand

**Requirements before starting:**
- Brand has an active Amazon Advertising account (Sponsored Products/Brands/Display)
- Person doing the invite has Account Owner or Admin role on the Ads account

**Sub-steps:**
1. Log into Amazon Advertising Console
2. Go to **Account Settings → Users**
3. Click **Invite a user**
4. Enter `gnguyen@daever.com`
5. Set role to **Edit** (Admin-level access — "View only" won't work for later steps)
6. Send invite

**Output:** Invitation sent, pending acceptance

**Waiting/approval estimate:** None — instant. Just waiting on Developer to accept (Step 2 sub-step 1).

---

## Step 2: Developer accepts invite + creates the LWA app (Security Profile)

**Who:** Developer

**Requirements before starting:**
- Step 1 completed (invite received)
- Access to `gnguyen@daever.com` inbox

**Sub-steps:**
1. Open invite email, accept it (confirms account access)
2. Log into Amazon (as `gnguyen@daever.com`) → confirm Brand's account now appears under managed accounts
3. Go to **developer.amazon.com/loginwithamazon**
4. Log in as `gnguyen@daever.com`
5. Click **Create a New Security Profile** — this is a free, no-approval-needed "client application"
6. Name it (e.g. "DAEVER Ads Integration")
7. Open **Web Settings** tab → copy **Client ID** and **Client Secret** — store securely
8. Under **Allowed Return URLs**, add redirect URI (e.g. `https://localhost` or DAEVER's real callback endpoint)
9. Save

**Output:** Client ID + Client Secret + registered redirect URI (the "client application" now exists — no approval required for this step)

**Waiting/approval estimate:** None — instant, self-service.

---

## Step 3: Developer applies for permission to access the API

**Who:** Developer

**Requirements before starting:**
- Step 2 completed (client application exists)

**Sub-steps:**
1. Go to **advertising.amazon.com/about-api**
2. Select **Direct advertiser** option
3. Fill out the application form — includes questions on how DAEVER intends to use the Ads API, plus acknowledgment of the **Amazon Ads API License Agreement** and **Data Protection Policy**
4. Submit while logged in as `gnguyen@daever.com`

**Output:** Application submitted to Amazon, pending review

**Waiting/approval estimate:** Amazon states **up to 1 business day**. If no response after that, email **ads-api-onboarding@amazon.com**.

---

## Step 4: Developer assigns API access (scope) to the app

**Who:** Developer

**Requirements before starting:**
- Step 3 approved
- Must be logged in as the same account that submitted Step 3

**Sub-steps:**
1. Go to **advertising.amazon.com/API/docs/en-us/guides/onboarding/assign-api-access**
2. Confirm logged in as `gnguyen@daever.com`
3. Follow on-page flow to assign `advertising::campaign_management` scope to the Client ID from Step 2
4. Verify: check developer dashboard shows the scope listed against the Client ID

**Output:** Client ID is now scope-enabled for Ads API calls

**Waiting/approval estimate:** None — instant, but occasionally requires a page refresh/re-login to reflect. If it fails with "unknown scope requested," it means Step 3 approval hasn't landed yet, or you're logged in as the wrong account.

---

## Step 5: Developer authorizes the app against Brand's account

**Who:** Developer

**Requirements before starting:**
- Step 4 completed
- Step 1 invite still active (account access confirmed in Step 2)

**Sub-steps:**
1. Build authorize URL:
   ```
   https://www.amazon.com/ap/oa?client_id=YOUR_CLIENT_ID&scope=advertising::campaign_management&response_type=code&redirect_uri=YOUR_REDIRECT_URI
   ```
2. Open it, log in as `gnguyen@daever.com`
3. Amazon shows Brand's advertiser account as an authorizable option (because of Step 1's invite)
4. Click **Allow**
5. Browser redirects to `YOUR_REDIRECT_URI/?code=...` — copy the `code` value

**Output:** One-time authorization `code`

**Waiting/approval estimate:** None — instant. Code expires quickly (minutes), so proceed to Step 6 right away.

---

## Step 6: Developer exchanges code for refresh_token

**Who:** Developer

**Requirements before starting:**
- Step 5 completed, `code` in hand (not expired)

**Sub-steps:**
1. `POST https://api.amazon.com/auth/o2/token`
   - `grant_type=authorization_code`
   - `code=<from Step 5>`
   - `client_id`, `client_secret` (from Step 3)
   - `redirect_uri` (must match Step 3 exactly)
2. Response returns `access_token`, `refresh_token`, `expires_in`
3. Store `refresh_token` securely per client (e.g. Secrets Manager) — this does not expire unless revoked

**Output:** Long-lived `refresh_token` for this Brand

**Waiting/approval estimate:** None — instant.

---

## Step 7: Verify access

**Who:** Developer

**Requirements before starting:**
- Step 6 completed

**Sub-steps:**
1. Use `refresh_token` to mint a fresh `access_token`
2. Call `GET /v2/profiles`
3. Confirm Brand's advertiser profile(s) appear — one per marketplace/region (NA/EU/FE)

**Output:** Confirmed working integration; profile_id(s) captured for pipeline config

**Waiting/approval estimate:** None — instant.

---

## Summary table

| # | Step | Who | Waiting/Approval |
|---|------|-----|-------------------|
| 1 | Invite DAEVER to Ads account | Brand | Instant |
| 2 | Accept invite + create LWA app | Developer | Instant (no approval needed) |
| 3 | Apply for API access | Developer | **Up to 1 business day** |
| 4 | Assign API scope | Developer | Instant |
| 5 | Authorize app | Developer | Instant |
| 6 | Exchange code for refresh_token | Developer | Instant |
| 7 | Verify access | Developer | Instant |

**Brand's total involvement: Step 1 only (~5 minutes).**
**Bottleneck: Step 3's Amazon approval (up to 1 business day) — everything else is same-day.**

---

## Known risks / notes

- If Step 4 fails with "unknown scope requested" → either Step 3 approval hasn't arrived yet, or wrong account logged in.
- If Edit-level invite (Step 1) isn't enough to complete Steps 3/4, Brand may need to re-invite as full Admin — undocumented by Amazon which invited-role tiers can complete these vs. only the original owner.
- This whole flow (self-registering per client) is a stopgap. Once DAEVER's own Partner/third-party application is approved (pending business registration), Step 1 disappears for all future clients — they'll just click one authorize link, no account access granted to DAEVER at all.

**Doc references:**
- advertising.amazon.com/about-api
- developer.amazon.com/loginwithamazon
- advertising.amazon.com/API/docs/en-us/guides/onboarding/assign-api-access
- Support: ads-api-onboarding@amazon.com
