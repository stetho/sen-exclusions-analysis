# SEN Exclusions Analysis

An investigation into whether English local authorities with higher rates of
permanent exclusions among children with Special Educational Needs also show
higher rates of young people entering the youth justice system.

## Why this project exists

My wife is a social worker in a child protection team in inner London. Over
time, a pattern became impossible to ignore: a significant proportion of the
children she works with - children who have become known to social services
after contact with the police - are subsequently diagnosed with a learning
disability. Dyslexia. ADHD. Autism. Sometimes more than one.

The pattern goes like this. A child is excluded from school, usually for
behaviour described as disruptive or aggressive. What isn't recognised at the
time is that the behaviour is the child's way of expressing a frustration
that nobody has named: they cannot engage with the learning experience their
peers are taking part in, because they have an unmet need that nobody has
identified. After exclusion, with no school to go to and too young to work,
the child gravitates toward others in the same situation. Some of those
people are a bad influence. Some of those situations lead to police contact.
Under 18, that means social care involvement. And at that point — often for
the first time — someone refers the child to CAMHS, and the diagnosis arrives.

By then, the child has a criminal record before the age of 18. Getting their
life back on track is a massive struggle. All because they didn't receive
support they should have received automatically — support that would have
cost far less than the social care, criminal justice, and long-term
consequences that follow.

My sociological training tells me that society has a template for the model
citizen, and anyone who falls outside that template gets treated as a problem
rather than a person whose needs are not being met. My criminological training
tells me that the pathways into the justice system are not random. And my data
skills tell me that if this pattern is as common as one social worker's caseload
suggests, there should be a signal in the data.

This project looks for that signal.

---

## The question

> Do English local authorities with higher rates of permanent exclusions among
> children with Special Educational Needs show higher rates of first-time
> entrants to the youth justice system?

And, once deprivation is controlled for:

> Is the exclusion of children with *no identified* SEN a better predictor
> of youth justice referrals than the exclusion of children with identified
> SEN — consistent with a pipeline driven by *undiagnosed* need?

---

## Datasets

All data is publicly available from UK government sources.

