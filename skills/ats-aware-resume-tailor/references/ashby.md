# Ashby ATS Reference Guide

## 1. System Profile & Recruiter Review Interface

- **Core Philosophy:** Ashby combines deterministic submission gatekeepers (Auto-Reject rules, Application Limits, and Fraud Detection) with criteria-driven LLM evaluation (AI-Assisted Application Review and AI Talent Rediscovery) inside a high-speed human triage workspace.
- **Bulk Application Review & Three-Tab Layout:** Recruiters and hiring managers evaluate applicants individually on the Candidate Profile or in batches via Bulk Application Review, using top toggle tabs to switch between: (1) all application information (form answers and resume together), (2) only application form answers, or (3) only the uploaded resume.
- **Dual Resume Storage Mechanism:** Ashby stores resumes at two distinct levels: on the specific Application and on the overarching Candidate Profile. If a candidate has uploaded different resumes across roles, Bulk Application Review displays the resume attached to that specific job application while providing a `See Latest Resume` button if a newer file exists on the overarching profile. Reviewers also see all other applications under the `Considered for jobs` section.
- **Rapid Keyboard Triage & Queue Sorting:** Reviewers sort the queue chronologically (newest first or oldest first) or rank applicants by **AI Job Criteria Met Percentage**. Using built-in keyboard shortcuts (Left/Right arrow keys to cycle candidates, `Esc` to close profiles), reviewers log a Score, apply Tags, add Notes, Advance, trigger Custom Next Steps, or Archive (Reject) with a mandatory archive reason.

---

## 2. Parsing Engine & Technical File Constraints

- **Supported File Formats & 50 MB Ceiling:** Ashby accepts standard **PDF, DOC, and DOCX** resume files up to a maximum file size of **50 MB**.
- **"Autofill from Resume" Scope:** When an employer enables `Show Autofill Field on Application Forms?`, uploading a resume in the top **Autofill from resume** box extracts and auto-populates only supported fields: **Resume** (file attachment), **Name**, **Email**, **Candidate's Location**, **Candidate's Education History** (`School name`, `Degree`, `Major`, `Start date`, `End date`), and **Social Links** (such as **LinkedIn** or **GitHub** URLs). Any field the parser cannot cleanly identify remains blank unless manually filled in.
- **Baseline vs. Opt-In "Enhanced Resume Parsing":** While Ashby offers an opt-in AI **Enhanced Resume Parsing** feature (`Admin > Organization Setup > Opt-In Features`) that handles complex or multi-column layouts and expands multi-language support (including Arabic, Armenian, Hebrew, Korean, Afrikaans, Albanian, and Amharic), candidates cannot tell if a company has enabled it and must format for the baseline single-column parser.
- **Form Field Connectors:** Admins attach Form Field Connectors to questions on Default, Shared, or Custom forms to automatically map candidate responses into structured, searchable **Candidate Custom Fields** on the Candidate Profile.

---

## 3. Automated Gatekeepers & AI Evaluation Mechanics

### Deterministic Submission Gatekeepers

- **Job Board Application Spam Protection:** Evaluates submission telemetry to separate human applicants from bots across _Permissive_, _Less Permissive_, or _Strict_ thresholds.
- **Application Limits (Throttling & Cooldowns):** Tracks the **email address** entered on the application form across three rule types: (1) **Throttling applications** (caps total applications to any job in scope over $X$ days), (2) **Repeat applications (same job)** (blocks rejected candidates from reapplying to the same role for $X$ days), and (3) **Repeat applications (any job)** (blocks rejected candidates from applying to any role in scope for $X$ days). These limits are bypassed if a recruiter sends a direct **Application Request** link or if applying via Indeed.
- **Auto-Reject Rules & Fuzzy Text Matching (`Similar to`):** Evaluates form answers at the exact moment of submission (per-job or via Enterprise Global Auto-Reject Rules) using boolean logic (`And`, `Or`, `Not`) and an optional configurable **delay (in days)** before dispatching the rejection email. In addition to multiple-choice fields, Auto-Reject evaluates direct text inputs using `Is empty/unassigned` (rejects blank optional fields), `Contains` / `Equals`, and **`Similar to` (Fuzzy Match)**—where partial word overlaps (e.g., typing `local` when `localization` is configured) trigger the rule.

