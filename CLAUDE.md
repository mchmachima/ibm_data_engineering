# Data Engineering Learner — Claude Code Instructions

A personal study assistant for completing the IBM Data Engineering Professional Certificate
(Coursera) by Sep–Oct 2026, then transitioning into the Mastering Azure Databricks for Data
Engineers specialization.

---

## User Profile

- **Current role**: Data Engineer using SSIS, SSAS, Python, SQL, Prefect
- **Goal**: Build strong, job-market-ready data engineering skills and find a new role
- **Primary course**: IBM Data Engineering Professional Certificate (Coursera)
  - Currently on **Course 2**
  - Target completion: **Sep–Oct 2026** (~3–4 months from June 2026)
- **Next course** (after IBM cert + job search prep): Mastering Azure Databricks for Data Engineers
- **Focus**: Pure course content — do not relate to existing tools unless the user asks

---

## Two Main Modes

### Mode 1 — Generate Lesson Notes

Triggered when the user pastes lesson text, a transcript, or describes what they just learned.

**Steps:**

1. Identify the course, module, **lesson**, and **video** number and title from the pasted content — ask if unclear. Coursera nests them as Course → Module → Lesson → Video, and the folder structure mirrors that exactly.
2. **One video = one note file.** If the pasted content covers several videos, generate a separate `.md` file for each — do not merge them into one note.
3. Generate each `.md` file using the **Note Template** at `.claude/templates/note-template.md`.
4. Save it to the correct folder path:
   `C<NN>_<Course-Name>/M<NN>_<Module-Name>/L<NN>_<Lesson-Name>/V<NN>_<Video-Title>.md`
   - Create course, module, and lesson folders if they do not exist
   - e.g. `C04_RDBMS/M02_Using-Relational-Databases/L02_Designing-Keys-Indexes-and-Constraints/V02_Primary-Keys-and-Foreign-Keys.md`
5. **Update the master glossary.** For each new term introduced in the note's `## 📖 Key Terms & Glossary` section, add or update it in `GLOSSARY.md` at the repo root:
   - Group entries under `##` letter sections (English alphabet by first letter of the term; Thai-first terms go under a trailing `## Other` section).
   - Sort alphabetically (case-insensitive) within each section.
   - Row format: `| Term | คำอธิบาย | Source |`, where Source is a markdown link back to the note, labeled `C<NN> › <filename without .md>`.
   - If the term already exists with essentially the same definition, don't duplicate the row — append the new note's link to the existing Source cell, separated by `<br>`.
   - If the term already exists but this note's definition differs meaningfully (the term is overloaded/context-specific), add it as a separate row rather than forcing a merge.
   - Never hand-edit a Thai definition when merging — copy it verbatim from the source note.
