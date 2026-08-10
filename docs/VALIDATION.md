# Validation

How the Sri Sarguru site was tested before release, what the results were, and
what the testing does not cover.

---

## Approach

Testing was split by what a machine can check reliably and what requires a
human with a device in their hand.

**Automated checks** covered anything with a deterministic answer: whether
every referenced file exists, whether required elements and sections are
present, whether page metadata is complete, whether every image carries alt
text, whether focus and keyboard behaviour is implemented, whether the same
photograph appears twice. These are cheap to run, so they were run on every
build.

**Manual checks** covered anything requiring judgement or a real device: does
the enquiry form actually deliver, does the phone number connect, does the
gallery scroll correctly on an iPhone, does a first-time visitor notice the
enquiry call to action.

The split matters. A machine can prove an image file exists; only a person can
tell you it is the wrong image.

## Coverage

**Automated — 103 cases, all passing**

| Area | Cases | What it checks |
|---|---|---|
| Structure | 18 | Required sections present, page count, no retired pages referenced, single `<h1>` |
| Content | 18 | No placeholder text, contact details correct, banned superlatives absent, agreed copy in place |
| Design | 15 | Type scale, spacing values, font families, hover and stacking behaviour, marquee |
| Accessibility | 13 | Reduced-motion handling, dialog semantics, focus trapping, Escape to close, focus-visible, `aria-expanded`, keyboard gallery control, no-JS fallback |
| SEO | 12 | Titles, meta descriptions, Open Graph and Twitter tags, canonical URLs, `lang`, viewport |
| Media | 10 | Every reference resolves, no orphans, no unintended repeats, alt text present, uniform grading |
| Navigation | 10 | Every link and anchor resolves, no placeholder hrefs, scroll-spy present, skip link |
| Performance | 7 | Payload thresholds, no framework or build step, privacy-mode video host, font preconnect |

The media checks earned their place. A previous build had shipped with nested
asset paths against a flat repository and returned 404 for all 37 photographs
in production. The check that would have caught it — every reference resolves
against the deployed tree — now runs on every build. 50 references, zero
broken, nothing orphaned.

**Manual — 85 cases**

| Result | Count |
|---|---|
| Passed | 68 |
| Failed | 8 |
| Not tested | 8 |
| Raised as a change request | 1 |

Measured thresholds rather than impressions: 219 KB to first paint, 2.85 MB for
the fully scrolled home page with lazy loading, and 5.15 MB for the entire
image library across 37 files. Colour grading was applied as a single uniform
treatment across the archive rather than adjusted per photograph.

Spanning the enquiry form, contact routes, social links, media behaviour,
content accuracy, deployment, link sharing, responsive layout, usability,
navigation, UI consistency, individual page sections, accessibility,
performance, cross-browser rendering, and a first-impression UX review.

## Two checks that were not about software

Both sat in the manual set and neither is a technical test. Both would have
been damaging to miss.

**Photographic consent.** Before publishing, consent had to be on record for
the children pictured across 37 photographs. A school website showing
identifiable minors without that is a safeguarding problem, not a design one.

**Publication rights.** One set of photographs came from a professional shoot.
Whether the school held the right to publish them was confirmed with the
photographer rather than assumed from possession of the files.

Neither would have been caught by any automated check, and neither would have
appeared on a test plan derived from the build. They came from asking what
could go wrong for the client rather than what could go wrong in the code.

## Results and what was done about them

The v3 cycle produced **13 logged issues** and a summary verdict of NOT YET.
The issues fell into three groups.

**Functional defects.** Multiple videos playing simultaneously. The enquiry
form failing to load on first visit and working after a refresh. The navigation
indicator highlighting the home section regardless of where the user actually
was. A phone-number field accepting invalid input on iOS.

**Presentation defects.** Header buttons inconsistent with the rest of the
site. Heading alignment diverging from the approved design. White borders
around images that should have been cropped to frame. A gallery whose scroll
behaved differently on iOS than on Android.

**Content changes.** Two pieces of copy replaced. A background image in a quote
band replaced. A page section restructured to alternate image and text rather
than placing two images side by side. Institutional marks used in place of a
photograph in one section.

v4 was built against this list and verified in a headless browser before
release. One issue could not be resolved in this codebase: the phone-number
validation defect lives inside the third-party form embed, and fixing it
requires either configuration on the provider's side or replacing the form.

## What this validation does not cover

This section exists because a test report that only reports passes is not
telling you much.

**There is no client acceptance record.** Every test was designed, executed and
signed off by me — the same person who built the site. That is a genuine
weakness, not a formality. Self-testing catches defects; it does not catch
wrong assumptions, because the assumptions are shared between the build and the
tests.

**Eight manual cases were never executed.** They required confirmation from the
school — checking a settings screen, observing a first-time visitor, confirming
a phone line is monitored during office hours. They are recorded as untested
rather than assumed to pass.

**No user testing with actual parents.** The site's entire objective is how a
parent responds to it, and no parent was observed using it. The UX review cases
are my judgement, which is the least reliable evidence in this document.

**Claim verification was manual and depends on the client's word.** Two
figures the school supplied did not match what public education directories
record. Both were raised with the school rather than published unchecked or
silently altered, and both were confirmed before release. That is the right
process, but the evidence is a verbal confirmation and not a document — which
is a weaker artifact than it should be for claims a parent may act on.

**No production monitoring.** Nothing currently alerts if the site goes down,
if the form stops delivering, or if enquiries stop arriving.

## What I would change

**Get written acceptance, however informal.** The gap between "I tested it and
it works" and "the client confirmed it meets the need" is the gap between a
build and a delivery.

**Verify externally checkable facts at requirements stage.** The figures the
school supplied should have been checked against public records when the copy
was written, not when it was being tested. Checking a client's own published
numbers is a twenty-minute task early and a credibility problem late.

**Test on real devices earlier.** Four of the eight failures were
device-specific and surfaced late, when the design was already settled. Two
cheap physical devices in the loop from the first build would have caught them
when they were still cheap to fix.

**Instrument the thing being optimised.** The objective was more enquiries.
There is no analytics baseline from the previous site, so the objective cannot
be evaluated. Measurement should have been the first item built, not an
afterthought.
