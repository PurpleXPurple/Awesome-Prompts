Nexus-Design.md

name: nexus-design
description: The unified frontend creation skill. Combines anti-slop art direction, official Anthropic frontend design, cinematic motion and media generation, browser-in-the-loop agentic website building, and production UI engineering into one end-to-end system. Use this whenever you build, redesign, restyle, polish, or review any public-facing web page, landing page, marketing site, portfolio, product page, dashboard, admin view, or app screen — in any stack. Also use when the user says a page looks generic, templated, "AI-made," or asks for premium, award-level, or production-quality output.
version: 1.0.0
sources: tasteskill v2 (Leonxlnx), frontend-design (Anthropic), frontend-dev (MiniMax-AI), agentic-website-design (NeverSight/LobeHub), frontend-ui-engineering (Addy Osmani).

---

0. How to use this skill

This skill replaces default model behavior — the centered hero, three icon cards, a soft gradient, and a monospace caption — with deliberate decisions. Nothing in a default AI page is wrong, but nothing in it was decided. Nexus-Design forces decisions at every layer: concept, composition, type, palette, motion, engineering, and verification.

Decide first which kind of work this is.

A page is read and scrolled: a landing page, marketing site, portfolio, event page, restaurant site, one-pager. It follows the page run.

An app screen is worked in: a dashboard, admin or analytics view, settings, billing, inbox, queue, wallet, filtered list, editor, or builder. It follows its own rules: quiet borders, small labels, repeated panels are normal; gradients, glows, tinted icon squares, rainbow charts, and invented vanity numbers are not. Read the app-screen references before starting; the page run does not apply wholesale. A product site that shows its app is still a page; the interface fragments on it follow app-screen rules for their insides.

When a request mixes both (a landing page and the dashboard behind it), build each with its own run and one shared palette and typeface.

Stack selection. Default to Astro. It ships zero JavaScript by default, supports any UI framework, and gives you full template control. Use Next.js when you need SSR, API routes, or the React ecosystem. Use Nuxt for Vue, SvelteKit for Svelte, 11ty for the simplest static sites. Never use no-code tools like Framer or Webflow: you must own every line of HTML, CSS, JS, and content.

---

1. Phase 0 — Read the brief and set the dials

1.1 The design read

Before touching code, state a one-line design read. If the brief is vague, decide and state your reading instead of asking. Ask only when a missing fact blocks the work (for example, no product name).

Then set three dials on 1–10 scales. These govern every downstream decision and may be adapted only when the user explicitly requests a different intensity.

Dial Default Range meaning
DESIGN_VARIANCE 8 1 = strict symmetry; 10 = maximal asymmetry
MOTION_INTENSITY 6 1 = static; 10 = cinematic
VISUAL_DENSITY 4 1 = airy; 10 = packed

1.2 The brief is the first constraint

Where the brief pins down a visual direction, follow it exactly. The brief's own words always win, including when it asks for a look that this skill would otherwise ban. Where the brief leaves an axis free, do not spend that freedom on a default.

---

2. Phase 1 — Inventory the subject

Before any design, find out what is real: the subject, the audience, the one thing the page must get someone to do, the stack, any existing brand (colours, fonts, logo), and above all what imagery truly exists — the project's own photographs or product shots, a real interface that can be shown, an image generation tool you can call, or nothing. Do not plan around pictures you do not have.

Then inventory the subject in your notes: eight to twelve things that belong to this client and to no competitor. Look for:

· Its name and what the name means.
· Its address, door number, street, neighbourhood, and what that place looks like — tiles, signs, paint colours, typefaces on shop fronts, light, weather.
· The numbers in the brief: seats, rooms, minutes to the sea, price, year.
· The constraint it lives with: only one batch a month, closed in winter, by appointment.
· Its materials and colours.
· Its rituals and hours: the oven lit at five, the ferry at seven.
· The printed things of its world: a timetable, a label, a ticket, a sign.

The competitor test. Put the nearest real competitor in the sentence that describes your emerging idea. If the sentence still works, the idea belongs to the category, not the client. "A loaf of bread" is every bakery; "the seven doors of the street this one is named after" is only this one. "Clean and modern" is not an idea.

This list is also your picture search. Each place, material, and printed thing on it is a query for the asset phase.

---

3. Phase 2 — Write the argument and the copy deck

3.1 Claims with proof

