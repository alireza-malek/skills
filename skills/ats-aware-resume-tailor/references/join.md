# JOIN ATS Reference Guide

## 1. System Profile & Recruiter Review Interface

- **Core Philosophy:** Built for SMB hiring teams around the methodology of **"Knockout questions first, deep CV read second."** JOIN scopes its AI strictly to operational tasks (CV parsing, Job Studio ad drafting, scheduling, summarization) and **does not use an autonomous AI match score** to rank or reject resumes.
- **Four Fixed Pipeline Stages & The Hidden `Applied` Stage:** Every pipeline template is built on four fixed stages: **`Applied`**, **`Application Review`**, **`Offer`**, and **`Hired`** (with custom `Application review` or `Interview` stages placed between `Application Review` and `Offer`). Qualifying applicants bypass `Applied` and land directly in **`Application Review`** (mapped to the system `Screening` state). The **`Applied`** stage remains hidden in the active pipeline and only appears under the **`Rejected`** tab when an applicant fails a **Knockout Question**.
- **Three-Panel Candidate Profile & Chevron Triage:** Recruiters cycle through candidates using top-bar chevron arrows (`<` `>`) inside a three-panel workspace:
  1. **Left Panel:** Photo/initials, Name, Candidate Source, timestamp, Email, Phone, Location (country), Social/Professional links (LinkedIn, XING, GitHub, portfolio, website), GDPR consent status, Quick Actions, and uploaded files.
  2. **Centre Panel:** Native **Embedded Document Viewer** rendering the uploaded CV in-browser alongside tabs for `Overview` (Screening question answers + Scorecards summary), `Emails`, `Interviews`, `Scorecards`, and `Documents`.
  3. **Right Panel:** Assigned `Jobs` (with stage and `Move forward` button), searchable `Tags`, and threaded internal `Notes` (supporting `@mentions` and private lock icons).
- **Consolidated Profile & "Most Recent CV" Overwrite Rule (Critical Rule):** Multiple applications from the same candidate merge into a single row in the global `Candidates` table. Crucially, **"If a candidate has applied to multiple jobs and submitted multiple documents, only the most recent CV is displayed here."** Furthermore, if a recruiter rejects a candidate from the global `Candidates` tab rather than inside a single job's pipeline, JOIN automatically rejects **all** of their active applications at once.

---

## 2. Parsing Engine & Technical File Constraints

- **Supported Formats & Hybrid Parser:** Accepts **`.pdf`, `.doc`, `.docx`, `.txt`, and `.rtf**` (with email attachments capped at **8.5 MB**). JOIN's hybrid pattern-rule and machine-learning parser automatically extracts **name, contact details, skills, and work history** into the profile (where personal details reflect what the candidate provided and cannot be manually edited by recruiters).
- **Three Documented Parser Failure Modes (Official JOIN Documentation):**
  1. **Creative & Multi-Column Formats:** Two-column layouts, tables, text embedded in images, and design-heavy CVs confuse section detection (_"A two-column design can scramble which dates belong to which job"_).
  2. **Non-Standard Structure & Skills in Prose:** Unusual section headings, mixed languages within the same document, or _"skills buried in prose rather than a list often get missed."_
  3. **Complex Dates & Compound Titles:** Titles combined with internal leveling or region codes—specifically cited by JOIN as **`"Senior Engineer II, EMEA"`**—or unlabelled gap years get misread or dropped by the parser.

---

## 3. Automated Gatekeepers: Knockout Questions & Workflow Automations

### Application Question Tiers & Knockout Rules

- **Three Question Tiers (`Optional`, `Required`, `Knockout`):** `Full name` and `Email address` are always mandatory (`CV` is Required by default; `Profile picture` and `Professional links` are always Optional). Standard screening questions (`Phone number`, `Country of residence`, `Availability`, `Work authorisation`, `Salary expectations`) and custom questions use 7 answer types, but **only 5 types can be configured as `Knockout**`:
  - **`Yes / No` (Boolean), `Single choice`, `Multiple choice`, `Number`, and `Date**`.
- **Free-Text & File Uploads Never Reject:** **`Short text` and `File` upload questions can NEVER be set to Knockout** (_"Text and file upload are Optional or Required only, never Knockout: there's no rule to match a free-text answer or a file against, so they can't reject"_).
- **Next-Day Delayed Knockout Email (24-Hour Grace Window):** Failing a Knockout question immediately marks the application `Rejected` in the hidden `Applied` stage and logs the exact answer and rule in an audit trail, but **delays sending the rejection email until the next day** so recruiters can reverse false positives (`Undo rejection`) with one click.

### Visual Workflow Automations & Time-Based Auto-Reject

