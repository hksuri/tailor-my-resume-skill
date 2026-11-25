---
name: tailor-my-resume
description: Given a job posting (link or pasted text), screen the user's resume the way ATS software and a hiring manager would, give an honest verdict on whether they should apply, then generate a tailored one-page LaTeX resume from their own template and knowledge bank. Use whenever the user wants to tailor their resume to a specific job, check resume–job fit, decide whether to apply, or asks to "run tailor-my-resume" / "/tailor-my-resume".
---

# Tailor My Resume

Turns a job posting into four things:
1. A blunt gap analysis, written the way ATS software and a hiring manager would read the resume.
2. An honest verdict on whether to apply for this specific posting.
3. General resume tips that apply to any application, not just this one.
4. A compiled, one-page, keyword-aligned LaTeX resume that's close to done and needs only minor edits.

The skill never invents experience. Every claim in the output must trace back to the user's knowledge bank.

## Inputs

Look for these in the user's current working directory:

- **`knowledge_bank.md`**: a file the user writes themselves. It holds their true professional history: roles, dates, what they actually did, real metrics, skills, education and anything else they might put on a resume. It's the **only source of facts** for this skill. If something isn't in it, it doesn't go on the resume. If the file is missing, stop and ask the user to create it. Don't try to reconstruct it from an existing resume or invent its contents.
- **`resume_template.tex`**: the user's own one-page LaTeX resume. It's the base for formatting and layout. If it's missing, stop and point the user to the setup steps in this skill's README.
- **A job posting**: a URL (fetch it) or pasted text. If neither is given, ask for one before continuing.
- Optional: the company and role name for naming the output folder (infer them from the posting if not stated), and any specific instructions about what to emphasize.

If the user has already tailored resumes in `Applications/`, start from the most recent one with the same job family and framing, not from the bare template. Recent resumes carry fixes the template may not have yet, such as spacing, confirmed metrics and better wording. Name the resume you started from in the summary.

## Workflow

### 1. Get the job posting
Fetch it if a URL was given, or use the pasted text directly.

**Multiple postings.** If the user sends more than one posting, say how many distinct resumes the set needs before doing any analysis. Explain which postings share a framing and which one is the outlier, then wait for the user to choose. If one posting matters more (for example, it comes with a referral), treat it as primary. Fold the others in only where they don't dilute it.

### 2. Extract the top ~20 keywords and requirements
Include hard skills, tools, domain terms, methods, and signals about seniority or scope. Separate must-haves from nice-to-haves.

### 3. Gap analysis (write this before touching the resume)
Compare the keyword list against `knowledge_bank.md` and the current template. Use exactly this structure:

1. **Top keywords, skills and requirements**: up to 20 distinct items from the posting, each marked Must or Nice. Don't bundle several requirements into one slot to reach 20. Report coverage against the real count.
2. **Missing keywords**: in the posting, absent from the knowledge bank.
3. **Weak or unclear matches**: present, but buried, vague or not quantified.
4. **Skills implied but not stated**: the knowledge bank has real evidence, but not in the posting's wording. Flag these as candidate additions, and don't add them silently.
5. **Phrasing too vague for ATS parsing**: any existing bullet that would parse poorly.
6. **Why this resume would be auto-rejected or ranked low**: specific, concrete reasons, not generic advice.
7. **Exact rewrite suggestions**: stronger, keyword-aligned wording for the weak spots above.
8. **5–10 ATS-friendly, results-based replacement bullets**, ready to use.

Be blunt and specific. Every point must name the keyword, bullet or section it applies to. Advice like "add more keywords" is not allowed.

### 4. Honest fit assessment: should they apply?
This is a judgment call, not a keyword count. Write it like a skeptical hiring manager giving a friend real advice. Cover:

