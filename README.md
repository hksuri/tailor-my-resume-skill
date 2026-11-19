# tailor-resume: a Claude Code skill

Give it a job posting. It reads your resume the way ATS software and a skeptical hiring manager would, tells you honestly whether you should apply, and builds a tailored one-page LaTeX resume.

It **never invents experience**. Every line it writes comes from a knowledge bank that you write yourself. Keywords you can't back up show up as gaps in the report. They never get added to your bullets.

## What you get

For each job posting, the skill writes an `Applications/<Company>-<Role>-<Date>/` folder containing:

- **`gap_analysis.md`**, which has three parts:
  - **Gap analysis**: the top 20 keywords, which are missing or weak, likely auto-reject reasons, and ready-to-use rewritten bullets.
  - **Apply verdict**: *Strong apply*, *Worth applying*, *Long shot but defensible* or *Skip this one*, with the reasoning behind it.
  - **General resume tips** that apply beyond this posting.
- **A tailored `.tex` and compiled `.pdf`**, checked to fit on one page and inspected visually for formatting problems.

## Setup

### Prerequisites
- [Claude Code](https://docs.claude.com/en/docs/claude-code)
- A TeX distribution that includes `pdflatex` (MacTeX, TeX Live or MiKTeX)
- Optional: `poppler` (for `pdftoppm` and `pdfinfo`). On macOS, the built-in `qlmanage` is enough.

### 1. Get your own LaTeX resume template
This repo doesn't include a resume template. Find one you like and fill it in with your own details. A good starting point is **Jake's Resume**: it's clean, single-column, ATS-friendly and MIT-licensed.
- Overleaf: <https://www.overleaf.com/latex/templates/jakes-resume/syzfjbzwjncs>
- Source: <https://github.com/jakegut/resume>
- Original it's based on: <https://github.com/sb2nov/resume>

Any one-page LaTeX resume works. Save yours as **`resume_template.tex`** in the folder you'll work from (your "resume folder").

### 2. Write your knowledge bank
Create **`knowledge_bank.md`** in the same folder. It's a file you write yourself, holding your true professional history: roles, dates, what you actually did, real metrics, skills and education. It's the skill's only source of facts. If something isn't in it, it won't appear on your resume. Build it yourself. The more complete and honest it is, the better the output.

### 3. Install the skill
```bash
git clone https://github.com/hksuri/tailor-my-resume-skill.git ~/.claude/skills/tailor-resume
```

### 4. Run it
Start Claude Code from your resume folder and run:
```
/tailor-resume https://example.com/careers/some-job-posting
```
You can also paste the posting text instead of a link.

Your resume folder should look like this:
```
my-resume/
├── resume_template.tex
├── knowledge_bank.md
└── Applications/          ← created by the skill
```
Keep your personal files in your resume folder, not inside the skill's folder, so they can never end up in a fork or pull request.

## Sample output (fictional)

This is an excerpt of `gap_analysis.md` for an invented candidate, *Jordan Lee*, a marketing manager applying to a made-up *Growth Marketing Lead* role at *Northwind Outdoor*.

> **Verdict: Long shot but defensible.** The posting asks for 7+ years and ownership of a $2M paid-acquisition budget. Jordan has 5 years and has managed at most $400K, so the role is pitched about one level above them. Jordan's lifecycle-email results are a genuine match for the posting's retention focus. It becomes a *Strong apply* if Jordan can point to owning a budget of seven figures or close to it.
>
> **Missing keywords:** incrementality testing · marketing mix modeling · Braze
>
> **Rewrite:**
> - Before: *Responsible for email campaigns that significantly improved engagement.*
> - After: *Rebuilt the onboarding email sequence around purchase-intent segments, lifting 30-day repeat-purchase rate by **18%**.*

## License
MIT. See [LICENSE](LICENSE).
