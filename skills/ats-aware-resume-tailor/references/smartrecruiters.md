# SmartRecruiters ATS Reference Guide

## 1. System Profile & Recruiter Review Interface

- **Core Architecture:** Built on the SmartOS talent operating system, powered by the Winston AI intelligence layer (Winston Match, Winston Screen, Winston Companion).
- **The Dual-View Interface:** Recruiters work inside a split-screen workspace featuring a Native Resume Viewer (renders original PDF/DOCX layout) side-by-side with structured profile tabs: Overview, Screening, Reviews, Interview, Compensation, and Notes.
- **Review Pace:** High-speed triage driven by Winston Match star ratings and the Top Applicants tab. Recruiters scan the AI Match Summary before opening the full resume.
- **Critical Candidate-Verified Data Priority:** SmartRecruiters' algorithm explicitly prioritizes candidate-verified data entered directly into portal form fields over raw parsed text from the resume document. If a discrepancy exists, the portal input takes precedence.

---

## 2. Parsing Engine & Technical File Constraints

- **Supported File Formats:** Text-based `.docx` or `.pdf` exported directly from word processors. Scanned image PDFs fail extraction and garble the text stream.
- **Layout Sensitivity:** Single-column layout is mandatory. Two-column layouts, tables, text boxes, and sidebars cause the parser to read horizontally across columns, merging unrelated lines into nonsensical strings.
- **Header & Footer Isolation:** Standard ATS parsers strip out header and footer layers to prevent repeating text. Place all contact information within the main document body at the top of Page 1.
- **Section Header Mapping:** Conventional section headings must be used for reliable classification into database slots: `Work Experience` (or `Professional Experience`), `Education`, `Skills`, `Certifications`.

---

## 3. Scoring & AI Mechanics: "Winston Match" & "Winston Screen"

Winston Match evaluates candidates across three core pillars: Skills, Work Experience, and Education.

### Winston Match Score (1 to 5 Stars)

Every applicant receives an overall rating on a 1–5 star scale (e.g., `4/5 Stars`, `5/5 Stars`), mapped into four qualification bands:

- **Very High** (typically 5 Stars)
- **High** (typically 4 Stars)
- **Medium** (typically 3 Stars)
- **Low** (1 to 2 Stars)
  Recruiters filter and sort candidate queues directly by these star ratings and review dimension-level subscores (Skills, Experience, Education).

### ESCO Skills Taxonomy Normalization

SmartRecruiters does not use simple keyword matching. Natural Language Processing extracts skills and normalizes them against the standardized ESCO (European Skills, Competences, Qualifications, and Occupations) taxonomy:

1. **Technical Skills:** Hard domain skills, frameworks, and methodologies. The system classifies these as either **Essential** or **Preferred** based on the job requisition.
2. **Transferable Skills:** Foundational capabilities (Communication, Influencing, Critical Thinking, Project Planning, Cross-functional Leadership).

### Evidence Verification & Confidence Levels

For every detected skill, Winston Match records the evidence source and assigns a confidence level:

- `Verified in: Resume`: Explicitly detected in the uploaded CV.
- `Verified in: Default Career Page`: Confirmed via portal screening fields.
- `Verified in: Inferred`: Inferred based on job title, adjacent duties, and industry context.
- **Confidence Rating:** Marked as **High**, **Medium**, or **Low** based on whether the context, metrics, and tenure substantiate the skill.

### The AI Match Summary Panel

At the top of the candidate profile, Winston Match displays:

- **Relevance Assessment:** Highlights key relevant titles and past employers.
- **Identified Skill Gaps:** Explicitly flags required skills not verified in the profile.
- **Compensating Experience:** Identifies whether broader background or adjacent achievements compensate for flagged gaps.

### Winston Screen & "Top Applicants" View

Winston Screen analyzes job requirements to generate role-specific qualifying questions. Candidates who answer screening questions satisfactorily are shortlisted into the exclusive **Top Applicants** view.

---

## 4. Search & Database Discovery Mechanics

- **ESCO Normalization in Search:** Recruiters search skills using canonical ESCO titles. Non-standard jargon or company-specific nicknames fail semantic matching.
- **Winston Companion Integration:** Recruiters query candidates on the go via Slack, Microsoft Teams, and WhatsApp using conversational prompts (e.g., _"Show me top 5 candidates for Senior Backend Engineer"_), which retrieves candidate profiles ranked by Winston Match stars.

---

## 5. Resume Engineering Playbook for SmartRecruiters

### Structure & Layout

- Export as a selectable-text `.docx` or `.pdf`.
- Single-column top-to-bottom layout; no tables, floating text boxes, or icons.
- Contact block placed in the main body text: Name, Phone, Email, City/State/Country, LinkedIn URL.

### Title & Role Alignment

- Standardize job titles to recognized industry equivalents (e.g., replace _"Growth Hacker"_ with _"Growth Marketing Manager"_). If the internal title was quirky, use: `Official Title (Standardized Industry Title)`.
- Format dates as `MM/YYYY – MM/YYYY` or `Month YYYY – Month YYYY` (use `Present` for current roles). Ambiguous year-only dates hinder tenure calculations.

### Maximizing Winston Match Confidence

- **Group Skills into Dual Categories:** Create an explicit skills section divided into **Technical Skills / Core Competencies** and **Professional & Transferable Skills** to feed both ESCO categories directly.
- **Contextual Proof for Essential Skills:** Never leave hard skills isolated in a bare keyword list. Every critical technical skill must appear inside an accomplishment bullet supported by operational scale, tools used, and measurable outcomes to trigger **High Confidence** and `Verified in: Resume`.
- **Acronym + Spelled-Out Syntax:** Write out both versions on first mention: `Search Engine Optimization (SEO)`, `Customer Relationship Management (CRM)`, `Enterprise Resource Planning (ERP)`.
- **Scale & Geographic Scope:** For regional or global roles, explicitly mention regional scope (e.g., `EMEA`, `APAC`, `North America`, `Global`) and distributed team leadership to trigger high relevance in work history analysis.
- **Preempt Gaps with Compensating Context:** In the Professional Summary, highlight broad competencies that offset any missing niche requirements to trigger Winston's **compensating experience** logic.

---

## 6. Portal Submission & Form Gatekeepers

- **Form Field Audit:** Always review and complete every pre-filled form field on the SmartRecruiters application page. Because candidate-verified form inputs override parsed resume data, leaving portal fields blank or uncorrected degrades your profile data.
- **Pre-Screening Question Precision:** Questions regarding work authorization, certifications, language levels, and travel availability feed directly into Winston Screen. Clear, affirmative responses ensure placement into the recruiter's **Top Applicants** tab.

---

## 7. Output Specifications for this ATS

### Target Evaluation Metric Display

**Winston Match Score:** [X]/5 Stars ──► [Y]/5 Stars
**ESCO Taxonomy Alignment:** [Low/Medium/High] Confidence
**Winston Screen Shortlist:** Positioned for "Top Applicants" tab

### Remaining Gaps Analysis

Identify any missing ESCO Technical or Transferable skills flagged by the job posting. Frame clarification questions around related tools or past projects that could serve as **compensating experience** in the AI Match Summary.