| Dataset | Source | What it provides |
|---|---|---|
| Suspensions and permanent exclusions by pupil characteristic | [DfE / Explore Education Statistics](https://explore-education-statistics.service.gov.uk/find-statistics/suspensions-and-permanent-exclusions-in-england) | Permanent exclusion and suspension rates by SEN status at LA level, 2019/20-2024/25 |
| Youth Justice Statistics — First Time Entrants | [Ministry of Justice / Youth Justice Board](https://www.gov.uk/government/statistics/youth-justice-statistics) | Rate of child first-time entrants to the youth justice system per 100,000 of 10-17 population, by LA, 2014-2024 |
| English Indices of Deprivation 2019 | [MHCLG](https://www.gov.uk/government/statistics/english-indices-of-deprivation-2019) | IMD average score by local authority — deprivation control variable |
| Special Educational Needs in England 2024/25 | [DfE / Explore Education Statistics](https://explore-education-statistics.service.gov.uk/find-statistics/special-educational-needs-in-england/2024-25) | School-level SEN counts by primary need type, aggregated to LA level |

Download instructions and exact filenames are in `data/raw/README.md`.

---

## What I found

### 1. The exclusion disparity is stark

Children with SEN support are permanently excluded at **nearly 5 times the rate**
of children with no identified SEN (median 0.085% vs 0.017% across 116 local
authorities). Children with EHC plans — who have the most formal protection —
are excluded at 1.8 times the no-SEN rate.

This inequality is the starting point. It is well documented, and it is
getting worse as funding for specialist support, specialist placements, and
SEN-trained teachers continues to fall.

### 2. Deprivation dominates both outcomes

Deprivation (measured by the 2019 Index of Multiple Deprivation) is strongly
correlated with youth justice referral rates (r=0.60) and moderately correlated
with SEN exclusion rates (r=0.27). A deprivation-only regression model explains
35% of the variance in youth justice referral rates across English local
authorities.

Any relationship between exclusion and justice referrals has to be assessed
against this backdrop. Crucially, deprivation is not the same thing as a
learning disability — the absence of a link between deprivation and SEN
prevalence in the data is itself meaningful.

### 3. Identified SEN exclusion rates do not predict youth justice referrals

Once deprivation is controlled for, the permanent exclusion rate for children
with identified SEN (either SEN support or EHC plan) adds no explanatory power
to the model. This is not the result we expected, and it is worth understanding.

### 4. The undiagnosed pipeline hypothesis

This null result prompted a reframe of the hypothesis. The children in the
pipeline are probably not the ones already coded as SEN in school census data.
They are the ones who have not yet been identified — children whose disruptive
or aggressive behaviour is the expression of an unmet learning need, not a
character flaw, but who get excluded before anyone names what is actually
going on.

In the data, these children appear in the *no identified SEN* category at
the time of exclusion. Consistent with this, the **suspension rate for children
with no identified SEN** shows a statistically significant positive correlation
with youth justice referral rates (r=0.20, p=0.035) — the only exclusion
measure that does.

### 5. London is a distinct case

London boroughs cluster strongly in the low-exclusion / high-FTE quadrant.
They exclude fewer SEN children than comparable deprived areas outside London,
but have some of the highest youth justice referral rates in England. This
suggests a parallel pathway — gang recruitment, county lines, the specific
social ecology of inner-city London — that operates largely independently of
school exclusion.

### 6. Some local authorities are dramatically different from their peers

Blackburn with Darwen has a deprivation score (36.0) comparable to Nottingham
(34.9), but a youth justice referral rate of 102 per 100,000 against
Nottingham's 374. Something is working in Blackburn that is not captured by
any of our variables. Whether it is specific diversion programmes, community
structures, or restorative justice approaches is not answerable from this
dataset — but it is exactly the question that policy makers should be asking.

---

## The research gap this analysis identifies

The question that cannot be answered from aggregate LA data but that is
answerable from linked individual-level data is:

> Of children who enter the youth justice system, what proportion were
> previously excluded from school, and how many received a post-exclusion
> diagnosis of a learning disability or neurodevelopmental condition?

This data exists. The Youth Justice Board collects individual-level data on
every child who enters the youth justice system. CAMHS referrals and diagnoses
are recorded in NHS systems. School exclusion records are held at pupil level
by local authorities and the DfE. These datasets are not linked in any public
dataset.

Linking them would not be technically difficult. It would require a policy
decision to treat this as a priority. The cost of not doing it is borne by
the children who fall through the gap — excluded, undiagnosed, and criminalised
before they are old enough to vote.

---

## Structure

```
notebooks/
  01_data_acquisition.ipynb          Load raw files, inspect, produce clean CSVs
  02_cleaning_and_joining.ipynb      Join datasets, resolve name matching, handle boundary changes
  03_exploratory_analysis.ipynb      Distributions, LA-level patterns, initial visualisations
  04_correlation_and_modelling.ipynb OLS regression, deprivation control, effect sizes
  05_outlier_analysis.ipynb          Quadrant analysis, residuals, case studies

data/
  raw/       Downloaded source files (not committed — see data/raw/README.md)
  processed/ Cleaned CSVs produced by notebook 01 (not committed)

outputs/     Charts and tables generated by the notebooks
src/         Reusable functions (future)
```

---

## Reproducing the analysis

```bash
git clone git@github.com:stetho/sen-exclusions-analysis.git
cd sen-exclusions-analysis
pip install -r requirements.txt
# Download raw data files per data/raw/README.md
jupyter notebook
```

Run the notebooks in order. Each writes to `data/processed/` and reads from
the outputs of the previous one.

---

## A note on causation

This analysis identifies correlations and tests hypotheses at local authority
aggregate level. Finding a correlation between any exclusion measure and youth
justice referral rates would not establish that exclusion *causes* youth
offending. Deprivation, family circumstances, unmet mental health need, the
quality of alternative provision after exclusion, and the presence of gangs
and county lines all play independent roles.

The goal is not to claim a single cause. It is to establish whether exclusion
is a meaningful risk factor in a pathway — something that can be intervened on
at a specific, identifiable point — and to be honest about where the available
data cannot take us.

---

## Author

Steve Thompson — [stetho.me](https://stetho.me) | [GitHub](https://github.com/stetho)