Before any design, list the page as claims with their proof, one line per section. The claims decide which sections exist and in what order. Delete any section that has no claim. This is how you avoid the stock sequence of hero, logos, three features, testimonial, pricing, call to action.

3.2 Copy frameworks

Use AIDA, PAS, or FAB depending on the page type.

· AIDA (landing pages, emails): Attention — bold headline with promise or pain. Interest — elaborate the problem ("yes, that's me"). Desire — show the transformation. Action — clear CTA.
· PAS (pain-driven products): Problem — state clearly. Agitate — make urgent. Solution — your product.
· FAB (product differentiation): Feature — what it does. Advantage — why it matters. Benefit — what the customer gains.

3.3 Headline and CTA rules

Every headline fits in two lines or fewer at every width. The studied gallery heroes have a median of five words and 32 characters. About 32 characters is safe everywhere at a confident size.

One supporting sentence sits directly under the headline (32–48 px below it) and is shown with it — never docked at the foot of the first screen or held back by an animation. When the headline is a name or a figure of speech, this sentence says plainly what the thing is.

One primary action, named for what it does, as a real button on the first screen. A CTA says exactly what happens when it is used: "Save changes," not "Submit." An action keeps the same name through the whole flow: the button that says "Publish" produces a toast that says "Published."

Bad CTAs: Submit, Click here, Learn more. Good CTAs: "Start my free trial," "Get the template now," "Book my strategy call." Formula: action verb + what they get + urgency or ease.

3.4 Writing as design content

Words appear in a design for one reason: to make it easier to understand and use. They are design content, not decoration. Bring the same intentionality and minimalism to copywriting that you bring to spacing and color.

Write from the end user's perspective. Name things by what users understand in simple language, not by how the system is built. A user manages notifications, not webhook config. Use active voice as default. Explain what went wrong and how to fix it in errors; an empty screen is an invitation to act. Keep tone conversational: plain verbs, sentence case, no filler, tone matched to brand and audience. Let each written element do exactly one job.

3.5 Proof is never invented

No customers, quotes, reviews, press, logos, or business statistics may be invented. Sample details are expected where the brief leaves gaps (dish names, room names, rates, project titles) so the design is seen at full density; list them in the report for the client to confirm. But proof is never invented.

---

4. Phase 3 — Draw the art direction

When asked to invent a look, a model picks the category default every time. This skill replaces invention-by-default with selection from a researched library.

Deal three directions from a distilled library of real, high-scoring sites. Hold each one against your inventory: which way of composing can carry something only this client has? Choose that one, not the most familiar. Write one line in your notes on why.

Then deal the full direction sheet. It is the art direction for this build: palette roles, type system, hero composition, and section blueprints.

Adopt it. Do not blend it with the layout you would have made anyway. If it feels unfamiliar, that is the point; the familiar layout is the one this skill exists to prevent.

Redraw only for a reason you can state: it contradicts a stated brand constraint, or needs an asset that does not exist. Never redraw to get something more comfortable. Record every redraw in your report.

Check the palette against the subject. A palette is a set of proportions — one ground, one ink, one accent, fields — as much as a set of colours. If its colours fight the subject's own material (candy pastels for a peat whisky, neon for a mountain hut), redraw once. If the subject has unmistakable colours of its own (its material, the paint of its street, the landscape outside its windows), keep the dealt proportions and take the colours from the subject instead, and say so in the report.

---

5. Phase 4 — Find the idea

The direction is a way of composing. The idea is what makes the page belong to this subject and no other. Write this sentence in your notes and do not start building until both blanks are filled from the subject itself:

On this page, [something that belongs to the subject] becomes [the device the direction calls for].

Places to look for the first blank: its output (what does the thing produce? put the real output on the page as material), its object or shape (enlarge it, crop it, make it the frame or the one motif), its name (can the letters hold a photograph, stack into a wall, split to two corners?), its place (is there one view that makes someone want to be there?), its ritual (is there a sequence the page can perform as it scrolls?), its artefact (what printed thing already exists in its world: ticket, menu, label, receipt, invitation, folder, manual?), its opposite (which other category owns the feeling this one lacks?).

One live thing. Give the page one thing the visitor can do, or that is different for each visit, made from the idea: pick a wood and see the chair in it; type your name and see it cut in the foundry's letters; set the loan and read the real monthly figure; see whether the hut's stove is lit right now and how long until the last ferry. Rules: it uses the subject's real content, it works on a phone with a thumb, it has a plain fallback without script, it never blocks reading, and it is one thing, not a playground.

