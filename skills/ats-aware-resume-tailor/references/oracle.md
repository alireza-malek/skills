# Oracle Recruiting Cloud (ORC) ATS Reference Guide

## 1. System Profile & Recruiter Review Interface

* **Core Architecture (Redwood Experience 25A–26B):** Operates on Oracle Fusion Cloud Recruiting's **Redwood Experience** (`fa-hcm-jobapplication`, `ORA_IRC_*`), evaluating candidates across a dual architecture: **parsed resume attachments** (which drive Generative AI Job Application Ratings, GenAI Profile Summaries, and attachment keyword searches) and **structured Profile Content Blocks** (which are the *only* data sent to `Suggested Candidates` Intelligent Matching).


* **Redwood Job Applications List & Details Page:** Recruiters filter applicants using interactive filter chips, view custom **Applicant Ratings** (AI scores) and prescreening scores in the grid, and open the `Details` tab where all sections expand by default and the **candidate's resume and attachments are pinned directly at the top**.


* **Quick Move in Attachment Preview (25C):** Recruiters do not need to exit the resume to make a triage decision; the **Attachment Preview Panel** includes built-in **Move Forward** and **Reject** action buttons that immediately transition the application through the Candidate Selection Process.



---

## 2. Parsing Engine & Technical File Constraints

* **RChilli & Partner Parsing Engines:** ORC uses **RChilli** (automatically provisioned in Oracle Recruiting Booster) or Oracle Cloud Marketplace parsing partners to extract resume text. If a resume contains no parseable text (such as a scanned image PDF or broken multi-column table), the Generative AI scoring engine returns **`No data for scoring`**.


* **The 6 Profile Content Blocks:** During the external Apply Flow, parsed data populates structured **Profile Content Blocks** tied to Oracle Cloud HCM's **Person Profile Type**:
1. **Education** (`School Name`, `Degree`, `Major`)


2. **Experience** (`Employer Name`, `Job Title`, `Dates`, `Work Summary`)


3. **Skills**

4. **Licenses and Certifications**

5. **Languages**

6. **Work Preferences**


(Employers can configure multi-section content blocks within the same application flow, such as separate sections for Bachelor Degree and Doctoral Degree, and all block data copies into the permanent HCM worker profile upon hire).




* **Candidate-Facing Apply Flow AI:** Career sites may display: (1) **Recommended Jobs Based on Candidate Resume** (weighted on parsed `current job title`, `location`, and `skills`), (2) **The Job Fit Metric** (`ORA_IRC_CE_JOB_FIT_GAI_ENABLED`, rating suitability on a **0–5 scale** across skills, job history, and education with a written explanation), (3) **AI Agent for Candidate Experience / Career Coach (25D)**, and (4) **Dynamic Skills & Skills Advisor** (interactive AI-suggested skill pills during application).



---

## 3. Scoring, Dual AI Engines & Automated Gatekeepers

### Engine 1: Redwood Job Application AI Rating (Generative AI Scoring, 25B–26B)

* **Four Rating Columns & Custom Weighting (`Sum = 1.0`):** Uses Generative AI (`ORA_IRC_APPLICANT_AI_SCORES_FEATURE_ENABLED`) to compare the parsed application against the job requisition across four columns:
1. **Profile Rating** (functions as the **Overall Rating** in Search Views and Sort menus)


2. **Education Rating**

3. **Experience Rating**

4. **Skill Rating**


Administrators configure the exact mathematical weights of **Education**, **Skill**, and **Experience** such that their **sum equals `1.0**` (25C).




* **Generative AI Profile Summary Panel & Queue Sorting:** Clicking any rating bar opens a **Profile summary panel** generated strictly from **the resume information that was parsed**. Recruiters filter by rating thresholds (25C) and sort the entire applicant queue from **High to Low** by Profile Rating, Education Rating, Experience Rating, or Skills Rating (26B).



### Engine 2: Oracle AI Apps `Suggested Candidates` (Intelligent Matching)

* **Structured-Fields-Only Rule (Critical Constraint):** `Suggested Candidates` uses NLP machine learning to display match strength across **Profile, Education, Experience, and Skill** (alongside `Similar Candidates` ranking up to 100 profiles and 26B **Autonomous AI Sourcing Agents** building Candidate Pools). Crucially, Oracle officially documents that **`Suggested Candidates` never receives a copy of a candidate's resume or PII**—it evaluates *only* the subset of structured profile fields pushed from ORC (in English).



### Disqualification Questions, Scored Prescreening & CSP Fast Formulas

* **Disqualification Questions (Negative Score Knockout):** Auto-populated into the prescreening questionnaire based on requisition context; selecting a disqualifying answer assigns a **negative score** that immediately moves the application to **`Rejected by Employer`** with the system reason: `The job application was disqualified`.


* **Overall Score of Prescreening Questions:** Non-disqualifying scored questions sum to an **Overall Score of Prescreening Questions**, which recruiters sort from `Score - High to Low` and filter by specific Q&A pairs (25C).


* **Candidate Selection Process (CSP) Fast Formulas:** Automated move conditions and custom **Fast Formulas** (`CONDITION_RESULT = 'Y'`) automatically transition phases/states, calculate **Rank Scores**, or trigger **AI Matching Ratings**.



