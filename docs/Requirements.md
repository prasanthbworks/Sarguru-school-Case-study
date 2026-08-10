# Requirements — Discovery Record

**Captured:** 7–11 July 2026, during and immediately after a requirements
conversation with the school.
**Form:** a discovery checklist I wrote and filled in myself. It records the
school's answers in my words, not theirs. It was never signed and is not a
client-approved specification.
**Status:** published as captured. Nothing has been rewritten to match what was
eventually built.

This is deliberately rough. It is the record that existed before the build, and
its value is that it is dated earlier than everything else in this repository.
A polished requirements specification written now would describe work it could
not have driven.

The checklist it came from covers a wider engagement — a school management
system replacement. Only the static website section was in scope here, and only
that section is reproduced.

---

## Purpose and scope

| Question | Answer captured |
|---|---|
| Primary purpose | Marketing, admissions enquiries and parent information — all three |
| Existing site | Carried only basic information and contact details. The school wanted enquiries submitted online and delivered by email |
| Target launch | Approximately one week |

## Pages

| Page | Answer captured |
|---|---|
| Home | Content to be supplied |
| About / management / history | Content to be supplied |
| Admissions | Enquiry form capturing phone number, parent's name, child's name and age, the class applied for, and a message |
| Academics | Pre-KG to 10th, State board. Classical dance, local music, karate, vedic maths, abacus, silambam, martial arts, Devaram and Thiruvasagam |
| Facilities | Science labs, computer science labs, library, smart classroom, PT room, three vans. University tie-up for SUITS from 5th to 9th standard, with a diploma in Computer Science awarded at the end of 9th grade |
| Gallery | Photographs to be supplied later |
| News / events / announcements | Annual day, sports day, KG graduation day |
| Contact | Address and map to be taken from Google |
| Mandatory disclosures | Not applicable — not a CBSE school |
| Careers / recruitment | Not required at this stage |

## Content and branding

| Question | Answer captured |
|---|---|
| Who writes the copy | First draft from our side |
| Brand assets | Logo received |
| Photographs | To be supplied later |
| Languages | English only |
| Brand colours | To be decided from the logo |

## Functionality

| Question | Answer captured |
|---|---|
| Enquiry destination | Email |
| Link or login to the school management system | No — customer-facing site only |
| Map, click-to-call, WhatsApp | All required, plus Facebook, Instagram and YouTube |
| SEO | School is searchable; a Google Business profile may or may not exist |

## Operations

| Question | Answer captured |
|---|---|
| Domain | Existing domain had expired and needed renewing |
| Hosting budget | Not specified |
| Who updates content after launch | Our side, for now |

## Headline claims supplied by the school

> Started in 2000 with only 5 students, crossed 500+. Always 100% result across
> 10 years.

These three figures came from the school at requirements stage and were carried
into the site's statistics bar. Two of them were later checked against public
education directories, which recorded different values; both were raised with
the school and confirmed before release. See `VALIDATION.md`.

---

## What shipped, and what didn't

The gap between this document and the delivered site is where the product
decisions are visible.

| Captured requirement | V1 outcome |
|---|---|
| Home page | Shipped |
| About / management / history | Shipped as the Our School page |
| Admissions enquiry form | Shipped as a hosted embed rather than a built form — see D-06 |
| Academics | Shipped |
| Facilities | Shipped as six cards |
| Gallery | Shipped as galleries plus a photo strip |
| Contact, map, click-to-call, WhatsApp, social links | Shipped |
| **News / events / announcements** | **Deferred.** A news section needs someone to maintain it, and nobody at the school does. Event content was folded into the galleries instead |
| **Careers** | **Excluded**, as the school requested |
| **Link to the school management system** | **Excluded**, as the school requested |
| **English only** | **Diverged.** Two Tamil uses shipped — the school motto and one attribution. Judged appropriate to the audience rather than a requirement breach, but it is a divergence and it is recorded as one |
| Domain renewal | Completed; the domain was located in an account the school already held |
| Approximately one week | **Missed.** Discovery ran in early July; the site went live in August, after a full test cycle and a v4 rebuild |

The last row is the honest one. The deadline slipped because the v3 test cycle
returned eight failures and a verdict of NOT YET, and shipping to the original
date would have meant shipping those. The trade was made knowingly — see D-13.

---

## A correction worth recording

This document captures the university partner's name incorrectly. The error
came from my note-taking during the conversation, was caught during content
validation when the SUITS wording was checked with the school, and was correct
on the site before launch. It is left uncorrected here because this is a record
of what was captured, not of what turned out to be true.
