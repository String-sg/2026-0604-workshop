# Workshop Prompts — Stacked, ECG-aligned

**Each prompt builds on the previous one.** Don't restart the chat between prompts. Claude keeps state — your filtered dataframe, your chart, your previous answer — and you build on top.

**The 4-step pattern** — same for every scenario:
1. **Profile + Slice** — fetch, clean, filter
2. **Visualise** — chart with intent
3. **Cross-cut** — add a second dimension *(when relevant)*
4. **Interrogate** — push back

**Magic phrase:** *"Run actual code. Don't summarise."*

If Claude responds without using the analysis tool, push back: *"You didn't run code. Try again."*

---

## 🎯 HERO — Where do I want to go?
*"Show students the full post-secondary landscape, honestly."*

**Use case:** CCE ECG lesson on post-secondary options. Subject combination briefing for Sec 2. ECG Fair preview.

### Prompt 1 — Load & rank

```
Fetch this CSV with real Python:
`https://raw.githubusercontent.com/String-sg/2026-0604-workshop/main/GraduateEmploymentSurveyNTUNUSSITSMUSUSSSUTD.csv`

Set `N = 30` at top of script.

For year 2024:
- No degree filter — include all degrees.
- Coerce `gross_monthly_median` and `employment_rate_ft_perm` to numeric; drop rows where `gross_monthly_median` is non-numeric ("N.A.").
- Sort by `gross_monthly_median` desc, then `employment_rate_ft_perm` desc (tiebreak).
- Show top N rows: university, degree, gross_monthly_median, employment_rate_ft_perm. State total row count and N.
- Run actual code. Don't summarise.
```

### Prompt 2 — Visualise

```
From that same filtered dataframe, plot a horizontal bar chart of the top N degrees.
- Salary on x-axis. Degree + university on y-axis.
- Highest salary at top. Dollar label at end of each bar.
- Title: "Top {N} starting salaries, Singapore graduates 2024"
- Make it screenshot-ready for an ECG lesson or PTM.
```

### Prompt 3 — Cross-cut

```
Same dataframe. Now show employment rate alongside each bar (dual axis or twin chart).

Flag in red any degree where `employment_rate_ft_perm < 65`. That's the "high pay, low employment" trap — relevant for the ECG conversation.

List those flagged degrees beneath the chart.
```

### Prompt 4 — Interrogate

```
For the flagged degrees:
1. What's the sample size for each? If small, is the median trustworthy?
2. What's the 25th and 75th percentile salary for them? A median can hide a wide spread.
3. Give me ONE honest sentence a teacher could say in a CCE ECG lesson about each of these "trap" courses — not dismissive, not naive. Intellectually honest.

Be direct. No platitudes.
```

---

## 📚 Scenario A — How do I get there? (JC Parent Path)
*"My child got 8 A1s. Computing vs Law vs Medicine?"*

**Use case:** PTM with Upper Secondary parents. ECG Fair Q&A.

### Prompt 1 — Load & filter

```
Fetch this CSV with real Python:
`https://raw.githubusercontent.com/String-sg/2026-0604-workshop/main/GraduateEmploymentSurveyNTUNUSSITSMUSUSSSUTD.csv`

For year 2024:
- Filter to degrees where `degree` contains "Computer Science", "Computing", "Law", "Medicine", or "MBBS" (case-insensitive).
- Coerce `gross_monthly_median` and `employment_rate_ft_perm` to numeric. Keep "N.A." rows but flag them.
- Show: university, degree, gross_monthly_median, employment_rate_ft_perm, gross_mthly_25_percentile, gross_mthly_75_percentile.
- Sort by median salary desc.

Run actual code.
```

### Prompt 2 — Compose for parents

```
Build a one-page parent comparison from that data. Three columns: Computing, Law, Medicine.

Per column:
- Starting salary at 6 months (note where suppressed, why)
- Salary spread (25th–75th percentile)
- Employment rate
- Time to first paycheck (Computing 3–4 yrs, Law 4 yrs + bar, Medicine 5 yrs + housemanship)
- Entry difficulty (rough A-level cutoff)
- Bond / commitment

Format as a clean markdown table I could screenshot and bring to PTM.
```

### Prompt 3 — Interrogate

```
What's the strongest argument AGAINST each of these three paths? Be honest — parents respect that more than reassurance.

Then: draft the question a Home Tutor should ask the parent INSTEAD of answering theirs. What about the child should we find out first — connecting back to the VIPS framework (Values, Interests, Personality, Skills)?
```

---

## 🏗 Scenario B — How do I get there? (Poly Pathway)
*"Are we training the right people for the right jobs?"*

**Use case:** Sec 4 streaming briefing. Poly-vs-JC conversations. ECG Fair for Lower Sec.

### Prompt 1 — Load two datasets

```
Fetch both these CSVs with real Python:

