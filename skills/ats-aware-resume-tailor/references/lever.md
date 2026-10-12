# Lever ATS Reference Guide

## 1. System Profile & Recruiter Review Interface

- **Core Architecture ("Profile vs. Opportunity" Model):** Lever separates candidate data into two tiers: a single global **Candidate Profile** (storing universal contact details, location, parsed work/education history, and the central `Resume and Files` repository) and job-specific **Opportunities** (created for each job application, holding the posting title, application resume, custom form answers, pipeline stage [`New Lead`, `New Applicant`, `Interview`], origin, and source).
- **Email & Nickname Deduplication:** Applying to multiple roles with the same email automatically links each new Opportunity under your existing Candidate Profile (indicated by a **suitcase icon** and an upper-right opportunity selector). If a different email is used, Lever detects duplicates via matching emails or nickname-aware name matching (e.g., `"Edward"` and `"Eddie"`) and prompts recruiters with a `"Merge them?"` banner.
- **Fast Resume Review (Strict FIFO Oldest-to-Newest):** Recruiters triage `New Applicant` opportunities in **Fast Resume Review**, which sequences applications **chronologically from oldest to newest**. Lever automatically displays a candidate's **non-LinkedIn uploaded resume** whenever available alongside tabs for `Application Questions`, `Add Note`, and `Talent Fit` insights. Reviewers click `Advance`, `Archive` (with optional batch-sent rejection emails), or press the **Right Arrow (`→`)** key to Skip.
- **Cross-Opportunity Safeguard Locks:** Lever disables `Advance` and `Archive` in Fast Resume Review if the candidate has another active opportunity at the company where an **offer is generated**, an **upcoming interview is scheduled**, or an active **Nurture email campaign** is running.
- **The "Resumes" vs. "Files" Stakeholder Share Strip (Critical Rule):** Lever stores the primary CV in the **Resume** slot and supplementary uploads (cover letters, portfolios) in the **Files** slot. When a recruiter uses Lever's **Share** feature to email a candidate's profile to an external hiring manager, **only the Resume is attached**—all documents in `Files` are omitted.

---

## 2. Parsing Engine & Technical File Constraints

- **Supported Formats, 10 MB Limit & No OCR:** Lever's parser supports **`.doc`, `.docx`, `.pdf`, `.rtf`, `.txt`, Corel WordPerfect, HTML, MS Office HTML, and `.odf**` up to a **10 MB** maximum file size. Lever does not perform OCR on scanned images (_"If you cannot highlight text, the document is likely not parseable"_), and invalid file types render completely blank in Fast Resume Review.
- **Live Autofill & Manual Form Precedence:** Every `jobs.lever.co` form starts with six non-deletable fields: **Resume** (live autofill trigger at the very top), **Full Name**, **Email**, **Phone**, **Current Location**, and **Current Company**, followed by **Link Questions** (auto-labeled with `"URL"`). Per Lever's API schema, **manual form inputs override parsed resume data**.
- **Date-Calculated "Years of Experience":** Lever does not read summary statements like _"8+ years of experience"_. **Years of experience are calculated strictly from parsed employment dates** on the candidate's resume.
- **The Location Attribution Bug:** Complex layouts or multiple geographic references on a resume frequently cause Lever's parser to misattribute past employer cities as the candidate's current location or output `null` / `"-"`.

---

## 3. Automated Gatekeepers & Native AI Suite ("AI Screening Companion")

### Automation Workflows (Knockout Rules) & Pre-Screening Add-Ons

- **Automation Workflows (`Application form submitted`):** Configured in the Automation Hub, deterministic workflows evaluate application form answers against `Continue if...` conditions to execute four actions: (1) **`Auto-archive`** (knockout rejection with a predefined Archive Reason and optional delayed email such as `Delay: Next weekday` from `no-reply@hire.lever.co` or `mail@hire.lever.co`), (2) **`Add tag`**, (3) **`Change stage`**, or (4) **`Send email`**.
- **Optional Pre-Screening Add-Ons:** Employers may enable **AI Screening by VONQ** (consent-triggered automated AI Chat and AI Voice screening stages immediately after submission) or **Candidate Insights Powered by ProboTalent** (automated skill assessments and authenticity checks in the `New Applicant` stage).

### "Talent Fit" LLM Evaluation (Resume + JD Only)

- **Strict Input Isolation:** As documented in Lever's _Talent Fit AI Brief_, **Talent Fit evaluates ONLY the Job Description and the candidate's resume**—completely ignoring cover letters, custom form answers, and portfolio attachments.
- **PII & Demographic Redaction:** Before sending resume text to LLMs via AWS Bedrock, Employ anonymizes both **PII** and **words indicating gender, race, or military status** (such as pronouns or gendered titles, audited daily via IBM watsonx.governance).
- **Four-Part Talent Fit Output:** Instead of a numeric percentage, Talent Fit outputs:
  1. **Binary Fit Recommendation** (highlighting top-fit matches in the pipeline and Fast Resume Review),
  2. **Candidate's Strengths**,
  3. **Key Considerations**, and
  4. **Areas for Clarification:** A bulleted list of ambiguities or missing context flagged for the recruiter to probe (e.g., listing project management without stating project size/budget, or claiming a qualification without specifying the exact license type).

### "Fraud Signals" (4-Pillar Verification) & "Talent Loss Risk"

