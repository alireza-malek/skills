# Breezy HR ATS Reference Guide

## 1. System Profile & Recruiter Review Interface

- **Core Architecture:** Breezy HR pairs a high-speed human triage workspace (**The Applicants List**) and a three-section Candidate Profile (**Candidate Details**, **Candidate Experience**, and **Hiring Team Actions**) with its credit-based **Breezy Intelligence** AI suite.

- **The "Experience" vs. "Resume" Dual View:** Inside the Candidate Experience section, Breezy separates parsed data from the native file across four dedicated tabs:
  1. **`Experience` Tab:** Displays structured data parsed from the resume and application form—specifically **work history, education, languages, and certifications**.
  2. **`Resume` Tab:** Renders the exact uploaded CV and displays **Resume Audit** results.
  3. **`Documents` Tab:** Stores supplementary attachments such as cover letters, portfolios, and work samples.
  4. **`Questionnaires` Tab:** Stores all candidate responses to screening questionnaires.
- **The Applicants List & Strict "Oldest First" Ordering:** Displays unreviewed applicants in **chronological order (oldest first)** by position. Under `Applicants > My Preferences`, each recruiter sets their personal default view to either **Resume** or **Experience** and can enable one-click triage toggles: **`Quickly accept new applicants`** (advances to next stage in 1 click without a dropdown) and **`Quickly reject new applicants`** (disqualifies in 1 click with a default Disposition Reason).
- **Default Candidate Combining & Email Merging:** **Combining Candidate Data** (`Aggregate candidate data`) is **enabled by default**, linking multiple applications via the **Other Places** dropdown and sharing **Resumes, Attachments, Email, Questionnaires, Previous Position Applications, Team Discussion comments, Candidate Communication, References, and External Recruiters** across all positions. Breezy also recommends duplicate profiles for permanent **Merging** (`Auto Merge` or manual merge) based on **matching email addresses**.

---

## 2. Parsing Engine & Technical File Constraints

- **Candidate Details & Four Parsed Experience Buckets:** Breezy's parser extracts identity metadata into the **Candidate Details** pane (`Name`, `Email`, `Phone`, `Summary`, `Source`, plus recruiter `Tags`) and maps resume content into four specific structured blocks on the **`Experience`** tab: **Work History, Education, Languages, and Certifications** (all editable by Hiring Managers via a pencil icon).
- **Resume vs. Documents Separation:** Cover letters and portfolios must be uploaded to the separate `Documents` / attachments fields. Combining a cover letter and resume into a single file forces reviewers in the `Resume` tab to scroll past a cover letter during high-speed 1-click triage.

---

## 3. Scoring & AI Mechanics: "Breezy Intelligence"

### Applicant Insights & The Overarching `Candidate Score`

- **Applicant Insights (~500 credits/run):** Evaluates candidate qualifications across **four default categories** or **custom categories** (auto-generated from the job description or written as a short `Title` + detailed instructional `Prompt`). Category subscores are saved in the **Applicant Insights** tab and displayed alongside the average criteria score in **Candidate Compare**.
- **Overarching `Candidate Score` Calculation:** Displayed prominently near the candidate's profile picture and in the candidate list, the overall **Candidate Score** is derived directly from **Applicant Insights** and **Candidate Scorecards**. (Note: Post-resume **AI Interview Analysis** grades video interview transcripts of $\ge 200$ words across up to 5 criteria, but explicitly does not contribute to the Candidate Score).
- **Candidate Match Score & Activity Summary (~700 credits/run):** Positions can also utilize **Candidate Match Score** to gauge role alignment and **Activity Summary** to recap profile events in the `Discussion` tab.

### Resume Audit (Originality & Authenticity Verification)

- **One-Click Authenticity Scanning (~600 credits/scan):** Recruiters can click **Run Resume Audit** directly from the top bar of **The Applicants List** during initial triage. Resume Audit scans the uploaded resume for **originality and authenticity indicators** and surfaces audit flags directly inside both the **Resume** tab and the **Applicant Insights** tab.

---

## 4. Search, Rediscovery & Automated Gatekeepers

- **AI Candidate Sourcing (~50,000 credits/position):** When a new role opens, recruiters run **AI Candidate Sourcing** to scan their existing Breezy database and resurface past candidates whose profiles match the new position.
- **Auto-Disqualification & Email Blocking:** Positions can be configured with **auto-disqualifying** questionnaire settings. Admins can also block a candidate by email address (preventing applications to any open position and blocking inbound emails) or trigger a **Candidate Email Verification** / **Application Request** from the profile menu.

---

## 5. Resume Engineering Playbook for Breezy HR

### Dual-View Formatting (`Experience` & `Resume` Tabs)

- **Explicit Headings for the 4 Parsed `Experience` Buckets:** Use clean, standard section headers for `Summary`, `Work Experience` (or `Professional Experience`), `Education`, `Certifications`, and `Languages` so the parser populates every block of the `Experience` tab.
- **Never Bury Languages or Certifications in Prose:** List **Languages** and **Certifications** under dedicated headers with standard titles and issuing bodies rather than hiding them inside work history paragraphs.
- **Visual Scannability for 1-Click `Resume` Triage:** Because recruiters using `Quickly accept` and `Quickly reject` make instant decisions in the `Resume` tab, use crisp typography, bolded job titles, clear `MM/YYYY – MM/YYYY` dates, and front-loaded impact bullets.

### Maximizing `Applicant Insights` & Passing `Resume Audit`

- **Align with the Job Description's Core Pillars:** Because recruiters can auto-generate **Applicant Insights** prompts directly from the job description, address all 4–5 primary requirement themes of the posting with explicit tool names, clear tenure dates, and quantified outcomes to maximize category subscores in **Candidate Compare** and boost your overall **Candidate Score**.
- **Pass `Resume Audit` with Authentic, Specific Context:** Avoid raw, generic LLM boilerplate or copying sentences verbatim from the job description—both trigger originality and authenticity flags during a **Resume Audit**. Anchor every bullet in specific project parameters, architectures, team sizes, and verifiable metrics:
  `Migrated 14 microservices from EC2 to EKS for a FinTech billing platform processing $18M/month, cutting AWS spend by 22%.`

---

## 6. Portal Submission & Form Gatekeepers

- **Apply Early (The "Oldest First" Advantage):** Because **The Applicants List** queues unreviewed applicants **oldest first**, applying promptly places your profile at the very front of the recruiter's triage queue.
- **Upload Cover Letters Separately:** Always attach cover letters or work samples to the separate `Documents` / attachments slot—never merged into your resume file.
- **Maintain Strict Consistency Across Applications:** Because **Combining Candidate Data** is enabled by default and exposes all previous resumes, attachments, and questionnaires via the **Other Places** dropdown, keep all dates, titles, and facts 100% consistent across applications at the same company.

---

## 7. Output Specifications for this ATS

### Target Evaluation Metric Display

**Applicant Insights & Candidate Score:** High Category Subscores Across All JD Pillars (`Candidate Compare` Ready)
**Breezy Resume Audit Status:** Passed Originality & Authenticity Verification (Zero Boilerplate Flags)
**Dual-View Parser Mapping:** 100% Coverage of Work History, Education, Languages & Certifications Tabs

### Remaining Gaps Analysis

Identify any core qualification pillars from the job description that lack explicit evidence for **Applicant Insights** category scoring, any missing **Languages** or **Certifications** that belong in Breezy's structured `Experience` tab, or any generic phrasing that risks flagging **Resume Audit**. Provide targeted clarification prompts to gather authentic, project-specific metrics.
