# Teamtailor ATS Reference Guide

## 1. System Profile & Recruiter Review Interface

- **Core Architecture:** Teamtailor combines deterministic stage automations (`Smart move` and `Request missing info` triggers), OpenAI GPT-powered **Teamtailor Co-pilot** features, and **Anonymous mode** blind hiring within a visual Kanban and side-by-side comparison workspace.
- **Kanban Board, Candidate Card Modal & 50-Candidate Compare View:** Every hiring pipeline starts at a fixed **`Inbox`** stage and ends at **`Hired`**, sortable by Co-pilot **Screening criteria**. Clicking a candidate opens a split-screen modal separating **Job data** (job questions/answers, job comments, non-CV application files) from **Candidate data** (general profile info, Connect documents, competencies, and the overarching profile resume), with keyboard shortcuts (`Ctrl + 1` through `Ctrl + 5`) and **Wide-screen mode**. Recruiters can select up to **50 candidates** in the **Compare candidates** matrix to sort side-by-side across _Job application rating, Candidate rating, Distance to job, Co-pilot Resume summary, Co-pilot Screening criteria, Job match score, Skills, Traits, Custom fields, and Application answers_.
- **Anonymous Mode (Blind Hiring & Automated Resume Redaction):** When enabled on a stage (commonly `Inbox`), candidates are assigned a randomized **Color + Animal alias** (e.g., _"Green Bear"_), profile PII is hidden, free-text mentions of name/email in cover letters or messages are replaced with `"masked in anonymous mode"`, and clicking `Show full resume` renders an **automatically redacted resume where the candidate's name, email, phone number, and any images are blacked out**.
- **Single Profile Resume & "Connect" Self-Service Editing:** Co-pilot always summarizes the **most recently uploaded resume** on the overarching Candidate Profile. While an application remains unprocessed in the **`Inbox`** stage, candidates can log into the **Connect dashboard** (`no-reply@companyname.teamtailor-mail.com`) and click the pen icon to replace or edit their Resume, Cover letter, Answers, and Additional files.

---

## 2. Parsing Engine, File Constraints & Anti-Manipulation Scanner

- **Supported File Formats:** Teamtailor's Co-pilot engine officially parses six document formats: **`.docx`, `.pdf`, `.pptx`, `.pages`, `.txt`, and `.rtf**`.
- **Enhanced Resume Parser & Auto-Competency Tagging:** Automatically extracts PII (`Profile image`, `First/Last name`, `Email`, `Phone`, `Candidate address`, `LinkedIn URL`), structures work and education history, and—when `Add matched skills and traits` is enabled—automatically tags any **Skills and traits** already configured in the company's Teamtailor account onto the profile as **`Candidate competencies`**.
- **Anti-Manipulation Scanner ("Scan for hidden instructions") (Critical Rule):** Co-pilot scans the underlying file structure for concealed text or prompt injections using **white-on-white text, very small font sizes, or transparent text**. Any detection places a prominent **warning banner on the candidate profile** (exposing the exact hidden text, how it was concealed, and why it was flagged) and records a permanent entry in the company's **audit log**.
- **5-Bullet Co-pilot Resume Summary & Visual Gap Timeline:** Co-pilot displays a PII-stripped **five-point bullet summary** at the top of the `Resume` section (hiding the full document behind `Show full resume`) alongside a visual **Resume timeline** that **automatically highlights periods where no employment or education is recorded**.

---

## 3. Automated Gatekeepers, Co-pilot Screening & Scorecard Math

### Pre-Submission Gatekeepers & Stage Triggers

- **Qualifying Questions & Silent Spam Shadow-Blocking:** Selecting `"No"` on a mandatory `Yes / No` **Qualifying Question** immediately blocks all remaining form inputs on screen and prevents submission. **Spam Filter Rules** matching flagged email domains or addresses either mark applications as `Spam` or silently **shadow-block** them (allowing the applicant to submit normally while hiding the application and messages from Teamtailor).
- **`Smart move` Stage Triggers (Entry-Time Evaluation Rule):** Automatically advances or routes candidates upon entering a stage based on: (1) **Keywords in `Resume text**`, (2) **Answers to questions**, (3) **Co-pilot `Screening criteria`status**, (4) **Location**, (5) **Tags**, (6) **Referral status**, or (7) **Job offer status**. Crucially, keyword, location, and question`Smart move`triggers evaluate **only at the exact moment the candidate enters the stage**—updating a resume after entering the stage will not fire a keyword-based`Smart move`.
- **Co-pilot `Request missing info` Trigger:** Automatically emails the candidate a dynamic follow-up questionnaire when screening criteria lack sufficient data (`Unknown`) or custom/personal fields are empty; submitting the form updates the profile, re-runs Co-pilot screening, and fires connected `Smart move` triggers.

### Co-pilot Candidate Screening (6 Data Sources)

- **Multi-Source Prompt Evaluation:** Evaluates custom or JD-suggested `Title` + `Prompt` criteria across **six sources simultaneously**: (1) **Resume text**, (2) **Cover letter**, (3) **Answers to job application questions**, (4) **Comments**, (5) **Candidate address**, and (6) **Candidate tags**.
- **Three-State Criterion Status:** Assigns each criterion **`✓` (`Meets criteria`)**, **`✕` (`Doesn't meet criteria`)**, or **`–` (`Unknown / Insufficient data`)** with written reasoning. Candidates cannot opt out of Co-pilot resume parsing, summaries, or screening.

### Dual Star Ratings & Weighted `Job Match Score` Math

