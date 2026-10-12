# Workday Recruiting ATS Reference Guide

## 1. System Profile & Recruiter Review Interface

- **Core Architecture (Single-Tenant-Per-Employer Dual System):** Workday operates on isolated employer tenants (`myworkdayjobs.com`) where candidates create a tenant-specific **Candidate Home** account. It separates overarching **Prospect / Active Candidate Profiles** from requisition-specific **Job Applications** (with all applications inside that company's tenant visible via the `Jobs Applied To` column unless restricted). In Workday, structured portal form fields and the uploaded resume document function as a dual system.
- **Four Application Intake Modes:** Employers configure their **Job Application Template** around four intake paths: (1) **Autofill with Resume (Quick Apply)** (parses the uploaded CV into a draft form that the candidate must review and fix before submitting), (2) **Use My Last Application** (clones structured fields from a prior application in that tenant while allowing a new resume upload), (3) **Apply Manually**, and (4) **Apply with LinkedIn / Conversational Chat (`Workday Paradox`)**.
- **Configurable Candidate Grid & Six Default Hidden Columns:** Unless sorted by **HiredScore Grade (`HS`)** or deterministic **Rank Candidates** templates, Workday sorts the Candidate Grid by **recruiting stage and job application creation date**. By default, Workday hides six columns to optimize grid loading: **`All Job Titles`, `Email`, `Jobs Applied to`, `Phone`, `Resume`, and `Skills**`—but when `Filter on Hidden Columns`is active, recruiters filter the left-hand facet panel by`All Job Titles`, `Skills`, and `Candidate Location` even while those columns remain hidden on screen.

---

## 2. Parsing Engine & Technical File Constraints

- **Supported File Formats & 30 MB Ceiling:** Workday's matching and AI grading engines process **`.DOC`, `.DOCX`, `.PDF`, `.RTF`, `.TXT`, `.HTM`, and `.HTML**`(with`.DOCX`and text-based`.PDF` designated as primary inputs) up to a **30 MB** system ceiling (often capped at 5 MB by front-end widgets).
- **The `Skills` & `Languages` Non-Autofill Rule (Critical Rule):** Workday's Administrator Guide officially states: _"Workday doesn't auto-fill fields you configure as hidden or these fields: Languages, Skills."_ In baseline configurations, the parser **never** auto-populates the `Skills` or `Languages` portal fields from a resume. Only if the employer enables Workday Skills Cloud with `Populate Suggested Skills for External Candidates and Prospects` will Workday suggest extracted skills—and the candidate must still manually confirm them in the portal form.
- **Formatting & Stacked-Role Vulnerabilities:** Images, image-based styles, tables, multi-column layouts, headers/footers, and stacking multiple promotions/titles under a single date range corrupt Workday's field extraction—and cause HiredScore Spotlight to refuse assigning a grade if the resume cannot be parsed.
- **The Tenant School Look-Up Trap:** When `Enable Candidate School Look Up for External Career Sites` is active, external candidates cannot enter free-text university names and must select their school from the employer's pre-loaded Workday school table.

---

## 3. Automated Gatekeepers & AI Engines (`Skills Cloud` & `HiredScore AI`)

### Recruiting Questionnaires & Automatic Stage Routing (ASR)

- **Scored & Disqualification Questionnaires:** Employers attach up to 4 questionnaires per requisition (Primary, Secondary, Supplementary) with point values (`Questionnaire Total Score`) and **Disqualification Questions**.
- **Automatic Stage Routing (ASR) Condition Rules:** Enabled on the `Initiation` step of the `Review Candidate` subprocess, ASR evaluates **Business Process Condition Rules** sequentially in exact display order. Matching a knockout rule immediately executes a **Recruiting Disposition** (declining the candidate with a generic external `Candidate Label Override` in Candidate Home). Additionally, when the final opening on a requisition reaches `Ready for Hire`, the `Decline Remaining Candidates` batch step automatically declines all remaining active applicants.

### Workday Skills Cloud (~60,000 Canonical Skills)

- **Node-Level Ontology Normalization:** Workday Skills Cloud normalizes **Skills Cloud skills** (~60,000 canonical terms), **Maintained skills** (tenant-custom), and **Crowdsourced skills** via automated synonym conversion. Recruiters filter and match at the **skill-entity node level** rather than simple text substrings; obscure abbreviations that fail to map to a canonical node will not register in skill filters. (Note: Legacy `Candidate Skills Match` outputs Strong/Good/Fair/Low tiers in English only without considering skill recency or tenure, and is being retired in favor of HiredScore/Illuminate).

### HiredScore AI for Recruiting (`Spotlight` Grading & LLM `Fit & Gap`)

- **Spotlight Candidate Grading (`A`, `B`, `C`, `D`):** Evaluates parsed resumes against requisition requirements (work history, skills, education, years of relevant experience, and certifications) and assigns a letter grade in the recruiter's `HS` column (never autonomously rejecting candidates):
  - **`Grade A`:** Meets **all basic (required) qualifications**, **most preferred qualifications**, and exceeds the average qualification match threshold.
  - **`Grade B`:** Meets **all basic qualifications**.
  - **`Grade C`:** Meets **most, but not all, basic qualifications**.
  - **`Grade D`:** Does **not meet most basic qualifications**.
    (No grade is assigned if no resume is submitted, the file format is unsupported/unparseable, no job description exists, or it is a campus/graduate requisition).

- **LLM-Driven `Fit & Gap` Across 6 Target Sections:** Evaluates each screenable basic and preferred qualification individually as **`Met`** or **`Not Met`**, generates a plain-language explanation, and deep-links directly to supporting evidence across six specific parsed sections: **`professional experience`, `education`, `skills`, `languages`, `certifications`, and `resume summary`**.

---

## 4. Search & Database Discovery Mechanics

- **`Find Candidates` Facets & Dynamic Pools:** For external candidates, `Find Candidates` facet filters evaluate data across **all job applications** the candidate has submitted within that employer's tenant. Recruiters also build automated **Dynamic Candidate Pools** (up to 50,000 members) based on saved search criteria.
- **HiredScore Spotlight Keyword Search, `Fetch` & `Reverse Fetch`:** Inside Spotlight, recruiters filter queues using **Keywords Search** (dynamically isolating resumes containing exact terms) alongside filters for **HiredScore Grade (A–D)**, **Distance**, **Education level & Major**, **Years of experience**, **Languages**, and **Visa status**. **HiredScore Fetch** proactively scans past applicants and silver medalists for new requisitions (`Show Only Recommended By Fetch`), while **Reverse Fetch** recommends up to 4 open requisitions for any candidate viewed in Global Talent Search.

---

## 5. Resume Engineering Playbook for Workday

### File, Section & Role Architecture

- Submit a single-column, selectable-text **`.DOCX` or `.PDF**` well under **30 MB** (aim for < 2 MB) with zero images, tables, text boxes, or margin headers/footers.
- Label sections using the exact six headings targeted by HiredScore's `Fit & Gap` engine: `Professional Summary`, `Work Experience`, `Education`, `Skills`, `Certifications`, and `Languages`.
- **Unbundle Every Role at the Same Employer:** Never stack multiple promotions or job titles under a single date range. Format each position as a distinct entry with Employer Name, Complete Job Title, Location, and consistent `Month YYYY – Month YYYY` or `MM/YYYY – MM/YYYY` dates (use `"Present"`—never `"Now"` or `"Current"`) so Workday creates clean work history blocks and accurately calculates total years of experience.

### Engineering for HiredScore `Grade A` & Skills Cloud Nodes

- **Satisfy 100% of Basic AND Most Preferred Qualifications:** Because meeting only Basic Qualifications caps your Spotlight grade at **`Grade B`** (and missing even one or two Basic Qualifications drops you to **`Grade C`**), address every Basic Qualification and the majority of Preferred Qualifications explicitly.
- **Dual-Anchor Evidence in `Summary` + `Work Experience`:** Include a 3–4 line **Professional Summary** stating total years of relevant experience, core domain, degrees/certifications, and top required qualifications, then reinforce each qualification with quantified outcomes in your **Work Experience** bullets so `Fit & Gap` links proof from both sections.
- **Canonical Skills Cloud Naming:** Mirror exact skill phrasing from the requisition's Qualifications block and write out full canonical names with acronyms in parentheses on first reference (`Search Engine Optimization (SEO)`, `Customer Relationship Management (CRM)`, `Enterprise Resource Planning (ERP)`).

---

## 6. Portal Submission & Form Gatekeepers

- **Audit the "Autofill with Resume" Draft Step:** Never click through the Workday form without checking every pre-filled **Job Title**, **Company Name**, **Date**, and **Role Description** block; misaligned fields exclude you when recruiters filter on hidden columns like `All Job Titles`.
- **Manually Populate the Portal `Skills` & `Languages` Fields:** Because Workday's baseline parser skips `Skills` and `Languages`, manually type skills into the portal's Skills prompt, wait for the **Skills Cloud dropdown** to load, and select the canonical tags matching the requisition.
- **School Look-Up & "Use My Last Application" Hygiene:** Search root university keywords (e.g., `"Berkeley"` instead of `"UCB"`) to match the tenant's school table, and update both structured form fields and the resume attachment whenever using `Use My Last Application`.
- **Apply Directly on `myworkdayjobs.com`:** Avoid applying via embedded third-party frames on Safari/iOS where blocked third-party cookies can break authentication, and monitor **Candidate Home** for stage-triggered Supplementary Questionnaires.

---

## 7. Output Specifications for this ATS

### Target Evaluation Metric Display

**HiredScore Spotlight Grade:** Grade [C/B] ──► Grade [A/B] (`Fit & Gap` Basic + Preferred Coverage)
**Workday Skills Cloud Match:** 100% Canonical Skill Node & Acronym Alignment
**Autofill & ASR Readiness:** Structured 6-Section Layout + Passed ASR Questionnaire Gates

### Remaining Gaps Analysis

Separate any missing qualifications into **Basic (Required) Qualifications** (which drop the candidate from Grade B to Grade C/D if unmet) and **Preferred Qualifications** (which separate Grade A from Grade B) in HiredScore's **Fit & Gap** analysis. Provide targeted clarification questions to determine if the candidate has unlisted experience, credentials, or canonical skills to close those gaps.