The scroll scenes carry the idea too. Decide what each scene shows from your inventory before building. A scene filled with pictures that could belong to anyone is a screensaver.

---

6. Phase 5 — Build the page

6.1 Tokens first

Colour and type roles from the sheet become CSS custom properties. Everything downstream references tokens only. A neutral foundation may provide reset, token names, fluid type steps, grid, focus and selection styles, and motion helpers, but it has no look of its own.

6.2 Hero exactly as the blueprint describes

The hero is the first thing viewers see. Open with the most characteristic thing in the subject's world, in the most appropriate form: a headline, an image, an animation, a live demo, an interactive moment, or another treatment. A big number with a small label, supporting stats, and a gradient accent is the default treatment — only use it if it is truly the best option.

Get the big thing big and the small things small before anything else exists. The hero media covers at least half the first screen when a real asset exists. Most strong heroes have no button at all, or one.

6.3 Section blueprints

Each section from its blueprint, with this project's content. Keep the geometry, change the subject. Neighbouring sections must not share a layout. Carry the idea into every section, pricing, FAQ, and footer included: the commonest failure in blind testing, even among good sites, is a distinctive first screen followed by stock blocks.

Every section the visitor came for — the line-up, the menu, the rooms, the price — has its own heading and its own place in navigation.

6.4 Imagery hierarchy

Use imagery in this order of preference:

1. The project's own photographs, product shots, or footage.
2. Generated assets, if an image tool is available.
3. Real pictures of the subject's world from open collections (Openverse, Wikimedia Commons, Art Institute of Chicago, the Met), only with licences that allow commercial use. Search the inventory, not the category: its street in old postcards, an old map of the neighbourhood, its material up close, its trade in engravings, a museum object of its kind, a botanical plate, a star chart. Use them large and with intent. A found picture shows the world around the client and is never presented as the client's own product, room, dish, person, or team.
4. Drawn in code from the subject itself, for what no honest picture exists of — its objects, tiles, windows, or product interface — made as one consistent flat graphic language in the palette's real colours.
5. Type and colour alone.

Floor: at least four real pictures on the page, usually six to twelve (photographs, archival images, maps, engravings, footage, raster artwork; for software, cropped interface fragments count). SVG drawings do not count toward the floor.

Never use placeholder URLs (unsplash, picsum, placeholder.com, via.placeholder, placehold.co, lorem.space, dummyimage) or external image URLs in production. Download every found picture into the project, credit it on the page, and list it in the report. Never write a sentence on the page about where the pictures come from or what they are not.

6.5 Fullness and the frame

Look at each desktop screen as a poster: one dominant thing and the content it carries. A screen that is mostly empty ground is an underbuilt section, not a rest. One deliberate rest per page is the limit.

Running text is in the full ink at 16 px or more; grey is for secondary notes. The page has one pair of equal outer margins at every width, and every section uses the width the page has set up. Asymmetry lives inside the grid, never in the frame. A void is placed only when something closes it on the far side. No stretch of the page taller than about half a screen holds nothing at all. Sections are 120–240 px apart on desktop.

Every section opens with a heading that names it, and the first screen carries the page's title in an h1.

6.6 States and semantics

Real links and buttons, visible focus, alt text, a phone layout that is its own composition. Always implement loading (skeleton), empty, error, and tactile feedback states. Forms: label above input, error below, gap-2 for input blocks.

---

7. Phase 6 — Motion engine

Motion is required, and it is more than one fade. The motion layer has four parts: the signature move named on the sheet (one, done well); an entrance for the parts of every section (heading, then text, then picture, staggered by 60–90 ms, rising 16–32 px with opacity, triggered once as the section enters); feedback on every control (press, hover, focus); and one moving picture (a found clip as a muted loop, a slow scale or drift on a photograph, or a scroll-linked move on the hero picture).

7.1 Tool selection matrix

Need Tool
UI enter/exit/layout Framer Motion — AnimatePresence, layoutId, springs
Scroll storytelling (pin, scrub) GSAP + ScrollTrigger — frame-accurate control
Looping icons Lottie — lazy-load (~50 KB)
3D/WebGL Three.js / R3F — isolated, own client boundary
Hover/focus states CSS only — zero JS cost
Native scroll-driven CSS — animation-timeline: scroll()

