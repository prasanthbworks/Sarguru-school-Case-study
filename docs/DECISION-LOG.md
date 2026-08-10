Decision Log

Decisions taken during the Sri Sarguru website rebuild, the options weighed against each, and the trade-off accepted in each case.

This is written as a record of reasoning, not of outcomes. Some of these decisions may turn out to be wrong; where I think one is likely to need revisiting, it says so.

A note on dating. This log was compiled in August 2026, after the build, from working notes and commit history. The decisions are real and were taken at the points described. The document is not contemporaneous and does not pretend to be.

D-01 · Two pages instead of eight

Decision. Consolidate the previous site's eight pages into a home page and an Our School page.

Options considered. Rebuild the existing eight-page structure; a middle option of four pages; a single long-scrolling page.

Reasoning. Most of the original pages carried about one screen of content. A parent researching on a phone will not tap through eight of those, and thin pages read as an unfinished site. Consolidating produced two dense pages that feel deliberate. A single page was rejected because the school's story and its enquiry path serve different intents and deserve separate entry points.

Trade-off accepted. Longer pages, and a slightly harder job keeping each one coherent. Also weaker search-engine surface area — eight pages can rank for more queries than two. For a school competing on local reputation rather than search traffic, that was an acceptable loss.

D-02 · Static HTML over a website builder or CMS

Decision. Hand-code static HTML, CSS and JavaScript. No framework, no build step, no dependencies.

Options considered. Staying on a paid Wix plan; another builder such as Squarespace or WordPress; a static site generator; a React application.

Reasoning. The site is two pages of largely fixed content with no authenticated users and no dynamic data. Every dependency added is something that can break later at a school with no technical staff to fix it. A framework would have added a build step and a toolchain for no functional gain.

Trade-off accepted. This is the most consequential trade-off in the project, and it points directly at D-12. Content changes require editing HTML and pushing to the repository. The site is cheap, fast and durable, and it is not self-service.

Would revisit if. The interim maintenance arrangement ends, or the school decides it wants to publish notices itself.

D-03 · Vercel over a paid builder plan

Decision. Host on Vercel's free tier, deploying from the repository.

Options considered. Wix's cheapest domain-capable plan, around ₹1,300–4,900 per month depending on tier; Netlify; GitHub Pages; shared hosting.

Reasoning. For a static site, the free tier covers everything needed: custom domain, HTTPS, automatic deployment on push. Netlify would have served equally well; Vercel was chosen on familiarity, which is an honest reason. The recurring cost saved is real money for a school.

Trade-off accepted. Two, and both are worth naming. The hosting tier is intended for non-commercial use, and whether a school's marketing site qualifies is a question worth resolving before anyone describes the hosting as permanently free. And no recurring cost means no commercial relationship, which means no vendor obligation if something breaks.

D-04 · Keep the domain at its existing registrar

Decision. Leave the domain registered where it was and repoint DNS, rather than transferring the registration.

Options considered. Transferring to the hosting provider or a dedicated registrar for consolidated management.

Reasoning. The domain was already school-owned — the single asset in the whole project that was — and it renews in 2028. A transfer would have triggered a sixty-day lock and a change of ownership record for no benefit. Repointing DNS achieves the same end state in an afternoon.

Trade-off accepted. Domain and hosting live in different places, which is one more thing to explain in a handover. Worth revisiting at renewal.

D-05 · Repoint DNS record by record, leaving mail untouched

Decision. Edit individual DNS records at the registrar rather than delegating the zone to the host's nameservers.

Options considered. Pointing nameservers at the host, which is the simpler and more usual approach.

Reasoning. Two forcing factors. The registrar does not permit external nameservers. And the zone turned out to contain a live Microsoft 365 mail configuration that nobody at the school had documented — delegating the zone would have dropped those records and taken down the school's email.

Trade-off accepted. Manual, error-prone record editing instead of a single delegation. The right call: the blast radius of a mistake here was the school's email during admission season.

Lesson generalised. Enumerate what depends on a resource before migrating it. The dangerous dependencies are the undocumented ones, and the only way to find them is to look.

D-06 · A hosted form service instead of building one

Decision. Embed a hosted enquiry form rather than writing a form with a backend.

Options considered. A custom form with a serverless function and an email service; a mailto link; a WhatsApp-only enquiry path.

Reasoning. A static site has no backend. Adding one for a single form means adding a function, an email provider, spam handling and a maintenance surface, all for a form that collects five fields. A hosted service does this for free and immediately. A mailto link was rejected because it fails on most phones and loses the enquiry silently.

Trade-off accepted. Three, and they compound. Enquiry data now sits with a third party. Notification routing is limited by the free tier (see D-07). And a validation defect inside the embed cannot be fixed from this codebase — a phone-number field accepts invalid input on iOS, and that is not fixable here. That last point is the real cost of the decision: I gave up the ability to fix my own bug.

