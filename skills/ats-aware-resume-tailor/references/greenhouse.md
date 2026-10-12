# Greenhouse Recruiting ATS Reference Guide

## 1. System Profile & Recruiter Review Interface

- **Core Philosophy:** Engineered around "Structured Hiring." Humans make all final advance/reject decisions, but automated knockout rules, Lucene full-text search, and Real Talent prioritization control who gets reviewed first.
- **The Application Review Stage:** Recruiters triage candidates in a high-speed split-screen workspace. The left pane renders the exact uploaded document (PDF or Word converted in-browser), while the right pane displays contact metadata and screening question responses.
- **Review Speed:** 10 to 20 seconds per candidate using built-in keyboard shortcuts to advance, reject, or skip.
- **Resume Anonymization Mode:** When blind hiring is enabled, Greenhouse uses machine learning to blur identifying PII (name, email, phone, photos, address) directly over the original document preview. It does not strip the resume into plain text.
- **Candidate Auto-Merge & Unified Profile:** Incoming applications are cross-referenced by exact Email Address or First Name + Last Name + Phone Number. Applications are consolidated into a single permanent Candidate Profile displaying all past applications, prior interview scorecards, past rejection reasons, and previously uploaded resumes.

---

## 2. Parsing Engine & Technical File Constraints

- **The 2.5 MB Parsing Ceiling (Critical Rule):** Greenhouse officially documents that while files up to 100 MB can attach, the resume parser aborts on any file larger than 2.5 MB. If a file exceeds 2.5 MB, it attaches as an unindexed static document: profile fields remain blank and resume text is completely invisible to recruiter Lucene searches.
- **Structured Data Extraction:** The parser extracts data into database arrays: `Employments` (`company_name`, `title`, `start_date`, `end_date`), `Educations` (`school_name`, `degree`, `discipline`), and contact details.
- **Achievement Bullets Are Indexed, Not Structured:** Achievement bullets are not placed into editable profile fields; they are indexed directly into the backend Full-Text Lucene Search engine.
- **Standardized Education Dropdown Trap:** The application form maps education against standardized database dropdown lists (`School`, `Degree`, `Discipline`). Non-standard abbreviations often fail to auto-select, leaving dropdowns blank and excluding candidates from structured recruiter education filters.
- **No Image OCR & Header Isolation:** Greenhouse does not perform OCR on flattened image PDFs. Information inside Word or PDF margin headers/footers is routinely ignored by the parser.

---

## 3. Automated Gatekeepers: The Application Rules Engine

Automated rejections in Greenhouse stem from deterministic rules tied to custom screening questions (Yes/No, single-select, multi-select), not AI resume reading:

- **Auto-Reject Rules:** Triggered immediately when a disqualifying response is submitted (e.g., visa sponsorship required, missing mandatory license). Candidates are routed to the Rejected bucket.
- **Auto-Advance Rules:** Automatically advances qualifying candidates to a designated interview stage (e.g., Recruiter Screen).
- **Auto-Tag Rules:** Attaches tags (e.g., `Security Clearance`, `Python`) for quick recruiter filtering.
- **Conflict Rule Hierarchy:** An Auto-Reject rule always overrides an Auto-Advance rule.
- **Configurable Delayed Rejection Timing:** Automated rejection emails can be delayed (immediate, 1 hour, 2 hours, 1 day, 2 days, 3 days, or 7 days). Rejections received days after submission often originate from submission-time rules.

---

## 4. Search & Database Discovery Mechanics

### Full-Text Lucene Search Syntax

Recruiters search resumes, cover letters, and application answers using strict Lucene syntax:

- **Uppercase Boolean Operators:** Operators must be capitalized (`AND`, `OR`, `NOT`). Lowercase operators are treated as search keywords.
- **Exact Phrase Matching:** Enclosing terms in quotation marks (`"product management"`) requires exact word order.
- **Wildcards:** The asterisk (`*`) matches multiple characters; the question mark (`?`) matches a single character (e.g., `architect*` matches architect, architecture, architecting).
- **Geographic Radius:** Recruiters filter candidates by distance radius (e.g., within 25 or 50 miles/km of a postal code).

### Greenhouse Real Talent™

- **Talent Matching:** Uses OpenAI-powered calibration based on skills and background competencies defined by the hiring team to reorder the unreviewed queue so top-match candidates are evaluated first.
- **Fraud Detection:** Analyzes submission telemetry, disposable email domains, and phone validity.
- **Human-in-the-Loop:** AI never automatically rejects or advances candidates on its own.

---

## 5. Resume Engineering Playbook for Greenhouse

### File & Header Architecture

- File size must be strictly under 2.5 MB (aim for < 1 MB). Selectable-text PDF or DOCX only.
- Single-column layout. Contact block placed in the body text at the top of Page 1: Name, Phone, Email, City/State/Postal Code, LinkedIn URL. Include Postal Code for radius searches.

### Employment History Mapping

- Format employment entries clearly so the parser reliably populates the `Employments` database array and the Application Review summary card:
  `Senior Product Manager | Acme Corp | 03/2021 – 05/2024`
- For non-standard internal titles, append the industry standard in parentheses: `Member of Technical Staff III (Senior Backend Engineer)`.

### Keyword Density & Boolean Alignment

- Mirror exact phrases from the job description's required competencies.
- Use both acronyms and full terms on first reference: `Search Engine Optimization (SEO)`, `Amazon Web Services (AWS)`, `Continuous Integration / Continuous Deployment (CI/CD)`.

### Structuring for the 5-Point Scorecard

Greenhouse interviewers evaluate candidates against predefined attributes using a standardized rating scale:
`Definitely Not ──► No ──► Mixed / Neutral ──► Yes ──► Strong Yes`

- Optimize bullet points for 10–20 second visual scanning by front-loading measurable outcomes:
  `[Strong Action Verb] + [Quantified Result] + [Tool / Method / Context]`
- Put the strongest, JD-aligned accomplishments in the top 2–3 bullets of each role.

---

## 6. Portal Submission & Form Gatekeepers

- **Verify Education Dropdowns:** After uploading a resume on a Greenhouse-hosted form, manually check the standardized dropdowns for `School`, `Degree`, and `Discipline`. If blank or misidentified, select the matching options from the lists.
- **Screening Questions Diligence:** Treat all dropdown and Yes/No questions as pass/fail criteria to avoid triggering Auto-Reject rules.
- **Application Hygiene:** Because all applications merge under a single Candidate Profile displaying past rejections, scorecards, and applied roles, apply selectively to closely related positions.

---

## 7. Output Specifications for this ATS

### Target Evaluation Metric Display

**Scorecard Alignment:** [X]% Core Competency Coverage
**Lucene Search Match:** 100% Exact-Phrase & Boolean Keyword Density
**Automated Gatekeeper Status:** Passed Screening Rules & Dropdown Checks

### Remaining Gaps Analysis

Identify missing qualifications that correspond to potential Scorecard Attributes or Auto-Reject criteria from the job posting. Ask targeted clarification questions to see if the candidate has unlisted experience that can be safely integrated.