Conflict rules: never mix GSAP and Framer Motion in the same component. R3F must live in an isolated Canvas wrapper. Always lazy-load Lottie, GSAP, and Three.js.

7.2 Intensity scale

Level Techniques
1–2 Subtle CSS transitions only, 150–300 ms
3–4 Smooth CSS keyframes + Framer animate, stagger ≤3 items
5–6 Fluid whileInView, magnetic hover, parallax tilt
7–8 Cinematic GSAP ScrollTrigger, pinned sections, horizontal hijack
9–10 Immersive Full scroll sequences, Three.js particles, WebGL shaders

7.3 Performance rules

Animate GPU-only properties: transform, opacity, filter, clip-path. Never animate width, height, top, left, margin, padding, or font-size — use transform: scale() or clip-path instead. Perpetual animations must be in React.memo leaf components. Use will-change: transform only during animation. Use contain: layout style paint on heavy containers.

Respect prefers-reduced-motion always. Disable parallax and 3D on pointer: coarse. Cap particles: desktop 800, tablet 300, mobile 100. Disable GSAP pin on mobile below 768 px. Every useEffect with GSAP or observers must return a cleanup: () => ctx.revert().

7.4 Motion accessibility

Wrap motion in prefers-reduced-motion checks. Never flash content more than three times per second (seizure risk). Provide visible focus rings using outline, not box-shadow. Add aria-live="polite" for dynamically revealed content. Include a pause button for auto-playing animations.

---

8. Phase 7 — Assets and media generation

When an image, video, audio, or music generation tool is available, generate all media assets locally. Never proceed to the build phase until all assets are saved. Craft an optimized prompt, show it to the user, and confirm before generating. Be specific about composition, lighting, and style. Never include text in image prompts.

Asset naming: {type}-{descriptor}-{timestamp}.{ext}. Save to the framework's public assets directory: Pure HTML ./assets/, React/Next.js public/assets/, Vue/Nuxt public/assets/, SvelteKit static/assets/, Astro public/assets/.

Post-process: Images to WebP, videos compressed with ffmpeg, audio normalized.

Preset shortcuts: hero = 16:9 cinematic text-safe; thumb = 1:1 centered subject; icon = 1:1 flat clean background; avatar = 1:1 portrait circular-crop-ready; banner = 21:9 OG/social; bg-video = 768P 6s static shot; video-hd = 1080P 6s; bgm = 30s no vocals loopable; tts = HD MP3.

Generative art. Philosophy-first workflow. Name the movement in one or two words. Articulate the philosophy in four to six paragraphs covering space, form, color, scale, rhythm, and hierarchy (or computation, emergence, noise, and parametric variation for interactive work). Identify a subtle, niche conceptual seed — sophisticated, not literal. For static mode: single page, highly visual, repeating patterns, perfect shapes, sparse typography, nothing overlaps, proper margins. For interactive mode: keep fixed sections (header, sidebar, seed controls), replace variable sections (algorithm, parameters), use seeded randomness, and output a single self-contained HTML file. Refine, don't add. Polish into a masterpiece.

---

9. Phase 8 — Engineering quality

9.1 Component architecture

Colocate everything related to a component: implementation, tests, stories, custom hook, and types. Prefer composition over configuration. Keep components focused: one thing each. Separate data fetching from presentation. Components with more than 200 lines should be split.

9.2 State management

Choose the simplest approach that works: local state for component-specific UI, lifted state for two or three siblings, context for theme/auth/locale (read-heavy, write-rare), URL state for filters and pagination, server state (React Query, SWR) for remote data with caching, global store (Zustand, Redux) for complex client state shared app-wide. Avoid prop drilling deeper than three levels.

9.3 Design system adherence

Use semantic color tokens: text-primary, bg-surface, border-default — not raw hex values. Use a consistent spacing scale: 0.25 rem increments, or whatever the project uses. Do not invent values. Respect the type hierarchy: h1 page title (one per page), h2 section title, h3 subsection title, body default text, small secondary/helper text. Do not skip heading levels. Do not use heading styles for non-heading content.

9.4 Responsive design

Design mobile-first, then expand. Test at 320 px, 768 px, 1024 px, and 1440 px. Use min-h-[100dvh], not h-screen. Use CSS Grid, not flex percentage math. Use max-w-[1400px] mx-auto or max-w-7xl for layout.