- **Four Fraud Signal Checks:** Inside the AI Companion widget, Lever audits:
  1. **Email:** Checks formatting, mail-server validity, and flags **disposable/temporary inbox services**.
  2. **Application Volume:** Counts the exact **number of times the candidate has applied to that company** and form completion speed to flag mass-application patterns.
  3. **Phone Number:** Identifies the line type (`mobile`, `VoIP`, `landline`) and flags **virtual / VoIP numbers** or inactive lines.
  4. **Company Background:** **Searches the web for an online presence of the companies listed in the resume's work experience** to flag unverifiable or fabricated employers.
- **Talent Loss Risk:** Monitors candidates who are a **Talent Fit match AND have passed the recruiter screening stage**, placing a Loss Risk banner at the top of their profile if they sit without stage movement or email contact for **$>7$ days** (`Medium Risk`) or **$>14$ days** (`High Risk`).

---

## 4. Search & Database Discovery Mechanics

- **Field-Specific Boolean Prefixes:** Full-text searches sort results by **Last Interaction Date** and highlight matching snippets in blue with icons indicating `resume`, `work history`, or `tag` matches. Recruiters use strict field operators (no space after the colon; quotes around multi-word phrases):
  - `title:` or `titles:` (e.g., `title:"Product Manager"`) — Searches **only** job titles parsed from the resume's work history.
  - `org:` or `orgs:` (e.g., `org:"Goldman Sachs"`) — Searches **only** company names parsed from the resume.
  - `school:` or `schools:` (e.g., `school:"UT Austin"`) — Searches **only** parsed education entries.
  - `location:` or `locations:` (e.g., `locations:"New York"`).
  - `tag:` or `tags:` (strictly **case-sensitive**).
  - `Resume:` (e.g., `Resume: (NOT "Java")`) — Searches or excludes terms strictly inside the resume body.
- **The 100-Mile Radius Location Rule:** Lever normalizes colloquial city names (`"San Fran, CA"` $\rightarrow$ `"San Francisco, CA, USA"`). Pipeline location filters search by **City or Country, NOT by State or Region**, because Lever automatically searches within a **100-mile radius of any selected city**.

---

## 5. Resume Engineering Playbook for Lever

### File & Stakeholder-Share Consolidation

- Export as a clean, single-column selectable-text **`.docx` or `.pdf` under 10 MB**.
- Because Lever's **Share** email feature strips out separate attachments stored in `Files` (including cover letters and portfolios), include a sharp 3–4 line **Executive Summary** and clickable plain-text URLs to your portfolio/GitHub directly inside the resume header.
- Keep street addresses off the resume and list a single clean `City, State, Country` in the top contact block to prevent the Location Attribution Bug.

### Engineering for `title:`, `org:`, Date-Math & Fraud Web Checks

- **Explicit `Month YYYY – Month YYYY` Dates on Every Role:** Because Lever calculates total **Years of Experience** strictly from parsed employment dates, include explicit months and years (e.g., `Mar 2021 – Present` or `03/2021 – 06/2025`) on every position.
- **Standardized Titles for `title:` Searches:** Pair quirky internal titles with standard industry equivalents (`Member of Technical Staff (Senior Backend Software Engineer)`) so `title:` queries match.
- **Verifiable Company Names for `org:` & Fraud Signals:** Because Fraud Signals searches the web to verify the online presence of past employers, include the recognizable parent brand or company domain in parentheses for stealth startups, DBAs, or LLCs: `Acme Labs (formerly BetaTech | acmelabs.io)`.

### Optimizing for "Talent Fit" & Eliminating "Areas for Clarification"

- **Put 100% of JD Evidence Inside the Resume:** Never rely on a cover letter or form field to explain a required skill, degree, or domain—Talent Fit reads **only** the Resume and Job Description.
- **Preempt "Areas for Clarification" with Explicit Scope:** Wherever you claim a responsibility or credential, explicitly quantify project scale, dollar budget, team size, technical environment, and exact license/certification tiers:
  `Led end-to-end AWS cloud migration ($1.8M budget, 45+ microservices) and managed a cross-functional team of 12 backend and DevOps engineers.`
- **Translate Demographic/Military Terms into Universal Functional Language:** Because Employ's governance pipeline redacts gender, race, and military-identifying terms before running Talent Fit, frame military or affinity leadership in universal civilian/operational terms as well.

---

## 6. Portal Submission & Form Gatekeepers

- **Verify `Current Location` After Live Autofill:** After uploading your resume at the top of `jobs.lever.co`, manually verify the `Current Location` field (ensuring it shows your major metro city for the 100-mile radius filter rather than `null`, `"-"`, or a past employer's city).
- **Apply Early & Upload a Non-LinkedIn Resume:** Because Fast Resume Review is ordered **oldest to newest** and prioritizes displaying a **non-LinkedIn uploaded resume**, apply early with a tailored `.pdf` or `.docx`.
- **Pass Fraud Signals & Respect Application Volume:** Use a standard mobile carrier number (never VoIP) and a permanent email address (never disposable), and apply selectively—Fraud Signals tracks and flags high **Application Volume** at the same company.

---

## 7. Output Specifications for this ATS

### Target Evaluation Metric Display

**Talent Fit Recommendation:** Binary Fit Match (Zero "Areas for Clarification" Flagged)
**Parser & Prefix Search Index:** 100% `title:`, `org:`, `school:` & Date-Calculated YoE Alignment
**Fraud Signals & Workflow Status:** Cleared 4-Pillar Verification & Auto-Archive Rules

### Remaining Gaps Analysis

Identify any job description requirements that are missing from the resume text or lack specific scope/scale details (such as budget size, team size, system scale, or exact license tier) that would cause Lever's **Talent Fit** LLM to flag them under **Areas for Clarification**. Provide targeted clarification prompts to help the candidate supply concrete scope metrics.