### AI-Assisted Application Review (Up to 50 Job Criteria)

- **Atomic Resume & Form Criteria:** Powered by external LLMs (OpenAI, Amazon Bedrock, Google Gemini), Ashby evaluates applications against **up to 50 explicit criteria** defined for the job:
  1. **Resume Criteria:** Evaluates the uploaded resume (or profile education/work history if no resume is uploaded) using AI-suggested criteria from the JD, reused criteria, or custom prompts (`Short title` + instructional `Prompt`). Ashby's validation gate requires criteria to be **Stand-alone**, **Singular** (one skill per criterion, such as splitting Python and AWS), **Clear**, **Specific** (measurable years of experience or titles), **Objectively verifiable**, and EEO-compliant.
  2. **Application Form Criteria:** On long-form text questions, toggling `Did the Application answer the question?` checks whether the candidate answered the prompt and flags blank, filler, or off-topic responses as **`Does not meet`**.
- **PII Redaction & Three-State Criterion Grading:** Ashby automatically redacts personal details (PII) before sending resumes to AI models and never trains models on candidate data. Each criterion is graded as **`Meets`**, **`Does not meet`**, or **`Undecided`** (when resume text is ambiguous or lacks sufficient detail), accompanied by written reasoning, linked resume evidence, and an overall **`AI Job Criteria Met Percentage`**.
- **Human-in-the-Loop & Candidate Opt-Out:** AI never autonomously rejects or advances candidates. Candidates can click the `Learn More` legal notice under `Submit Application` to opt out of AI processing, which skips AI evaluation entirely and leaves their `AI Job Criteria Met Percentage` uncalculated.

### Native Candidate Fraud Detection

- **Four Evaluated Signal Categories:** Audits incoming applications across: (1) **Device Signals** (browser/device fingerprinting anomalies), (2) **IP Address Signals** (VPN/proxy routing and mismatches between stated location and submission IP, exposing the exact IP address in the Fraud Signal History UI), (3) **Email Signals** (disposable domains, poor domain reputation, institution impersonation), and (4) **Phone Signals** (invalid, burner, or suspicious VoIP numbers).
- **Fraud Review Workflow:** Flagged profiles display a **`Fraud Signals Detected`** warning badge and optional **AI Candidate Fraud Signal Summary** until a human marks the profile as **`Mark Fraud`** (archived as `Rejected by Organization: Fraudulent Candidate`), **`Not Fraud`** (`Fraud Signals - Cleared`), or **`Unsure`**.

---

## 4. Search & Database Discovery Mechanics

- **Full-Text Resume Search Modes:** Recruiters query resumes manually or via the natural-language **AI Filter Assistant** across four matching modes:
  - **`Matches`:** Case-insensitive match of all individual words anywhere in the resume, regardless of word order (supports `OR`, `NOT`).
  - **`Contains`:** Case-insensitive match of the exact phrase in the exact word order (supports `AND`, `OR`, `NOT`).
  - **`Equals`:** Case-sensitive exact phrase match (supports `OR`, `NOT`).
  - **`Similar`:** Case-insensitive stemming/variation match for partial matches or word forms (supports `AND`, `OR`, `NOT`).