9.5 Loading and transitions

Use skeleton loading for content, not spinners. Handle optimistic updates for perceived speed. Provide meaningful empty and error states.

9.6 Rationalization table

Rationalization Reality
"Accessibility is a nice-to-have" It is a legal requirement in many jurisdictions and an engineering quality standard.
"We'll make it responsive later" Retrofitting responsive design is 3x harder than building it from the start.
"The design isn't final, so I'll skip styling" Use the design system defaults. Unstyled UI creates a broken first impression.
"This is just a prototype" Prototypes become production code. Build the foundation right.
"The AI aesthetic is fine for now" It signals low quality. Use the project's actual design system from the start.

---

10. Phase 9 — Accessibility (WCAG 2.1 AA)

Every component must meet these standards.

Keyboard navigation. Every interactive element must be keyboard accessible. Native buttons and links are focusable by default. If you build a custom control, add keydown handling for Enter and Space.

ARIA labels. Label interactive elements that lack visible text. Label form inputs. Use aria-label when no visible label exists.

Focus management. Move focus when content changes. Trap focus inside dialogs when open. Return focus when the dialog closes.

Meaningful empty and error states. Do not show blank screens. Explain what happened and what to do next.

Color contrast. Ensure sufficient contrast: 4.5:1 for normal text, 3:1 for large text. Do not rely solely on color to convey information; use icons, text, or patterns too.

Motion. Respect prefers-reduced-motion. Provide visible focus rings. Do not flash more than three times per second. Use aria-live for dynamic content. Pause auto-playing animations.

---

11. Phase 10 — Browser inspection and iterative improvement

11.1 Browser-in-the-loop

Without browser access, you are coding blind. After every meaningful change: save files and wait for hot reload, screenshot the page and capture the full viewport, inspect the result for layout, spacing, colors, typography, identify issues (misalignment, overflow, broken responsive, missing states), fix, and re-screenshot. Repeat until the section looks correct.

11.2 The improvement loop

Generate the initial version. Screenshot and evaluate. Identify the weakest element. Improve that element specifically. Screenshot and compare. Repeat at least three more times. Final polish pass.

After every change, ask: "Now improve what you just did." "Make the animations smoother and more intentional." "This is good, but push it further — what would a top design agency do differently?" "Add subtle micro-interactions that make this feel premium." "The spacing feels off — tighten it up and make it breathe properly."

Never settle on the first version. Push for at least three rounds of improvement on each section. Work section by section, fully polishing each before moving on. Hero gets the most time. Social proof must feel credible. Features must be benefit-driven with supporting visuals.

11.3 Automated inspection

Run a lint pass that scans source for banned patterns and stock phrases. Run an inspector that opens the page in a real browser at 1440, 768, and 390 px, measures headline line counts, type sizes, fonts actually loaded, colours, rules, shadows, gradients, contrast, overflow, and the frame (equal margins, sections huddled on one side, blank stretches, title presence, untitled sections, text against the screen edge), and saves screenshots. Fix every FAIL. Treat each WARN as wrong unless you can defend it to a designer in one sentence. Re-run until there are no FAILs. Do not argue with the meter and do not hide things from it.

11.4 Look and critique

Passing the gate means nothing banned was measured. It does not mean the page is good. Open every screenshot and critique it with a juror's rubric. Write the critique down. Fix the top three problems, run the inspector again, look again. Three rounds at most. Judge only the pictures. If a problem is visible in a screenshot, it is real, whatever the code says.

---

12. Phase 11 — Quality gates and delivery

Do not report until all of these are true:

· The inspector shows zero FAIL at 1440, 768, and 390 px on the final version.
· You opened every screenshot it listed after the last change, not only the first screen.
· With scroll scenes, every scene was photographed at 0, 10, … 100% of its hold on desktop, phone, and reduced motion; no scene never started or rests.
· A stranger reading the first screen for five seconds can say what this is and what to do.
· Every section has a heading; the margins match on both sides; no stretch of the page is empty.
· The critique of your last round is written down.
· Accessibility: keyboard accessible, screen-reader conveyable, responsive at 320, 768, 1024, and 1440 px, loading/empty/error/success/permission states handled, design-system compliant, no accessibility warnings in dev tools or axe-core.
· Motion: correct tool per the matrix, no GSAP + Framer mixed in the same component, all effects have cleanup returns, prefers-reduced-motion respected, perpetual animations in React.memo leaves, only GPU properties animated, heavy libraries lazy-loaded.
· Assets: no placeholder URLs anywhere; all media exist as local files in the project's asset directory; asset prompts confirmed with the user before generation.