- **Dual Star Reviews:** Tracks **Candidate review (Orange Stars, 1–5)** for overall company fit and **Job application review (Yellow Stars, 1–5)** for specific role fit.
- **Scorecard Conversion & The Unscored Skill Penalty:** On structured Scorecards, weighted **Skills** (blue) and **Traits** (yellow) are scored 1–5 (scores $\ge 3$ become permanent Candidate competencies) and converted into the **`Job match score`**:
  `Score 5 = 100% ──► Score 4 = 75% ──► Score 3 = 50% ──► Score 2 = 25% ──► Score 1 = 0%`

If any skill or trait on the scorecard is left **unscored**, Teamtailor automatically penalizes it as a **score of `1` (`0%` match)**.

---

## 4. Search & Database Discovery Mechanics

- **Field-Specific & Boolean Search Syntax:** Supports field prefixes (`resume_text:`, `cover_letters:`, `linkedin_profile:`, `pitch:`, `first_name:`, `last_name:`, `email:`, `phone:`, `comments:`, `reviews:`) and missing-field queries (`!_exists_:[field]`). **Without quotation marks, spaces between words act as Boolean `AND**`, while quotes (`"Marketing Manager"`) enforce exact phrase matching, alongside `OR`, `NOT`, and parentheses `( )`. Recruiters also filter and sort by **`Distance to job`\*\* (calculated from the Candidate address).
- **Co-pilot Candidate Suggestions (Two-Stage Database Rediscovery):** Populates the purple **Suggested** stage in two steps: Stage 1 selects a top 20 pool via Resume, Meeting summaries, Answers, Reviews, and Job match scores; Stage 2 conducts a deep check adding Job applications, Messages, Comments, Locations, and Inbox screening criteria (automatically excluding candidates with **no activity in the past year**, mismatched locations on non-remote roles, or missing GDPR future-jobs consent).

---

## 5. Resume Engineering Playbook for Teamtailor

### File Integrity, Anti-Manipulation & Redaction-Safe Layout

- Submit a clean, selectable-text **`.pdf` or `.docx**` with 100% visible, standard-sized typography; never use white, tiny, or transparent text that triggers the **Hidden Instruction Detection** warning banner.
- Isolate Name, Email, Phone, Address, and LinkedIn URL cleanly at the very top of Page 1 and omit profile photos on the resume so **Anonymous mode** redaction boxes never obscure adjacent summary or experience bullets.
- Include your full City/Address in the header and portal form to enable **`Distance to job`** sorting and avoid exclusion from Co-pilot `Candidate suggestions` on non-remote roles.

### Engineering for the 5-Bullet Summary & Zero Timeline Gaps

- **Front-Load a 4–5 Bullet Executive Highlights Block:** Place 4–5 metric-backed highlights at the top of Page 1 summarizing total tenure, domain expertise, core competencies, and top outcomes to directly feed Co-pilot's **5-point Resume summary** (viewed before the full CV and inside the 50-candidate comparison table).
- **Eliminate Unrecorded Date Gaps:** Use explicit `MM/YYYY – MM/YYYY` or `Month YYYY – Month YYYY` dates across all roles and education. Account for any career breaks (sabbaticals, independent consulting, caregiving, or continuing education) as formal timeline entries so Co-pilot's **Resume timeline** does not highlight them as unexplained gaps.

### Optimizing for Co-pilot Screening (`✓`), `Smart move` & Competencies

- **Embed Exact JD Skills & Traits on First Upload:** Because keyword-based `Smart move` triggers evaluate **only upon entering the stage**, ensure your initial uploaded resume contains the exact spelling of every hard skill and soft trait from the job ad—triggering automatic `Candidate competencies` tagging (`Add matched skills and traits`) and giving scorecard reviewers evidence for every weighted criterion so none default to `1 (0%)`.
- **Eliminate `–` (`Unknown`) Ratings Across All 6 Sources:** Explicitly verify every hard requirement (degrees, certifications, languages, tools, years of experience, location/relocation willingness) across your Resume, Cover letter, and Application question answers so Co-pilot awards **`✓` (`Meets criteria`)**.

---

## 6. Portal Submission & Form Gatekeepers

- **Pass Qualifying Questions & Respond to `Request missing info` Emails:** Verify mandatory `Yes / No` Qualifying Questions carefully, and immediately complete any automated follow-up questionnaire sent via the `Request missing info` trigger to trigger an automatic Co-pilot re-screen and `Smart move` advancement.
- **Use the `Connect` Dashboard for Early Edits:** If you spot an error after applying, log into the company's **Connect dashboard** while your application is still in the `Inbox` stage to replace your resume or edit question answers.

---

## 7. Output Specifications for this ATS

### Target Evaluation Metric Display

**Co-pilot Screening Criteria:** [X]% ──► 100% `✓ Meets criteria` (Zero `– Unknown` Flags)
**Job Match Score Projection:** [X]% ──► [Y]% (All Weighted Skills & Traits Substantiated; Zero `1 = 0%` Penalties)
**Timeline & Gatekeeper Status:** Zero Unexplained Timeline Gaps | Cleared `Smart move` & Hidden-Text Checks

### Remaining Gaps Analysis

Identify any job requirements, weighted Skills/Traits, or chronological date gaps that are missing or ambiguous across the resume—which would trigger a **`–` (`Unknown`)** rating in Co-pilot Screening, an unscored **`1 (0%)` penalty** on the Job Scorecard, or a highlighted gap on the **Co-pilot Resume timeline**. Provide targeted clarification questions so the candidate can address them on the resume, cover letter, or application questions.