- **Seniority and scope.** Work out the level the posting implies from its language: years of experience, IC vs. lead, team size and the ownership expected. A stated years-of-experience number is often wrong, so don't rely on it alone. Compare that level with the user's real timeline in the knowledge bank. If the posting is aimed at someone more senior, say so plainly and say by how much.
- **Disqualifying vs. merely weak gaps.** Separate "missing, but learnable in a few months" from "core to the role and absent from their background". Name the specific requirement.
- **Competitive reality.** Describe what a realistic competing applicant pool looks like for this kind of role at this kind of company, and how the user compares. Base it on the gaps found in Step 3, not on impressions.
- **Genuine strengths for this posting.** Don't bury real differentiators under the list of gaps.
- **Verdict**: one of **Strong apply**, **Worth applying**, **Long shot but defensible** or **Skip this one**, with 2–4 sentences of reasoning that cite the evidence above. For anything below "Strong apply", say what would need to be true for it to become one. If the verdict depends on something the knowledge bank doesn't settle, such as location, relocation, work authorization or travel, give the verdict on qualifications alone and state the condition in one line.

The verdict doesn't block the rest of the workflow. Generate the tailored resume regardless, since the user may still want it. But the verdict must be the first thing the user sees in the summary.

### 5. General resume tips (not tied to this posting)
Make one pass over the resume as a document and flag concrete problems that would affect any application. For example:
- Metrics are uneven across bullets: some are precise, others vague.
- An entry dilutes focus instead of adding signal.
- A strong asset is under-surfaced.
- Content is stale for the user's career stage.
- Section order doesn't match how this kind of role is usually read.

Every tip must name the exact line or section it applies to. Keep only the few tips that are true and worth fixing, not a padded checklist.

### 6. Generate the tailored `.tex`
Copy `resume_template.tex` and edit:

- **Headline or summary line** (if the template has one): adjust it to the role, using only true framing.
- **Skills**: reorder and choose skills from the knowledge bank to maximize overlap with the posting.
- **Experience**: for each role, choose the bullets that are strongest for this posting. This is typically 3–5 for the most recent or most relevant role and 1–3 for older roles. These are guidelines, not caps: the page should be full (see Step 7). Never merge two accomplishments into one bullet to hit a count. Light rewrites to work in a keyword are fine only if the keyword is true. Every bullet must pass the voice check below.
- **Bullet order follows the posting.** Within each role, put the bullets that match the posting's function first. If the user's current role is in a different function than the posting (e.g. a paid-media role applying to lifecycle), still list roles in date order. Make sure the current role shows at least one bullet in the posting's function, if the knowledge bank has one.
- **Domain framing calibration.** The same real work can often be framed in more than one domain. For example, a project in a specialized industry can be described with industry-specific detail or as transferable technical work. Choose the framing this posting calls for, then **apply it consistently across every role**, not bullet by bullet. Use specialist detail when the posting is in that domain. Use transferable framing when it isn't. Employer names never change. Only the bullet content is reframed.
- **Optional sections** (projects, publications, volunteering and so on): include them only when they add keyword coverage for this posting. They're the first things cut for space.
- **Bold**: bold the metrics, plus only 2–3 short concepts that match the posting. Don't bold tool or product names. Too much bold weakens the emphasis.