Report briefly: the direction dealt and any redraws with reasons; the idea sentence and the live thing; the final critique scores and what still falls short; every found picture and clip with its title, creator, licence, and source page, and where its credit sits on the page; which bundled photographs and drawings stand in for photography; and any sample content that must be swapped before launch. Do not claim a check you did not run.

---

13. Anti-slop registry

These patterns are banned unless the brief explicitly demands them. They are the commonest tells of a generated page.

Typography tells

· Accenting just a single word or phrase in a headline (one word italic, bold, or a different colour).
· Using all caps for labels.
· Adding unnecessary typographic labels above content (eyebrows, pill badges, "New ·" chips).
· Numbered markers (01 / 02 / 03) unless the content actually is a sequence.
· Monospace outside real code blocks.
· Inter as the default. Use Geist, Outfit, Satoshi, or a deliberate alternative.
· Serif on dashboards.
· Oversized H1s.
· Five sizes on a page is the ceiling; one family is normal, two is the limit.
· Display type tracking should be -0.02 to -0.05 em, leading 0.85–1.05. Text should be 16–20 px, leading 1.45–1.6, 45–75 characters per line.
· Left-aligned by default. Centre at most one moment.
· Default line lengths under 80 characters. Serif body text can be slightly longer with slightly more line-height.

Colour tells