D-07 · Not upgrading the form service to its paid tier

Decision. Stay on the free tier despite the notification-routing limitation.

Options considered. The paid tier at roughly $23 per month, which would allow arbitrary notification recipients and team access.

Reasoning. The upgrade solves exactly one problem the school has, and bundles it with features it does not need. A forwarding rule solves the same problem for nothing, and rebuilding the form under a school-owned account solves it permanently for the cost of one line of HTML.

Trade-off accepted. The interim state is unsatisfying — enquiries notify a personal address until someone at the school completes a verification step. Recommending a subscription would have been easier than explaining why not to buy one.

Explicitly rejected. Accepting a workspace invitation on the free tier. It is a one-way action that cannot be undone without the paid plan.

D-08 · Click-to-play video facades instead of embedded players

Decision. Render a lightweight placeholder for each community video and load the real player only on click.

Options considered. Standard embedded iframes; self-hosted video files.

Reasoning. Two defects had the same root cause. Multiple embedded players could run simultaneously, and playback intermittently failed to start. Loading players on demand fixed both, and removed two third-party players from initial page weight — which matters on the mobile connections most parents will use.

Trade-off accepted. One extra tap before a video plays. Slightly more JavaScript to maintain.

D-09 · The school's own photographs only

Decision. No stock imagery anywhere on the site.

Options considered. Supplementing thin areas with licensed stock.

Reasoning. A parent in Coimbatore can tell the difference between children they might recognise and a stock photograph of a classroom in another country. Stock imagery on a school site actively destroys the trust the site exists to build. The archive was thinner and less polished, and that was the point.

Trade-off accepted. Substantially more work. Roughly 37 photographs needed uniform colour grading so the galleries read as one archive rather than a decade of different cameras; three needed watermarks cropped; one entire shoot was excluded for falling below standard.

D-10 · No photograph appears twice

Decision. Every content photograph is used once across the whole site.

Options considered. Reusing the strongest images across sections.

Reasoning. Repetition on a small site reads as a thin archive, which undercuts the impression of an active school.

Trade-off accepted. Weaker images had to carry sections where a repeat would have looked better. One exception was needed: a photograph relevant to both pages. Rather than repeat it, institutional marks were extracted from a photograph of the school's own signage for one page, leaving the photograph itself on the other. The client approved the exception before it shipped.

D-11 · Positioning: premium, never charitable

Decision. Communicate quality, trust and aspiration. Never frame students through a need-based or charity lens.

Options considered. A community-and-affordability angle, which is a common register for schools of this type.

Reasoning. Parents are choosing where to invest in their child's future. Messaging that positions the school as somewhere for families without other options gives every reader a reason to keep looking. A nearby school was used as a quality benchmark, but explicitly as a standard rather than a template — copying its identity would have produced a worse imitation of a competitor instead of the school's own presence.

Trade-off accepted. Higher expectations. A site that reads premium must be executed to that standard everywhere, which is what drove D-09 and D-10.

D-12 · Accepting an interim maintenance arrangement with no end date

Decision. Ship V1 as a static site on the basis that content updates would be handled from my side for the foreseeable future, and record that no successor arrangement exists.

Options considered. Adding a CMS pre-emptively. Delaying launch until the school committed to a long-term maintenance owner. Recommending a builder despite the cost, purely for self-service editing.

Reasoning. The maintenance question was asked at requirements stage and the answer was that updates would come from my side for now. That answer is what makes D-02 defensible — a static site is the right call when the person editing it is comfortable editing HTML. Building a CMS for an organisation with nobody to operate it would have added a system that nobody would use.

Trade-off accepted. The architecture depends on my continued availability, and "for now" has no end date. If that arrangement lapses before a successor is found, the school owns a site it cannot change. This is the same risk as the account ownership problem and it should be resolved at the same time.

Would revisit if. The school takes on content maintenance itself, or the interim arrangement ends. In either case the honest recommendation is a builder, even though that means recommending against the delivered solution.

D-13 · Fixing v3 rather than shipping and patching

Decision. After the v3 test cycle returned eight failures, eight untested cases and thirteen issues, build v4 against the issue list before release.

Options considered. Launching v3 — which looked finished — and fixing issues in place afterwards.

Reasoning. The failures were not cosmetic. They included a navigation indicator showing the wrong section, videos playing over each other, and a form that intermittently failed to load on first visit. Launching with those meant a parent's first impression of a site built for credibility would be a site that visibly did not work.

Trade-off accepted. A delayed launch. Eight manual cases remain untested because they required the school's participation and were not returned; those are recorded as open rather than assumed to pass.