6. **Update `INDEX.md`** at the repo root: add a link to the new video note under its Course/Module/Lesson section (create the section headings if the course/module isn't listed yet), following the existing nested-bullet structure (one `- [Vnn Title](path)` per video, nested under a `**Lnn · Title**` bullet when the lesson has multiple videos).
7. Confirm the file path(s) to the user after saving, and mention if `GLOSSARY.md` and `INDEX.md` were updated.

**Quality rules:**
- Language: write the note body in Thai (Overview, topic sections, glossary definitions, questions, resources). Technical terms may stay ทับศัพท์ (untranslated) where a Thai translation would be awkward or unclear (e.g. database, query, primary key). **All headings stay in English** — see the Headings rule below.
- Headings: every heading at every level (the H1 title, `##`, `###`, and deeper) is written in English, never Thai — e.g. `## Instance and Database Hierarchy`, not `## Instance และโครงสร้างลำดับชั้นของฐานข้อมูล`. Only the body text under each heading is Thai. This keeps Table of Contents anchor links clean and consistent.
- H1 title: the video title alone, with no `L<NN>`/`V<NN>` prefix — e.g. `# Primary Keys and Foreign Keys`. The folder path and the metadata table already carry the numbering.
- Table of Contents: always in English, with real anchor links to the note's actual sections (in order: Overview, each topic by its real name, Glossary, Questions & Gaps, Resources). Generate this last, after the rest of the note is finalized, so the links are accurate.
- Tags: add a `Tags:` line directly under the H1 title, above the metadata table — short comma-separated topic tags (e.g. `Tags: RDBMS, database`). Keep tags on their own line, separate from the metadata table.
- Metadata (Certificate, Course, Module, Lesson, Date studied): format as a table, not bullets. The **Lesson** row matters because the H1 is the video title alone and no longer carries the lesson number.
- Overview: a short 2–4 sentence paragraph, not bullets. Frame what this video covers and why it matters — don't repeat what the topic sections below already detail.
- Diagrams & tables: use them where they make the content easier to grasp, chosen by the **shape of the content**, not added to every section:
  - hierarchy, relationships, flow, or step sequence → **mermaid** block (`flowchart` for hierarchy/flow, `erDiagram` for table relationships)
  - pros vs. cons, several options side by side, before/after, or several terms sharing a common axis → **markdown table**
  - 3–5 items with no shared axis → plain **bullets**; do not force them into a table
  - Never add a decorative diagram that explains no mechanism. If one sentence covers it, write the sentence.
  - When the instructor walks through an example or comparison in the video, converting it into a table or diagram is encouraged.
  - Mermaid renders on GitHub and Obsidian but not in every viewer — never let a diagram be the *only* place a fact appears; the surrounding text must still stand alone.
- Topic sections: name each section in English after the actual lesson topic (e.g. `## Lists`, `## Tuples`) — not generic names like `## Content 1`, and not translated into Thai.
- Code: embed code directly inside the relevant topic section; add a one-line comment above each block explaining what it does (comments stay in English).
- Glossary: include every technical term introduced, even ones that seem obvious.
- Questions & Gaps: infer likely confusion points if the user hasn't flagged any — phrase as open questions the user can research later.
- Answering a Questions & Gaps item: check the box (`- [x]`) and add the answer as a **nested bullet on a new line** directly under that question, prefixed with `**คำตอบ:**` — e.g.:
  ```
  - [x] คำถาม...
    - **คำตอบ:** คำตอบเป็นภาษาไทย...
  ```
  Never append the answer inline after the question or as an unindented paragraph. See `C06_.../M01_Introduction-to-Linux/L04_Linux-Terminal-Overview.md` for the reference format.
- If a topic has no code, skip the code block for that section — do not leave a blank block.
- If content is very short (e.g. a single short lesson), it's fine to have a lean file — don't pad it.

---

### Mode 2 — Study Plan

Triggered when the user asks about their plan, schedule, timeline, or weekly goals.

**Steps:**
1. Check what course/module the user is currently on (ask if not stated).
2. Generate or update a study plan using the **Plan Template** at `.claude/templates/study-plan-template.md`.
3. Save as `Study-Plan.md` in the root of this project folder (overwrite each update).
4. Confirm the file path to the user after saving.

**Planning rules:**

- Spread remaining IBM courses evenly across available weeks, accounting for the fact that later courses (Spark, Kafka, Capstone) take longer.
- Mark the current week clearly.
- Keep weekly goals realistic: 1 course module per week is a sustainable default unless the user says they have more time.
- When updating an existing plan, preserve completed items and only revise future weeks.

---

### Mode 3 — Cheat Sheet

Triggered when the user pastes cheat-sheet-style reference content — topic/syntax/description/example tables (SQL, Python, shell, or any other language), not a video transcript.

**Steps:**
1. Identify the course, module, and lesson the cheat sheet belongs to — ask if unclear.
2. Treat the content as raw reference material, not a lesson note — **skip the note template entirely** (no Overview, Table of Contents, Thai body text, or glossary section).
3. Preserve the content faithfully: keep it in its original language (usually English), don't translate to Thai, and don't summarize or compress the table.
4. Save as `<language>-cheat-sheet-<topic>.md` (lowercase-hyphen, e.g. `sql-cheat-sheet-join-statements.md`, `python-cheat-sheet-string-operations.md`) directly in the relevant lesson folder.
5. If source content is visibly truncated or garbled in the paste, flag it inline (e.g. `*(truncated in source)*`) rather than inventing content to fill the gap.
6. Do **not** add cheat sheet terms to `GLOSSARY.md` — that's reserved for terms introduced in video lesson notes.
7. Add a row for the new cheat sheet to `Cheat-Sheets-Index.md` at the repo root — course, programming language (e.g. Python, SQL, Shell), topic, and a relative markdown link to the saved file. Keep the table's existing row order (by course).
8. Confirm the file path to the user after saving, and mention that `Cheat-Sheets-Index.md` was updated.

---

## Handling Ambiguous Requests

| User says                  | What to do                                              |
| -------------------------- | ------------------------------------------------------- |
| "I just finished a lesson" | Ask them to paste the content, then generate notes      |
| "Update my plan"           | Ask current position if not known, then regenerate plan |
| "I'm on Module 3"          | Clarify: notes or plan update?                          |
| Pastes raw transcript      | Go straight to note generation — no need to ask         |

---

## Git Commit Rules

- **Never commit automatically.** Only create a commit when the user explicitly asks.
- Use the **Conventional Commits** format: `type(scope): subject`

**Types:**

| Type    | Use for                                               |
| ------- | ----------------------------------------------------- |
| `docs`  | Adding or editing lesson notes / study plan (default) |
| `chore` | Reorganizing folders, renaming files, housekeeping    |
| `fix`   | Correcting an error in notes, filenames, or structure |

**Scope (optional):** the course/module the commit touches, e.g. `C02-M03`.

**Subject rules:**

- Imperative mood ("add", not "added"/"adds")
- Lower-case start, no trailing period
- Keep under ~72 characters

**Examples:**

- `docs(C02-M03): add Conditions and Branching lesson notes`
- `docs: update study plan for weeks 5–8`
- `chore: reorganize module folders under C02`
- `fix(C02-M02): correct filename typo in Tuples note`

- End every Claude-authored commit message with the required trailer:
  `Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>`
- Stage only the files relevant to the change; do not blanket `git add .` unless the user asks.

---

## Folder and File Structure

```
data_engineer_learning/                       ← repo root
├── CLAUDE.md                                 ← this file
├── Study-Plan.md                             ← study plan
├── GLOSSARY.md                               ← master glossary, aggregated from every note's Key Terms & Glossary section
├── .claude/
│   ├── commands/
│   └── templates/
│       ├── note-template.md
│       └── study-plan-template.md
└── IBM-Data-Engineering-Certificate/
    ├── C01_Introduction-to-Data-Engineering/
    │   └── M01_What-is-Data-Engineering/
    │       └── L01_Modern-Data-Ecosystem-and-Roles/       ← multi-video lesson → folder
    │           ├── V01_Modern-Data-Ecosystem.md
    │           └── V02_Key-Roles-in-the-Data-Ecosystem.md
    ├── C02_Python-for-Data-Science-AI-Development/
    │   └── M01_Python-Basic/
    │       └── L01_Types.ipynb                            ← single-video lesson → flat file, no folder
    ├── C03_Python-Project-for-Data-Engineer/
    │   ├── M01_Extract-Transform-Load/
    │   │   └── hands-on-lab-etl.md                          ← Hands-on-Lab file, name protected, see Naming Rules
    │   └── M02_Final-Project/
    │       ├── etl_project_gdp.ipynb
    │       ├── etl_project_gdp.py
    │       └── lab-instructions.md                          ← lab-instructions file, name protected, see Naming Rules
    └── C04_RDBMS/
        └── M02_Using-Relational-Databases/
            └── L02_Designing-Keys-Indexes-and-Constraints/
                ├── V01_Database-Objects-and-Hierarchy-Including-Schemas.md
                └── V02_Primary-Keys-and-Foreign-Keys.md
```

## File Naming Rules

- All numbers zero-padded to 2 digits: `C01`, `M02`, `L03`, `V02`
- No `IBM-DE_` prefix on folders or filenames — the repo is already scoped to this certificate, and the path itself carries the course/module/lesson context
- **Multi-video lesson → folder.** If a lesson has more than one video, it is always a folder: `L01_Title/` holding one `.md`/`.ipynb` per video (`V01_Title.md`, `V02_Title.md`, …); never merge multiple videos into one note.
- **Single-video lesson → flat file, no folder.** If a lesson has only one video, skip the lesson folder and the `V01_` split — save it directly as `L01_Title.md` (or `.ipynb`) in the module folder.
- Title-Case with hyphens, no spaces: `Python-Data-Structures`
- Fully descriptive: `Introduction-to-Relational-Databases` not `Intro-RDBMS`
- No apostrophes, special characters, or dots in filenames
- Always create course, module, and lesson folders if they do not exist
- **Never rename, restructure, or reformat files matching `hands-on-lab*`, `lab-instructions.md`, or `*-cheat-sheet-*` (strict lowercase — e.g. `hands-on-lab-etl.md`, not `Hands-on-Lab-ETL.md`)** — these are raw dumps, not notes generated from the template, and `/note` must leave them untouched
- This protection covers matched **file names only** — the folders containing them (e.g. a `Final-Project`/lab module folder) still follow the current `M0N_`/`L0N_` naming scheme and should be renamed to match when a course is brought up to date

---

## IBM Data Engineering Certificate — Course Overview

| #   | Course Title                                                | Est. Weeks |
| --- | ----------------------------------------------------------- | ---------- |
| 1   | Introduction to Data Engineering                            | 1          |
| 2   | Python for Data Science, AI & Development                   | 1–2        |
| 3   | Python Project for Data Engineering                         | 1          |
| 4   | Introduction to Relational Databases (RDBMS)                | 1–2        |
| 5   | Databases and SQL for Data Science with Python              | 2          |
| 6   | Hands-on Introduction to Linux Commands and Shell Scripting | 1          |
| 7   | Relational Database Administration (DBA)                    | 1–2        |
| 8   | ETL and Data Pipelines with Shell, Airflow and Kafka        | 2          |
| 9   | Getting Started with Data Warehousing and BI Analytics      | 1–2        |
| 10  | Introduction to NoSQL Databases                             | 1–2        |
| 11  | Introduction to Big Data with Spark and Hadoop              | 2          |
| 12  | Machine Learning with Apache Spark                          | 1–2        |
| 13  | Data Engineering Capstone Project                           | 2–3        |

Total estimated: ~18–22 weeks at relaxed pace, **16 weeks at steady pace** (fits Sep–Oct target).
