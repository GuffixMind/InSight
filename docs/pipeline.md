# 🛠 Full Pipeline – Phishing Awareness Kit

---

## **Phase 1 – Company Onboarding**

**Goal:** Get company + employees into the system.

1. **Company Owner Signs Up**

   * Registers account → `users` table.
   * Provides company name, domain.

2. **Employee Data Ingestion**
   **Two options at MVP:**

   * **CSV Upload**

     * Owner uploads `.csv` → triggers **multi-agent validation pipeline**.
     * Pipeline checks → File Agent → Format Agent → Duplicate Agent → Enrichment Agent → Approval Agent.
     * Owner reviews results → approves → valid rows saved into `employees` table.
   * **Manual Add**

     * Owner enters single employee (name, email, dept) via frontend form.
     * Backend validates (same pipeline but row-by-row).

3. **Data Storage**

   * Clean employee records stored in PostgreSQL under `employees` table, linked to `user_id`.

---

## **Phase 2 – Campaign Creation**

**Goal:** Let company owner test employees.

1. **Owner Creates Campaign**

   * Chooses **phishing type**: Email, Spear, Clone.
   * Selects template from library OR customizes one.
   * Sets campaign parameters → target employees (all or specific department), schedule, tracking options.

2. **Backend Prepares Campaign**

   * Insert into `campaigns` table.
   * For each employee → generate **unique tracking tokens** (for links, buttons, fake login).
   * Save tracking tokens in `events` table with pending status.

---

## **Phase 3 – Email Simulation Delivery**

**Goal:** Deliver realistic phishing simulations.

1. **Email Sending**

   * Use AWS SES / SendGrid / Mailgun to send templated emails.
   * Example:

     * Subject: *“Microsoft Password Reset Required”*
     * Button → `https://phishkit.com/track/click/{token}`

2. **Employee Interaction**

   * Opens email → triggers `/track/open/{token}`.
   * Clicks link → `/track/click/{token}`.
   * Submits fake login form → `/track/submit/{token}`.
   * Reports phishing (if “Report” button installed) → `/track/report/{token}`.

3. **Event Logging**

   * Each interaction recorded in `events` table:

     * `event_type` = open, click, submit, report.
     * `timestamp`.

---

## **Phase 4 – Training Reinforcement**

**Goal:** Educate employees who fail.

1. **If Employee Clicks / Submits**

   * Redirected to **training page** (short video or checklist).
   * Shows “You clicked a simulated phishing link – here’s how to spot this in the future.”

2. **If Employee Reports**

   * Redirected to “✅ Well done! You spotted the phishing attempt.”
   * Award badge/score (optional gamification).

---

## **Phase 5 – Reporting & Analytics**

**Goal:** Give company owner clear visibility.

1. **Real-Time Dashboard** (Frontend React)

   * Campaign stats:

     * Emails sent.
     * Open rate.
     * Click rate.
     * Submission rate.
     * Reported phishing rate.
   * Department breakdown.
   * List of high-risk employees.

2. **Monthly Report Generation (Backend Python)**

   * Aggregate data per company.
   * Store report summary in `reports` table (JSONB for flexibility).
   * Generate **PDF** with:

     * Risk Score (e.g., 72/100).
     * Metrics (open %, click %, submit %).
     * Department-wise breakdown.
     * Trend graph (month-over-month).
     * Recommendations.

3. **Report Delivery**

   * Owner can:

     * Download PDF from dashboard.
     * Receive email with attached summary.

---

## **Phase 6 – Continuous Improvement**

**Goal:** Make system smarter & scalable.

* **Add Integrations** (later): Google Workspace, Microsoft 365, HR APIs → auto-sync employees.
* **Expand Template Library** → more phishing scenarios.
* **Gamify Training** → leaderboards, badges, rewards.
* **AI-powered Templates** → generate new phishing emails based on real-world trends.

---

# 🔹 Example End-to-End Workflow (Company Owner POV)

1. Sign up → Add employees via CSV upload.
2. CSV processed by **multi-agent pipeline** → valid employees stored.
3. Create campaign → choose phishing type + template → launch.
4. Employees receive phishing emails → system tracks interactions.
5. Employees who fail → redirected to training page.
6. Owner opens dashboard → sees real-time results.
7. End of month → owner downloads full PDF report.