**Guardrails. These override keyword coverage:**
- **Never add a keyword the user couldn't defend in an interview.** A missing keyword is a gap for the report, not something to attach to a bullet or pad into a skills row. The reverse also holds: when the user's real work genuinely *is* the keyword, use the posting's exact wording.
- **Never soften a missing requirement into nearby wording.** If the user hasn't done something, a vaguer phrase that implies they have is the same overclaim.
- **Never let a number drift.** Every metric and count must match the knowledge bank exactly. Don't round, inflate or merge figures. If two sources disagree, ask the user.
- **Keep the credit where the knowledge bank puts it.** If the bank says the user did something *with* someone or *alongside* an engineer or team, keep that in the rewrite. Dropping a collaborator to tighten a sentence is an ownership overclaim.
- **Renaming vs. inferring.** If the knowledge bank describes the exact activity in different words, use the posting's word ("holdout tests" → "incrementality testing" when the holdouts measured incremental lift). If using the posting's word would need you to infer something the bank doesn't say, such as scope, seniority, ownership or a tool, it's an "implied" skill: flag it and don't add it.
- **Confirm "implied" skills first.** Anything flagged in Step 3, item 4 needs the user's confirmation before it goes on the resume. This also covers true facts the bank doesn't tie to a specific role: ask where they belong, and don't guess.
- **When you can't ask.** If no one is available to answer a question, the default is always the conservative one: leave the item off, and list the question in the summary.
- **Template vs. knowledge bank.** The user wrote both, so details found only in the template (contact info, education dates and so on) can stay. If the two conflict, ask before writing. If no one is available to answer, use the knowledge bank's value and put the conflict at the top of the summary.

**Voice check. Every generated or rewritten bullet must pass:**
- It opens with one strong past-tense verb that names what was done (Built, Designed, Owned, Launched, Led, Reduced…).
- It names the real object immediately, using specific nouns, not "a solution".
- It's one dense sentence of at most 2 wrapped lines, never a fragment or a mini-paragraph.
- Where the knowledge bank has one, it ends with a concrete, exact, bolded **outcome** metric tied to a verb. It never uses vague results like "significantly improved". Only the outcome is bolded, never input or setup figures such as test counts, cadences or holdout sizes. If the bank gives only an approximate count ("about 45 tests"), write it as approximate and don't let it be the bullet's headline result.
- It joins two objects with "and", never "plus".
- It has no first person, no passive voice and no buzzwords: *leveraged, utilized, spearheaded, responsible for, passionate, cutting-edge, synergies, results-driven*.
- No opening verb repeats within one role, and at most one repeats on the whole page. Avoid weak openers such as *customized, worked on, helped*.
- **It passes the stranger test.** A recruiter outside the user's team must understand, on one read, what was done and why it mattered. A bullet that only makes sense next to the one before it, or that describes a method with no result, gets rewritten or cut.
- **It's simple.** Verb, a brief what, then the metric. Internal mechanics and long tool chains belong in the knowledge bank or interview prep, and tool names belong in the skills section.
- No concept or method appears in two bullets. Keep it in the bullet where it's stronger.

A bullet that hits every keyword but reads like generic ATS bait fails the check. Rewrite it.

**Truth pass. Run this before compiling:**
- Any wording not copied directly from the knowledge bank is a new claim. List each one in the summary for the user to confirm. Don't present it as settled.
- Watch for the common inflations. Examples: "model-based" or "LLM-based" when it was rule-based, "built" when it was co-built, "implemented" when the user used an existing library, "real-time" when it was batch, and any named technique the user couldn't explain in an interview.
- Skills rows hold only tools and techniques a recruiter would search for. Task descriptions such as "data labeling coordination" don't belong there.

### 7. Compile and enforce one page
Run `pdflatex` (or `latexmk -pdf`) twice in the output directory, then check the page count with `pdfinfo` or `python3 -c "from pypdf import PdfReader; print(len(PdfReader('<file>.pdf').pages))"`.

**Fill before you trim.** The page should end within about an inch of the bottom margin. If more than about 15% of the page is blank, add back the next-strongest truthful bullet that fits the posting, preferring the most relevant role. Recompile and repeat. A resume that's a third empty while relevant experience sits unused is a failure, just as much as one that runs to two pages. If the knowledge bank runs out of relevant, truthful material first, stop there. Never pad. Say in the summary that the page is short and that more detail in the knowledge bank would fill it.

If the resume runs over one page, trim in this order and recompile after each step:
1. Drop optional sections that add the least coverage for this posting.
2. Drop the weakest remaining bullet, starting with the role that has the most bullets.
3. Tighten vertical spacing slightly, by no more than 1–2pt from the template's values. Going further looks cramped.