- **No-Code Recruitment Automation Editor:** Chains automated actions (Send email, Change stage, Add tag) from 4 triggers: (1) stage move, (2) source board, (3) application-form answer, or (4) manual profile button click—with conditional branching by **score, location, department, or language**.
- **Stale Screening Auto-Reject:** Admins can enable **Auto-reject applications** in `Settings → Pipelines` to automatically transition applications to `Rejected` if they sit untouched in the initial `Screening` stage longer than a configured number of days or weeks.

---

## 4. Search, Sorting & Structured Scorecard Mechanics

- **Global/Table Search & Chronological Sorting:** Recruiters manage pipelines in **Cards view** (Kanban) or **Table view** (10–100 rows) and search across **tags, country, CV, email address, and GDPR consent status** (`Cmd + K` / `Ctrl + K`). Pipeline queues sort strictly by **`Time in stage`**, **`Applied on` / `Last Applied On**`, or **`Name`\*\*.
- **8 Quick Apply Boards & The Spontaneous Application Deletion Trap:** JOIN supports native 1-click Quick Apply on **Indeed, LinkedIn, StepStone, XING, Talent.com, Arbeitnow, InfoJobs, and Job Today**. On **Spontaneous Applications**, recruiters see only `Accept` (saves to Talent Pool) or `Reject`—and clicking **`Reject` permanently deletes** the spontaneous submission and profile data.
- **4-Tier Structured Interview Scorecards:** Scorecards remain private to their author until published, requiring a mandatory written summary under **`Overall conclusions, pros and cons`** and an **Overall impression** on a 4-point verdict scale:
  `Definitely no ──► No ──► Yes ──► Definitely yes`

---

## 5. Resume Engineering Playbook for JOIN

### Parser-Safe Formatting (Preventing Documented Failures)

- Use a strict **single-column, selectable-text `.pdf` or `.docx**` (under 8.5 MB) written entirely in the single configured language of the job ad so dates never scramble across jobs.
- **Dedicated Bulleted/Comma-Separated `Skills` Section:** Never bury core skills exclusively inside prose paragraphs; list hard skills, tools, frameworks, and languages under a standard `Skills` heading.
- **Clean, Standardized Job Title Headers:** Strip internal level and region codes from role headers—write `Senior Software Engineer` rather than `"Senior Engineer II, EMEA"`—and clarify short stints parenthetically next to dates (e.g., `1-Year Fixed Contract`).

### Engineering for the 5–10 Minute "Deep CV Read" & Scorecards

- **Front-Load Clickable Work Samples & Outputs:** JOIN's screening methodology ranks clickable work outputs (code repositories, portfolios, shipped products, published work) as the #1 predictive signal during the deep CV read. Place active links at the top of Page 1 as well as in the portal's `Professional links` field.
- **Write "Shipped / Measured / Changed" Bullets:** Replace vague claims (`"Led marketing for a B2B SaaS"`) with concrete operational proof that equips interviewers to select **`Definitely yes`** and complete the scorecard's mandatory **pros and cons** box:
  `Took outbound lead volume from 60 to 280 per quarter over two quarters by rebuilding the SDR cadence and switching ICP focus to mid-market.`

---

## 6. Portal Submission & Form Gatekeepers

- **Scrutinize the 5 Knockout Question Types:** Verify exact compliance on `Yes/No`, `Single/Multi-choice`, `Number`, and `Date` fields—especially **`Availability`**, **`Work authorisation`**, and **`Salary expectations`**—to avoid landing in the hidden `Applied` rejected bucket.
- **Populate Optional `Professional Links` & `Country of Residence`:** Filling out LinkedIn, XING, GitHub, and portfolio fields populates the dedicated `Professional Links` icon column in the recruiter's Table view.
- **Protect Against the "Most Recent CV" Overwrite:** Avoid mass-applying to disparate roles at the same JOIN company; if applying to two related roles, ensure your most recently uploaded CV encompasses both roles, and never submit a Spontaneous Application without a tailored Cover Letter explaining your motivation.

---

## 7. Output Specifications for this ATS

### Target Evaluation Metric Display

**JOIN Scorecard Alignment:** Positioned for `Yes` / `Definitely yes` Verdict (Strong Pros/Cons Evidence)
**Hybrid CV Parser Integrity:** 100% Single-Column Date Mapping, Explicit Skills List & Clean Titles
**Automated Gatekeeper Status:** Passed 5-Type Knockout Rules & Workflow Branching

### Remaining Gaps Analysis

Identify any hard eligibility thresholds (Work authorisation, Availability, Salary expectations, years of experience, language levels) that could trigger a **Knockout Question** (`Yes/No`, `Choice`, `Number`, `Date`), as well as any vague experience claims that lack **"shipped / measured / changed"** metrics or clickable work outputs for the human "deep CV read". Frame clarification prompts to capture concrete metrics and work samples.