1. `https://raw.githubusercontent.com/String-sg/2026-0604-workshop/main/IntakeEnrolmentandGraduatesofPolytechnicsbyCourse.csv`
2. `https://raw.githubusercontent.com/String-sg/2026-0604-workshop/main/EmploymentPersonsBySectorAsAtYearEndAnnual.csv`

For each:
- Show columns, year range, 3 sample rows.
- Note any wide-format issues, leading whitespace in category names, type coercion needed.
- State row counts.

The sector CSV is in WIDE format (years as columns) — flag this. Don't analyse yet, just profile.
```

### Prompt 2 — Slice & pivot

```
For years 2014–2024:

From poly CSV: sum intake across sex by year, for courses: Information Technology, Engineering Sciences, Health Sciences, Business & Administration, Applied Arts. Output long-format table (year, course, total_intake).

From sector CSV: pivot to long format. Strip whitespace from sector names. Show employment ('000) by year for: Information And Communications, Manufacturing, Financial And Insurance Services, Community Social And Personal Services, Accommodation And Food Services.

Run actual code.
```

### Prompt 3 — Visualise

```
Two line charts side by side, same time axis (2014–2024).
- Left: poly intake trends (one line per course)
- Right: sector employment trends (one line per sector)
- Use the same colour for matched pairs (IT ↔ Info & Comms, Engineering ↔ Manufacturing)
- Annotate any course where intake dropped >20% over the decade.
```

### Prompt 4 — Interrogate

```
1. Which poly course is growing while its matching sector is also growing? (alignment)
2. Which is shrinking while its sector grows? (talent gap)
3. Which is growing while its sector is flat or shrinks? (potential oversupply)

Cite specific numbers. Then tell me what to say to a Sec 4 student about the "safer" poly bet — and what assumptions I'm making. This connects to "How do I get there?" in the ECG framework.
```

---

## 🎨 Scenario C — Who am I? (Passion vs Paycheck)
*"My student wants something creative. Am I letting them down?"*

**Use case:** Home Tutor 1-on-1 DSLO review. Form teacher interest-mapping. CCE ECG lesson on VIPS (Values, Interests, Personality, Skills).

### Prompt 1 — Load & filter (two groups)

```
Fetch this CSV with real Python:
`https://raw.githubusercontent.com/String-sg/2026-0604-workshop/main/GraduateEmploymentSurveyNTUNUSSITSMUSUSSSUTD.csv`

For year 2024:
- Two groups.
  - GROUP_CREATIVE: degrees containing "Industrial Design", "Landscape Architecture", "Linguistics", "Art, Design", "Communication Studies", "Bachelor of Arts"
  - GROUP_PRACTICAL: degrees containing "Computer Science", "Accountancy", "Nursing (Hons)", "Law - Cum Laude"
- Coerce salary fields to numeric. Drop N.A. rows.
- Show: degree, university, gross_mthly_25_percentile, gross_monthly_median, gross_mthly_75_percentile, employment_rate_ft_perm, group.
- Calculate `spread = 75th - 25th`. Sort by spread desc within each group.

Run actual code.
```

### Prompt 2 — Visualise

```
Range chart: each degree is a row. Bar spans 25th to 75th percentile. Marker at median.

Colour-code by group (creative vs practical). Sort within each group by median desc.

Highlight the degree with the widest spread (skill rewards highest) and narrowest (pay flattens regardless).
```

### Prompt 3 — Interrogate

```
1. In which courses does being top-quartile actually pay off most? In which is the ceiling low?
2. For a Sec 3 student who has strong creative INTERESTS (the I in VIPS) but worried parents — give me 3 sentences that respect their interest without being dismissive ("be realistic") or naive ("follow your heart"). Honest about the trade-off.

Don't moralise. Just be intellectually honest.
```

---

## ❤️ Scenario D — How do I get there? (Mission Jobs)
*"Should I encourage my brightest students to consider teaching, nursing, or social work?"*

**Use case:** Form-class assembly. CCE ECG lesson on values + occupations. Speech Day occupational-appreciation theme.

### Prompt 1 — Load & filter

```
Fetch this CSV with real Python:
`https://raw.githubusercontent.com/String-sg/2026-0604-workshop/main/GraduateEmploymentSurveyNTUNUSSITSMUSUSSSUTD.csv`

For year 2024:
- Filter to degrees where any of these appear (case-insensitive): "Nursing", "Education", "Social Work", "Early Childhood".
- Coerce salary + employment rate to numeric. Drop N.A. rows.
- Show: university, degree, gross_monthly_median, employment_rate_ft_perm.
- Sort by salary desc.

Also flag: this dataset does NOT include MOE teachers (PGDE route), entry-level RNs, or social workers at NCSS-affiliated agencies. From your general knowledge, note typical 2025 SG starting pay for these, with sources cited.

Run actual code for the dataset part.
```

### Prompt 2 — Compose comparison

```
Compare mission jobs against the market darling: NUS Computer Science (median ~$6,500, 75th ~$7,500).

Build a chart showing the salary gap, but annotate each mission role with a qualitative dimension I can't get from the data: typical work hours, job security, mission alignment, exit options. Make a reasoned judgement per row.
```

### Prompt 3 — Interrogate

```
Help me build the honest case for my brightest students:
1. Name the financial gap explicitly. Don't soften it.
2. For each mission job, what's appealing that the salary number misses? (This connects to VALUES in the VIPS framework.)
3. Draft 3 sentences I could say in form-class assembly. Not a sales pitch — intellectual honesty.

What's the parent's strongest objection going to be? How would you answer it?
```

---

## 🔁 Universal push-back prompts

Use these on any answer that feels too smooth.

```
You didn't run the analysis tool. Try again — actually execute Python on the data.
```

```
What's the sample size for this course? If small, is the median trustworthy?
```

```
Show the trend from 2019 to 2024, not just this year. Is 2024 typical or unusual?
```

```
What would a sceptical parent ask that this analysis doesn't answer?
```

```
The "**" or "N.A." values — what do they mean? Are you treating them as zero?
```

```
Make this readable for a 15-year-old. Cut the jargon.
```

```
How does this connect back to "Who am I?" / "Where do I want to go?" / "How do I get there?" — the 3 Core ECG Questions?
```

---

## 📋 FALLBACK DATA (if URL fetch fails)

Paste this in place of Prompt 1 for the HERO scenario:

```
Use the analysis tool. Here's real Singapore Graduate Employment Survey 2024 data:

| Course | University | Median (S$) | 25th %ile | 75th %ile | FT Employment (%) |
|--------|------------|------------:|----------:|----------:|------------------:|
| Computer Science | NUS | 6500 | 5600 | 7500 | 87.8 |
| Law (Cum Laude+) | SMU | 7000 | 6500 | 7000 | 95.9 |
| Information Security | NUS | 6110 | 5400 | 7000 | 88.2 |
| Computer Engineering | NTU | 5500 | 4800 | 6200 | 89.9 |
| Data Science & AI | NTU | 5450 | 4800 | 6100 | 76.0 |
| Engineering Science | NUS | 5500 | 4800 | 6200 | 81.0 |
| Business Analytics | NUS | 5400 | 4700 | 6000 | 87.8 |
| Business Admin | NUS | 5100 | 4500 | 5800 | 81.8 |
| Architecture | NUS | 4995 | 4500 | 5500 | 94.1 |
| Accountancy | NTU | 4350 | 4100 | 4500 | 93.2 |
| Bachelor of Arts | NUS | 4300 | 3700 | 5000 | 74.2 |
| Social Sciences | NUS | 4210 | 3700 | 4800 | 73.9 |
| Psychology | NTU | 4174 | 3600 | 4700 | 68.9 |
| Nursing (Hons) | NUS | 4050 | 3880 | 4250 | 94.3 |
| Industrial Design | NUS | 4025 | 3500 | 4500 | 60.0 |
| Sports Science | NTU | 4000 | 3500 | 4500 | 55.1 |
| Communication Studies | NTU | 3880 | 3500 | 4300 | 70.3 |
| Linguistics | NTU | 3850 | 3600 | 4050 | 62.3 |
| Landscape Architecture | NUS | 3800 | 3500 | 4330 | 52.9 |
| Art, Design & Media | NTU | 3500 | 3200 | 4000 | 47.1 |

Set N = 20. Sort by median desc, employment desc (tiebreak). Show all rows. Then proceed with Prompt 2 (Visualise).
```

**Mission jobs not in dataset (typical SG starting pay 2025):**
- MOE teacher (fresh PGDE grad): ~S$3,625
- Registered nurse (fresh grad, public hospital): ~S$3,500
- Social worker (degree, NCSS-affiliated): ~S$3,900

---

## 📊 Data sources

- Graduate Employment Survey 2024 (data.gov.sg)
- Polytechnic Intake by Course (data.gov.sg)
- Employment Persons by Sector (data.gov.sg, MOM)
- MOE Careers; NCSS salary guidelines
- MOE CCE 2021 syllabus (ECG framework, 3 Core Questions, VIPS)

Workshop repo: https://github.com/String-sg/2026-0604-workshop