---

## 4. Search & Database Discovery Mechanics

* **Oracle Search (`fa-hcm-jobapplication`, 15-Minute Indexing):** Keyword search queries across **Resume Attachments**, **Job Title**, **Employer Name**, **Work Summary**, **Skill**, **Degree**, **Major**, **School Name**, **License**, **Candidate Name**, **Candidate Number**, and **Language**.


* **Strict Boolean Search Rules in ORC (Critical Rules):**
  1. **Supported Operators:** Only uppercase/English **`AND`**, **`OR`**, **`NOT`**, and single-level parentheses `()` are supported.


  2. **Implicit Exact-Phrase Rule:** In ORC, **anything placed between Boolean operators is automatically treated as a full quoted exact phrase** (e.g., `business analyst AND marketing manager` is treated identically to `"business analyst" AND "marketing manager"`).


  3. **No Wildcards (`*`) & No Nested Parentheses:** ORC **does not support wildcard searches (`*`)** and **does not support multi-level nested parentheses** such as `(manager AND (analyst OR associate))`.




* **Generative AI Natural Language Search (26B):** Converts natural-language recruiter prompts into structured UI filter chips and automatically populates multiple synonyms.



---

## 5. Resume Engineering Playbook for Oracle (ORC)

### RChilli Parser & "Quick Move in Attachment Preview" Architecture

* Submit a clean, single-column selectable-text **`.docx` or `.pdf**` with zero tables, text boxes, or graphic headers so RChilli/ORC parsing succeeds and never returns `No data for scoring`.


* Map section headings directly to ORC's 6 Profile Content Blocks: `Professional Experience` (or `Work Experience`), `Education`, `Skills`, `Licenses and Certifications`, and `Languages`.


* **Top-Third Visual Hierarchy for 25C Attachment Preview:** Because recruiters have **Move Forward** and **Reject** buttons directly inside the **Attachment Preview Panel**, place a metric-driven **Executive Summary / Work Summary** and **Core Skills** block in the top third of Page 1.



### Optimizing for Weighted GenAI Ratings (`Sum = 1.0`) & ORC Boolean Syntax

* **Maximize All Three Weighted Pillars (`Education`, `Experience`, `Skill`):**
* **Education Rating:** Explicitly spell out `Degree` (e.g., *Bachelor of Science*), `Major` (*Computer Science*), and full `School Name`.


* **Experience Rating:** Use standard industry `Job Titles` (append target titles in parentheses after internal titles), clear `MM/YYYY – MM/YYYY` (or `Present`) dates, and comprehensive `Work Summary` achievement bullets.


* **Skill Rating:** Mirror exact requisition skills in both the dedicated `Skills` section and `Experience` bullets.




* **Keep Multi-Word Phrases Intact & Adjacent:** Because ORC treats all words between `AND`/`OR`/`NOT` as an exact quoted string, write standard multi-word terms together intact (e.g., write *"Supply Chain Management"* rather than *"Managed the supply chain"*).


* **Include Morphological Variations & Acronym Pairs:** Because ORC **does not support `*` wildcards**, naturally incorporate both noun and verb/adjective forms (e.g., *automated* and *automation*) alongside full terms and acronyms (`Search Engine Optimization (SEO)`, `Certified Public Accountant (CPA)`).



---

## 6. Portal Submission & Form Gatekeepers

* **Verify Every Structured Profile Content Block on the Portal:** Because **`Suggested Candidates` never sees your raw resume file** and evaluates only structured profile fields, manually verify and complete every auto-populated block (`Experience`, `Education` [including multi-section degree blocks], `Skills`, `Licenses`) on the web form before submitting.


* **Click Interactive "Skills Advisor" Pills & Pre-Test with "Job Fit Metric":** Select all relevant **Skills Advisor** pills during the Apply Flow to normalize skills against the requisition's library, and use the career site's **Job Fit Metric (0–5)** or **AI Career Coach (25D)** to audit your draft before final submission.


* **Treat Prescreening Questions as Scored Gates:** Avoid negative-score **Disqualification Question** answers (`The job application was disqualified`) and select the highest truthful proficiency tiers on scored prescreening questions to maximize your **Overall Score of Prescreening Questions**.



---

## 7. Output Specifications for this ATS

### Target Evaluation Metric Display

**Redwood GenAI Profile Rating:** High Alignment Across Weighted Pillars (`Education` + `Experience` + `Skill` = 1.0)
**Oracle Search (`fa-hcm-jobapplication`):** 100% Implicit Exact-Phrase & Non-Wildcard Morphological Coverage
**Content Block & Prescreening Status:** Verified 6 Profile Content Blocks (`Suggested Candidates` Ready) & Disqualifier Safe

### Remaining Gaps Analysis

Identify any missing qualifications across the three weighted **Job Application AI Rating** pillars (`Education`, `Experience`, `Skill`), any split multi-word phrases or missing word variations that would fail ORC's **implicit exact-phrase / non-wildcard Boolean search**, or any mandatory eligibility criteria tied to **Disqualification Questions**. Provide targeted clarification prompts to bridge those gaps safely.