- **Boolean Syntax & Exclusion Quirk:** Supports `OR` (`|`, `,`, `OR`), `AND` (`&`, `+`, `:`, `AND`), exact phrase quotes (`"software engineer"`), prefix wildcards (`*script`, `*end`), and brackets (`( )`, `[ ]`). Typing the word `NOT` does not work for exclusions in Ashby; recruiters must use `!` or `-` (e.g., `!remote`).
- **AI Talent Rediscovery & AI Candidate Assistant:** **AI Talent Rediscovery** scans existing database and project candidates against a new job's AI criteria (grouping matches meeting **50%+ of set criteria**) and highlights **Positive Rediscovery Signals** (`Warm Lead`, `Silver Medalist`, `Made It To Offer Stage`, `Bad Timing`, `Rejected by Candidate`, `Only Good Feedback`, `Hired for Different Role`). The **AI Candidate Assistant** profile chat summarizes resumes, form answers, emails, and visible interview feedback.

---

## 5. Resume Engineering Playbook for Ashby

### File & Autofill Header Architecture

- Export as a selectable-text **PDF or DOCX** under **50 MB** using a clean, single-column layout built for the baseline parser.
- Format Autofill-targeted fields as plain, visible text at the top of Page 1: Name, Email, City/State Location, full URLs (`[linkedin.com/in/yourname](https://linkedin.com/in/yourname)`, `[github.com/yourname](https://github.com/yourname)`—never hidden behind icons), and clearly structured Education (`Bachelor of Science in Computer Science | University Name | 2018 – 2022`).

### Engineering for 100% "AI Job Criteria Met" (Zero `Undecided` Ratings)

- **Unbundle Compound Requirements into Atomic Proof Points:** Because Ashby validates each AI criterion to be **Singular** and **Objectively verifiable**, address every individual tool, methodology, and qualification from the job posting explicitly in your text.
- **Eliminate `Undecided` Ratings with Explicit Scope & Tenure:** Never force the LLM to guess what your past employer does or calculate ambiguous dates. Explicitly state company domain, product scale, and exact tenure in role headers and bullets:
  `Senior Product Designer, Acme Corp (Consumer SaaS platform, 2M+ MAU) | Mar 2021 – Jun 2025 (4+ years)`
- **Include Exact Phrases & Root Variations:** Pair standard full job titles (`"Software Engineer"`, `"Product Manager"`) with both spelled-out terms and acronyms to match `Matches`, `Contains`, `Equals`, and `Similar` search modes.

---

## 6. Portal Submission & Form Gatekeepers

- **Verify Autofilled Fields:** Always inspect Location, Education History, and Social Links after clicking **Autofill from resume**, as Form Field Connectors map them directly into your permanent Candidate Profile.
- **Never Leave Long-Text Questions Blank or Write Filler:** Writing `"N/A"` or `"See resume"`—or leaving optional text prompts blank—triggers **`Does not meet`** in AI Application Form Criteria (`Did the Application answer the question?`) or immediate rejection via `Is empty/unassigned` Auto-Reject rules.
- **Leave AI Processing Enabled:** Do not click the Opt-Out link under `Submit Application` if your resume is tailored; opting out prevents Ashby from calculating your `AI Job Criteria Met Percentage` and drops you out of the AI-sorted queue.
- **Maintain Clean Digital Identity Signals:** Apply with VPNs/proxies turned off (so your IP matches your stated location), use a reputable personal email and standard mobile carrier phone number (avoiding VoIP/disposable flags), and apply selectively to 1–2 roles to avoid email-based **Application Limit** blocks.

---

## 7. Output Specifications for this ATS

### Target Evaluation Metric Display

**AI Job Criteria Met:** [X]% ──► [Y]% (`Meets` vs. `Undecided` / `Does not meet`)
**Boolean Search Alignment:** 100% Coverage across `Matches`, `Contains`, & `Similar` modes
**Automated Gatekeeper Status:** Cleared Auto-Reject (`Similar to`), Form Criteria & Fraud Signals

### Remaining Gaps Analysis

Identify any singular, objectively verifiable requirements from the job description (specific tools, exact years of experience, product domains, or credentials) that lack explicit proof on the resume and would be graded as **`Undecided`** or **`Does not meet`** by Ashby's AI-Assisted Application Review. Ask targeted clarification questions to uncover verifiable scope, tenure, or adjacent evidence.
