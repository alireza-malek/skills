# BambooHR ATS Reference Guide

## 1. System Profile & Recruiter Review Interface

- **Core Philosophy:** Built for small-to-medium-sized businesses (SMBs, typically 25 to 1,000 employees) around a **"human touch"** review philosophy that prioritizes human evaluation and cross-role fit over automated buzzword-weeding, with a benchmark response time of two business days.
- **No Native Autonomous AI Resume Scoring:** BambooHR does not natively score or rank resumes against job criteria. While **Bamboo AI™** supports conversational HR policy assistance, workforce reporting, burnout detection, and workflow recommendations, automated resume parsing or AI candidate matching only occurs when an employer connects third-party API/Marketplace bolt-ons (**Skima AI**, **Parseur**, **CandidateZip**, or **Taskade AI Kits**).
- **Hiring Team Roles & Guaranteed Resume Visibility:** Every job opening is managed by a **Job Creator** (who receives email alerts and a non-disableable **"What's Happening"** widget alert), a **Hiring Lead**, and **Collaborators**. Because the collaborator permissions setting asks **_"Besides resume, what info can collaborators view?"_**, **every reviewer added to the hiring process is guaranteed to see the uploaded resume**, even if compensation, address, or custom answers are restricted.
- **Desktop View vs. "BambooHR Hiring" Mobile App:** On desktop, reviewers scan the Candidate Overview Table (`Name`, `Most Recent Star Rating (1–5 stars)`, `Application Date`, `Current Status`) and the in-browser document viewer. On smartphones, email alerts open the **BambooHR Hiring** mobile app (iOS/Android), where unreviewed applicants show an unread dot badge, reviewers land on the **Info tab** first (Application Date, Star Rating, Quick Contact buttons, Status, Custom Question count, Basic Info), and opening the resume requires reviewers to **"Pinch to zoom in"**.

---

## 2. Parsing Engine & Technical File Constraints

- **Supported Formats & 5 MB Ceiling:** The resume upload field accepts **`.docx`, `.pdf`, `.doc`, `.rtf`, and `.txt**` files up to a **5 MB** maximum file size limit.
- **The Three Resume Processing Paths:** Depending on employer setup, resumes follow one of three paths: (1) **Direct Human Document Review** (rendered in the desktop browser viewer or mobile app), (2) **Form-First Applications** (manual form entry with the resume as a secondary attachment), or (3) **Third-Party Parser / AI Bolt-On Path** (`Parseur`, `CandidateZip`, `Skima AI` pushing structured data via the `POST Create Candidate` REST API endpoint).
- **Single-Column vs. Two-Column Parser Retention Benchmarks:** Benchmark testing on BambooHR bolt-on parser workflows shows that **single-column `.docx` files retain 96% of work-history fields**, whereas **two-column PDFs retain only 65% to 80%** and force mobile reviewers to pinch and pan horizontally across columns.
- **No Text Boxes & Public LinkedIn Photo Auto-Import:** Floating text boxes and graphic headers cause contact details to be cropped on mobile candidate cards or skipped by bolt-on parsers. Additionally, if a candidate provides their LinkedIn URL on the application form and their LinkedIn profile photo is set to public, **BambooHR automatically pulls that photo into their ATS Candidate Profile**.

---

## 3. Evaluation, Triage & Intake Gatekeepers

- **1–5 Star Rating & Queue Sorting:** BambooHR's native ATS does not run automated boolean knockout rules on custom questions to silently auto-reject applicants on submission. Reviewers manually evaluate custom questions, assign a **1-to-5 Star Rating**, filter by **Active** or **Disqualified** status, and sort the applicant list by five attributes: **Application Date, Last Update, Name, Rating, or Status**.
- **Delayed Rejection Emails ("Send Later"):** Recruiters can send emails immediately (`Send Now`) or schedule them (`Send Later` with a 1-hour buffer), which dispatch automatically regardless of subsequent candidate or job status changes unless manually canceled in the `Scheduled` tab.
- **The External Job Board Intake Trap & Mandatory Confirmation Email:** BambooHR warns that native _"Apply Here"_ buttons on non-partner job boards often fail to route submissions into BambooHR's ATS. Conversely, BambooHR automatically sends an **Application Confirmation Email** upon submission that admins **cannot disable or delete**, serving as definitive proof that the application entered the ATS.

---

## 4. Search & Database Discovery Mechanics

- **Manual Keyword Search:** Recruiters search applicants using the manual text search bar in the upper-right desktop interface, mobile Home screen, and global `Candidates` tab. Unsearchable scanned image PDFs are invisible to text searches if no third-party parser is attached.
- **Talent Pools & Cross-Role Movement:** Because native BambooHR does not auto-match past applicants to new openings on its own, recruiters manually organize strong candidates into skill-based **Talent Pools** (`Talent Pools` tab) or use **`Move a candidate to a different job opening`** when custom questions and resume review reveal fit for another open position.

---

## 5. Resume Engineering Playbook for BambooHR

### Mobile "Pinch-to-Zoom" & Bolt-On Parser Formatting

- Submit a clean, **single-column `.docx` or selectable-text `.pdf` under 5 MB** (1 page, or 2 pages for 8+ years of experience) to ensure 96% field retention on bolt-on parsers and effortless vertical scrolling on the **BambooHR Hiring** mobile app.
- Place all contact details as standard inline body text at the very top of Page 1 (never inside text boxes): Full Name, Email, Phone, **City & State** (essential for SMB geographic proximity filtering), and **LinkedIn URL**. Omit headshots on the resume document itself while ensuring your public LinkedIn profile photo is polished.

### Designing the "Top Third" & Results-First Bullets for 5-Star Triage

- **Top-Third "Above the Fold" Hierarchy:** Place your current/target **Job Title**, **Employer**, and a concise **1–2 line Role-Specific Summary** in the top third of Page 1 immediately below the contact block so reviewers can assign a high Star Rating within seconds.
- **Standard Section Headings:** Use predictable labels (`Summary`, `Experience` or `Professional Experience`, `Education`, `Skills`, `Certifications`).
- **Lead Bullets with Results First, Actions Second:** Because every collaborator on the hiring team reads your resume, front-load measurable business impact before describing actions and tools, and mirror exact job description terminology for manual text searches.

---

## 6. Portal Submission & Form Gatekeepers

- **Apply Only via the Direct BambooHR URL:** Always apply directly on the company's `*.bamboohr.com` link or embedded careers widget and verify receipt of the mandatory **Application Confirmation Email**.
- **Prioritize Custom Questions & the Mobile `Info` Tab:** Because reviewers on the **BambooHR Hiring** mobile app land on the **Info tab** (displaying Basic Information and Custom Application Questions) _before_ tapping to open your resume, write specific, high-value responses to every custom question and use the optional _"Tell us about yourself"_ box for a targeted 2–3 sentence pitch.

---

## 7. Output Specifications for this ATS

### Target Evaluation Metric Display

**Human Triage Rating Target:** [X]/5 Stars ──► 5/5 Stars (Desktop & Mobile "Pinch-to-Zoom" Optimized)
**Bolt-On Parser & Search Alignment:** 96% Single-Column Field Retention & Exact Manual Search Keyword Match
**Collaborator & Talent Pool Readiness:** Above-the-Fold Title Hierarchy + Cross-Role Skill Positioning

### Remaining Gaps Analysis

Identify any missing software tools, certifications, or core functional competencies from the job description that a recruiter would query in BambooHR's **manual keyword search bar** or evaluate when assigning a **1–5 Star Rating** and tagging candidates into **Talent Pools**. Ask targeted clarification questions to uncover measurable results or adjacent skills.
