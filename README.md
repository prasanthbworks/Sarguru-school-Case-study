# Sri Sarguru Matriculation School — Website Rebuild

**Live:** [www.sarguruschool.com](https://www.sarguruschool.com) · **Status:** In production since August 2026 · **Build:** v4

A real client project for a matriculation school in Theethipalayam, Coimbatore.
I worked as business analyst, product owner and implementation lead. The site is
hand-coded static HTML/CSS/JS, deployed on Vercel, built with AI assistance.

This document is the case study. The reasoning behind the build is in
[docs/DECISION-LOG.md](docs/DECISION-LOG.md), the test approach in
[docs/VALIDATION.md](docs/VALIDATION.md), and the as-built system in
[docs/SOLUTION-DESIGN.md](docs/SOLUTION-DESIGN.md).

---

## The problem

The school's web presence had lapsed. An older Wix site had dropped to a free
plan, which meant it carried Wix's own branding and no custom domain behaviour.
For a school whose admissions depend almost entirely on local reputation and
word of mouth, this was the wrong first impression at the wrong moment.

The brief I was given was "the website needs updating." That is not a problem
statement, so the first job was to find the real one.

Parents choosing a school for a five-year-old do not buy a syllabus. They buy
confidence that the place is serious, that the people are competent, and that
their child will be looked after. Most of them now form that impression on a
phone, before they ever call the office or visit the campus.

So the objective was not a prettier website:

> **Increase admission enquiries by making the school's digital presence
> credible enough that a parent researching on a phone takes the next step —
> a call, a message or a visit.**

That reframing changed almost every decision that followed. It is the reason
the site has an enquiry form above the fold on mobile and does not have a
student portal.

## Constraints

These shaped the solution more than any preference did.

| Constraint | Consequence |
|---|---|
| No budget for recurring hosting or platform fees | Ruled out every website builder with a domain-capable paid tier |
| No technical staff at the school | Whatever shipped had to keep working without anyone maintaining it |
| No stock imagery | Every photograph had to come from the school's own archive |
| Domain ownership initially unknown | Could not plan the launch until it was located |
| A live email service on the domain, undocumented | Discovered during DNS work; had to survive the cutover untouched |
| Single contributor, AI-assisted | Scope had to stay small enough to actually finish and test |

The last one matters more than it looks. A scope that one person can build,
test and hand over is a different scope from one a team can. Deciding that
honestly at the start is cheaper than discovering it at week six.

## Scope: what was deliberately left out

V1 shipped two pages — a home page and an Our School page — consolidated from
the eight pages of the original site.

**In:** school introduction, academic positioning, the SUITS programme, life
beyond the classroom, photo galleries, community videos, enquiry form, contact
details, responsive layout.

**Out of V1:** student or parent portals, a CMS, blog and news, alumni
features, downloadable documents, elaborate animation.

Each exclusion was a decision, not an oversight, and each is recorded in the
decision log with its reasoning. The consolidation from eight pages to two came
from a simple observation: on the original site, most pages held one screen of
content each, and a parent on a phone will not tap through eight of them. Fewer,
denser pages read as more confident.

The pressure in a project like this is always to add. A school will happily
list twenty things it would like on a website. The useful contribution of a BA
here is not writing all twenty down — it is knowing which two matter for the
stated objective and being able to explain why the other eighteen wait.

## Approach

**Requirements and positioning.** Established what the school wanted to
communicate — quality, trust, aspiration, responsible citizenship — and, just
as importantly, what it should not communicate. A deliberate constraint was
that the site must never frame students through a charity or need-based lens.
The target register was credible and premium without being flashy. A nearby
school was used as a quality benchmark, explicitly not as a template to copy.

**Content.** Worked from roughly 37 photographs of the school's own events and
facilities. Applied uniform colour grading across all of them so the gallery
reads as one archive rather than a decade of different phone cameras. Cropped
watermarks from three. Removed one entire shoot that did not meet the standard.
Established that no photograph appears twice on the site, with one exception
the client approved.

**Build.** Hand-coded static HTML, CSS and JavaScript. No framework, no build
step, no dependencies. Files are served exactly as they sit in the repository.
For a two-page site that must survive without maintenance, every dependency is
a future failure with nobody available to fix it.

**Validation.** A full test cycle before release — see below.

**Release.** Located the domain, repointed DNS to the new host without
disturbing the school's existing email, verified, went live.

## What went wrong

**v3 deployed with every image broken.** The markup referenced an `images/`
folder. The repository was flat. Locally it had worked; in production, all 37
photographs returned 404. Fixed in a three-file patch, but the lesson is that
"it works on my machine" has a specific meaning for static hosting: the path
that matters is the one in the deployed tree, not the one in the editor. Every
asset reference is now verified at build time — 50 references, zero broken.

**The domain was not where anyone expected.** It was in the school's own Wix
account, which also turned out to be running a live Microsoft 365 email tenant
that nobody had documented. Repointing DNS to the new host meant editing
records individually inside Wix — external nameservers are not permitted — while
leaving the mail records completely alone. Getting this wrong would have taken
down the school's email during admission season. This was the highest-risk
thirty minutes of the project, and it was risky specifically because the
dependency was discovered rather than declared.

**A test cycle that said "not yet."** The v3 test run finished with eight
failures, eight untested cases and thirteen logged issues. The verdict cell in
the report read NOT YET. The temptation at that point — the site looked
finished — was to ship and fix later. v4 was built against the issue list
instead and verified in a headless browser before release.

## Validation

| | Cases | Result |
|---|---|---|
| Automated | 103 | 103 passed |
| Manual | 85 | 68 passed · 8 failed · 8 untested · 1 change request |
| **Total** | **188** | **13 issues logged → v4** |

Automated coverage spanned media integrity, page structure, navigation,
content accuracy, SEO, design consistency, accessibility and performance.
Manual coverage spanned the enquiry form, contact routes, responsive behaviour,
cross-browser rendering, and usability review.

Two of the manual cases were not about software at all. Before publishing, I
confirmed that photographic consent was on record for the children pictured,
and that the professional photographs were cleared for publication by the
photographer rather than assumed to be usable because I had the files. Neither
would have been caught by any automated check. Both would have been damaging to
miss on a school website.

Measured, not asserted: 219 KB to first paint, 2.85 MB for the fully scrolled
home page, 5.15 MB for the whole image library across 37 files.

**Honest limitation:** all of this testing was performed by me. There is no
client acceptance record, and no user testing with actual parents. Two manual
cases were designed to require the school's confirmation and were never
returned. The detail, including what I would do differently, is in
[docs/VALIDATION.md](docs/VALIDATION.md).

## Outcome

The site is live at www.sarguruschool.com. Enquiries arrive through a hosted
form. The school pays nothing for hosting; the only recurring cost is the
domain it already owned, which renews in 2028.

I am not going to claim an enquiry uplift. The site launched in August 2026 and
there is no reliable baseline from the previous site to compare against —
analytics were not in place beforehand. Claiming a percentage here would be
inventing a number, and the fastest way to lose a technical interview is to
quote a metric you cannot defend. What can be said is that the objective was
defined in measurable terms, and that measuring it properly is the first item
of V2 work.

## Open items

Carried forward honestly rather than closed on paper.

- **Two content figures rest on the school's own account of itself.** Both
  were supplied at requirements stage, checked against public education
  directories during testing, raised with the school where they differed, and
  confirmed before release. The school is the source of record for its own
  history, but the discrepancy is documented rather than buried.
- **Enquiry notifications route to a personal address.** The free tier of the
  form service locks the recipient to the account owner. An interim forwarding
  rule is planned; the durable fix is rebuilding the form under a school-owned
  account.
- **Ownership sits on personal accounts.** The repository, the hosting and the
  form are all under my personal accounts. Only the domain is school-owned. If
  this project transfers, those three need migrating or the school loses
  control of its own site. This is a governance risk, and it is named here
  because a handover that omits it is not a handover.
- **Content maintenance has an interim owner and no successor.** At
  requirements stage the school confirmed updates would be handled from my side
  for now, which is what makes the static architecture defensible. What has not
  been decided is what happens when that arrangement ends. If the school ever
  needs to publish notices itself, this architecture is the wrong fit and a
  builder would be the honest recommendation.
- **Backlog:** a custom 404 page, and a phone-validation defect on iOS that
  lives inside the form provider's embed rather than in this codebase.

## What I would do differently

**Check the client's own published facts before writing copy, not after.**
Two figures the school supplied did not match public education records. I found
that during testing, when the copy was already written and approved. Comparing
what a client tells you against what public records say is a twenty-minute task
at requirements stage and a credibility problem at release.

**Ask "who edits this after I leave?" rather than "who edits this now?"** I did
ask the maintenance question at requirements stage, and the answer — my side,
for now — is what made a static site defensible. What I did not ask was what
happens when "for now" ends. The architecture depends on an arrangement with no
end date and no successor, and that is the weaker half of a question I thought
I had covered.

**Inspect the domain's DNS zone before planning the cutover.** The live mail
tenant should have been a known input to the release plan, not a discovery
during it. The general form of the lesson: before migrating anything, enumerate
what else depends on it.

**Get written acceptance rather than self-signing off.** Testing my own build
and declaring it ready is the weakest part of this project. A short signed
acceptance, even informal, would have closed the loop properly.

**Instrument before launching.** Without analytics on the old site, the stated
objective became unmeasurable at exactly the moment it mattered.

## Why this is in a data portfolio

The obvious objection is that a static school website is not analytics work.
Fair. What transfers is the part that is not HTML: taking a vague request and
finding the actual business problem, controlling scope against pressure to
expand, making decisions with trade-offs I can still defend, testing my own
output rather than trusting it, discovering an undocumented production
dependency before breaking it, and writing the whole thing down so somebody
else could take it over.

Those are the same instincts a data pipeline needs. The technology is
different; the judgement is not.

---

## Repository

| File | What it is |
|---|---|
| `README.md` | This case study |
| `DECISION-LOG.md` | Decisions, options considered, trade-offs accepted |
| `VALIDATION.md` | Test approach, coverage, results, limitations |
| `SOLUTION-DESIGN.md` | As-built system documentation |
| `index.html`, `about.html` | The two pages |
| `styles.css`, `script.js` | Styling and behaviour |

*Published with the school's permission. Photographs are the property of Sri
Sarguru Matriculation School and are included here as delivered project
evidence.*

---

## Documentation

| Document | What it holds |
|---|---|
| [docs/REQUIREMENTS.md](docs/REQUIREMENTS.md) | Discovery record as captured in July 2026, and what shipped against it |
| [docs/DECISION-LOG.md](docs/DECISION-LOG.md) | Thirteen decisions: options considered, reasoning, trade-off accepted |
| [docs/VALIDATION.md](docs/VALIDATION.md) | Test approach, coverage, results, and what the testing does not cover |
| [docs/SOLUTION-DESIGN.md](docs/SOLUTION-DESIGN.md) | As-built system documentation, compiled after implementation |
| [docs/ACCEPTANCE.md](docs/ACCEPTANCE.md) | Client acceptance record — published unsigned, deliberately |

Operational detail — account ownership, DNS record values, notification
addresses — is held in a private handover with the client and is deliberately
absent here.

Published with the school's permission.