· Purple or indigo everything. Models default to visually "safe" palettes.
· Warm cream background (near #F4F1EA) with a high-contrast serif and a terracotta or warm-clay accent.
· Near-black background with a single bright acid-green or vermilion accent.
· Gradients as backgrounds, on buttons, or in text. A neutral scrim under text on a photo is the one exception.
· Neon glows, oversaturated accents, gradient text on headers.
· Pure black (#000).
· The assistant palette: yellow-cream ground with terracotta or rust; violet or indigo.
· More than one accent. Stick to one palette with maximum one accent, saturation under 80%.

Layout tells

· Centered hero with three lines, a little label above it, a row of icon cards, a soft gradient, thin rules, and a monospace caption.
· Three-column equal card rows.
· Stock card grids where every card is identical.
· Floating elements with awkward gaps.
· SaaS-card kit: content chopped into identical rounded cards, one border-radius on everything regardless of hierarchy, the same soft grey shadow under each, and gradient washes as decoration.
· Broad sheet-style layout with hairline rules, zero border-radius, and dense newspaper-like columns — only if the brief truly calls for it.
· Template chrome: tracked-out all-caps eyebrow label above every heading; meta strings joined with middle dots; labels built as "WORD — fragment" with a spaced em dash; tinted near-black standing in for black; a monospace face for small data labels; a "→" appended to link and button text.
· Oversized padding everywhere. Equal generous padding destroys visual hierarchy.
· Shadow-heavy design. Layered shadows compete with content and slow rendering.

Component tells

· Icon-topped feature cards.
· Rows of identical cards.
· Glassmorphism as a default (backdrop-blur panels, floating pill navigation).
· Emoji anywhere. Use Phosphor or Radix icons only.
· Custom cursors.
· Default shadcn/ui without customization.
· Buttons with a soft 9–16 px radius. Choose flat, one fill colour, either square-cornered or fully round, and decide once.
· Fake dashboards of grey bars.
· Decorative shapes, gradient blobs, or assistant-style ornaments.

The AI aesthetic table

AI Default Why it is a problem Production quality
Purple/indigo everything Models default to visually "safe" palettes; every app looks identical Use the project's actual colour palette
Excessive gradients Adds visual noise and clashes with most design systems Flat or subtle gradients matching the design system
Rounded everything (rounded-2xl) Maximum rounding signals "friendly" but ignores the hierarchy of corner radii Consistent border-radius from the design system
Generic hero sections Template-driven layout with no connection to content or user need Content-first layouts
Lorem ipsum-style copy Placeholder text hides layout problems that real content reveals Realistic placeholder content
Oversized padding everywhere Equal generous padding destroys visual hierarchy and wastes screen space Consistent spacing scale
Stock card grids Uniform grids ignore information priority and scanning patterns Purpose-driven layouts
Shadow-heavy design Layered shadows add depth that competes with content and slows rendering on low-end devices Subtle or no shadows unless the design system specifies

---

14. Laws of the craft

Numbers are from a study of 247 curated gallery sites unless marked as house style.

Type

Two or three sizes do the visible work: one very large, one small, nothing in between. Median headline-to-body ratio on the studied sites is 4.3; the best go far beyond (largest type: median 8.4 vw on top sites against 5.0 vw on weak ones).

Display type: tracking -0.02 to -0.05 em, leading 0.85–1.05. Text: 16–20 px, leading 1.45–1.6, 45–75 characters per line.

Five sizes on a page is the ceiling. One family is normal (a third of the studied sites); two is the limit.

Left-aligned by default. Centre at most one moment.

Space

One grid, one pair of tight margins (16–32 px), used everywhere; large things run to the edges. Text blocks share two or three left edges on the whole page.

Asymmetry inside the grid, never in the frame: the two outer margins are equal, and an off-centre heading is answered by something at the far side of the grid.

The gap between sections is clearly larger than any gap inside one: 120–240 px on desktop (the study's median is about 240).

Each section has a different composition from its neighbours; none is a card grid.

Colour

One ground, one ink, one accent. The accent appears on the action and perhaps one other thing.

Running text in the full ink. A page set in soft grey reads unfinished.

Surfaces are separated by a small step in tone, never by a line or a shadow.

A section may change the whole field colour. Nothing fades into anything.

Imagery

One dominant image beats six small ones. On the top-scored sites, hero media covers at least half the first screen 68% of the time.

Same crop ratios, same treatment, square corners unless the direction says otherwise (73% of the studied sites leave media square).

No image is better than a placeholder, an icon, or an abstract shape. A picture drawn for this subject, in its colours and with care, is better than either. A real picture of its world, found and credited, is better still.

Every found picture is downloaded into the project, credited on the page, and listed in the report. None is presented as the client's own.

Nothing ever lies on top of a headline or a statement.

Components

Buttons: flat, one fill colour, either square-cornered or fully round, decided once (top sites: 52% square, 23% pill; the 9–16 px "soft" radius is the weak-site default). Most strong heroes have no button at all, or one.

Navigation: wordmark, three to five plain links, one action. No bar full of icons.

No thin rules, hairline dividers, 1 px outlined boxes, grid lines, crosshairs, or crop marks. Use space, a step in background tone, a change of scale, or a filled shape.

No shadows on things at rest. No glows. Use a tone step between surfaces. Shadows only on what truly floats: a menu, a dialog.

No glass. No backdrop blur panels. No floating pill navigation.

---

15. Reference files

Read these when the detail behind a decision is needed. They are part of this skill's system.

· references/app-screens.md — run and rules for dashboards, admin, settings, inboxes, and other worked-in screens.
· references/copy.md — claim/proof format, headline arithmetic, full copy deck guidance.
· references/imagery.md — searching, licences, credits, video, drawing, crops, treatments.
· references/scenes.md — how to put a scroll scene on a page, fill it, fit it, and check it.
· references/type.md — type system detail.
· references/colour.md — palette proportion detail.
· references/layout.md — grid and frame detail.
· references/motion.md — motion tokens and tested recipes.
· references/critique.md — the fourteen passes, the weighted score, the caps.
· references/tells.md — the dated registry of anti-slop patterns.
· references/motion-recipes.md — full code snippets for scroll reveal, stagger grid, pinned timeline, tilt card, magnetic button, text scramble, SVG path draw, horizontal scroll, particle background, and layout morph.
· references/minimax-cli-reference.md — CLI flags for asset generation.
· references/asset-prompt-guide.md — asset prompt engineering rules.
· references/accessibility-checklist.md — detailed accessibility requirements and testing tools.

---

16. Final instruction

This skill exists to prevent one outcome: a page that is technically correct and completely forgettable. Every phase above is a decision point. The design read, the inventory, the argument, the direction, the idea, the live thing, the frame, the motion, the inspection, the critique — none of these are optional.

When the model is tempted to default to the familiar centered hero, three icon cards, a soft gradient, and a monospace caption, it should remember: the familiar layout is the one this skill exists to prevent. The page that wins is built from an item on the inventory list — the client's name and what it means, the street it stands in, the one constraint that makes it what it is — not from the category.

Build deliberately. Inspect honestly. Improve relentlessly. Report accurately.