Never shrink the font or margins to make the resume fit.

### 8. Check the formatting on the rendered PDF
A one-page count doesn't mean the layout is clean. Render the PDF to an image (`pdftoppm -png -r 200`, or `qlmanage -t -s 2000 -o .` on macOS), then look at it and check:
- **No bullet wraps to 3 or more lines.** Shorten the sentence itself.
- **Each skills row stays on one line.** Drop that row's lowest-priority item, and don't change column widths.
- **The gaps look even, measured rather than eyeballed.** Measure pixel gaps on the render: before each section heading, from heading to first item, between bullets and between entries. Each group should match within a few pixels. Before tuning any spacing value, compare the body's structure with the template's. A missing list wrapper or spacing command can look like a spacing problem.
- **There are no orphaned tails.** The last line of a bullet shouldn't hold just one or two words, and a number shouldn't be split from its unit, if a small rewording fixes it. Use `~` in LaTeX to keep a number and its unit together.
- **The bottom of the page is right.** If the page ends against the bottom margin, cut the weakest line instead of squeezing the spacing. If there's leftover room after Step 7, spread it evenly (before first bullets, between bullets and between sections) rather than leaving it all at the bottom.
- **The skills label column fits its labels.** If every label is short, a wide fixed label column leaves a gap that looks loose. Size the column to the longest label actually used.

### 9. Write outputs
Create `Applications/<Company>-<RoleShort>-<YYYYMMDD>/` in the working directory, using today's date. Delete the LaTeX build files (`.aux`, `.log`, `.out`) and the preview image when you're done. The folder should contain only:
- `Resume_{Name}_{JobFamily}_{Company}_{SubTeam}.tex`
- `Resume_{Name}_{JobFamily}_{Company}_{SubTeam}.pdf`
- `gap_analysis.md`: Steps 3, 4 and 5 combined, in that order.

Filename segments are CamelCase with no spaces, joined with underscores:
- `{Name}`: the user's name as it appears in their template, e.g. `JaneDoe`.
- `{JobFamily}`: the role's function in short CamelCase, e.g. `SWE`, `PM`, `DataScience`, `GrowthMarketing`, `Design`. Reuse one from earlier files in `Applications/` when it fits, so similar roles get the same label.
- `{Company}`: the employer.
- `{SubTeam}`: the team or product named in the posting, or `None` if no team is named.

### 10. Summarize to the user
Lead with the Step 4 verdict. Then cover:
- The keyword coverage achieved (e.g. "16/20 top keywords now covered").
- What changed compared with the closest earlier resume (or with the template, if this is the first run).
- Any "implied but not stated" items and truth-pass wording that need the user's confirmation, as a short numbered list the user can answer inline (e.g. "1. yes 2. no, it was co-built").
- The top 1–2 general tips from Step 5.

### 11. Common follow-ups
Handle these with the defaults below, without asking for preferences first.

**Referral blurb ("why this candidate").** A referrer often needs a few lines to paste into a referral form. Defaults:
- Third person, using the candidate's first name only.
- 3–5 short, casual sentences, written the way someone who knows the candidate would talk. If the user says how the referrer knows them, include it.
- No metrics and no lists. Describe the kind of work and the kind of teammate.
- Avoid phrasing that reads as AI-written, such as "rare combination", "not just X but Y", "keeps coming back to" and filler like "actually".
- If the candidate has a background outside work that's relevant to the role, lead with it.

**Copy-paste text** (blurbs, emails, outreach messages, skill lists for application forms): give it as plain, unindented text, not in a block quote or an indented code block, so it copies cleanly.

**Feedback from another reviewer.** If the user pastes feedback, don't apply it all at once. Go point by point: agree, partly agree or disagree, each with a reason. Ask any factual question a suggestion depends on before acting on it. Then make small edits that keep the current format and length.
