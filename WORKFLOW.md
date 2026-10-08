# Web Project Workflow

## Phase 1: Environment & Repository Setup ( from template )
- [ ] **Create new GitHub repository and local directory from template:** From "Web Projects" run `gh repo create project-name --template erevan-oblivious/green-page-stc --public --clone`. Use `erevan-oblivious/green-page-stc-ss` for sites that require server functionality such as for webforms.
- [ ] **Install environement dependencies:** `npm install` 
- [ ] **Verify Development Server:** Run `npm run dev` to confirm local environment builds cleanly. 
- [ ] **Create cloudflare worker for a temporary site:** Go to Cloudflare Dashboard and select Pages & Workers under Computing. Create Application; Connect to GitHub repositiry; Save and Deploy. 
- [ ] **(Optional - server-only) Add secrets to Cloudflare worker:** for example add `RESEND_API_KEY`, `CONTACT_EMAIL`, and `SITE_NAME` in the Cloudflare Dashboard under [Relevant Worker] > Settings > Variables & Secrets for webforms. Also create local `.dev.vars` file and add local API keys (`RESEND_API_KEY`, `CONTACT_EMAIL`, `SITE_NAME`). This allows use of this information when using `npm run dev`. Verify `.gitignore` contains `.dev.vars*` and `.env*` to prevent accidental secret commits.

### OR - Environment & Repository Setup ( from new ) 
- [ ] **Create Local Directory:** Run `mkdir project-name && cd project-name` 
- [ ] **Install Astro + dependencies and initialise GIT:** Run `npm create astro@latest .` (minimal template; y install dependencies; y initialize a new git repository). Also run `npx astro add cloudflare` if server side required. 
- [ ] **Publish to remote GitHub Repository:** run `gh repo create <new-repository-name> --public --source=. --remote=origin --push`
- [ ] **Verify Development Server:** Run `npm run dev` to confirm local environment builds cleanly. 
- [ ] **Create cloudflare worker for a temporary site:** Go to Cloudflare Dashboard and select Pages & Workers under Computing. Create Application; Connect to GitHub repositiry; Save and Deploy.
- [ ] **(Optional- server-side only) Create Resend API and add Secret API Key to Cloudflare worker (if needed for server-side):** for sending email from site e.g., for webforms.

## Phase 2: Assets Setup
- [ ] **SVG Favicon:** obtain high-contrast line art logo and convert to vector using SVGcode or Vectorizer.ai. Save output file as `public/favicon.svg`. and link to BaseLayout.
- [ ] **Static images:** Place direct static assets and background images referenced via CSS `url()` inside `/public` 
- [ ] **Optimised Images:** Place dynamic/optimized content images in `src/images/` for Astro `<Image/>` component processing.
- [ ] **Videos:** Encode videos into suitable formats (background, inline video, high-quality inline video) using ffmpeg. Save raw and converted files under separate video directory in 'raw' and 'ffmpeg' respectively. Save videos under 10mb (background, standard inline video) in `/public/videos`. Videos over 10mb (high-quality inline video) should be hosted via Cloudflare R2 buckets. Ensure the Cloudflare R2 bucket has Public Access enabled (or a custom domain attached) and a basic CORS policy configured if videos fail to seek or stream on mobile. 

## Phase 3: Build Site
- [ ] **Set style:** define root variables (colour, fonts etc.) and styles in global css - reflect across components and pages as required.
- [ ] **BaseLayout:** update BaseLayout with website name, description and other required meta data as well as header and footer content.
- [ ] **Build content:** make and edit pages html inclduing by adding components and setting component string variables.
- [ ] **Run dev:** use `npm run dev` to check website changes in real-time until website is built.

## Phase 4: Quality Assurance
- [ ] **AI Code Audit:** Perform an audit on `global.css` as well as `.astro` layout, pages, and components to check for errors; redundant or duplicative styles or html wrappers; accessibility flaws; SEO optimisation; any general improvements. 
- [ ] **Test Production Build:** Run `npm run build` locally to confirm zero build errors or broken imports and run `npm run preview` and navigate the built site. To account for caching, open site in incognito mode or check Disable Cache under Chrome DevTools > Network. 
- [ ] **Email Deliverability Check (optional - if using server-side):** Send a test form submission and verify it arrives in the primary inbox (not Spam). 
- [ ] **Lighthouse Audit:** Run Chrome DevTools Lighthouse audit in Incognito to catch accessibility contrast issues or unoptimized layout shifts.
- [ ] **Mobile & Cross-Browser Verification:** Use Chrome DevTools to test layouts at mobile, tablet and laptop break-points.
- [ ] **Repeat:** Repeat steps until all issues resolved. 

## Phase 6: Production Build & Deployment
- [ ] **Add custom domains to publish site at client url:** Make sure to add custom domain apex (without www.); then edit that domain to include www.; then add the apex domain again. This is the best way to create two bindings for both the www. and apex on cloudflare. (or add www. as a sub-domain?).
- [ ] **Email DNS Records:** If sending webform emails from the client’s domain (e.g., `info@client.com`) then add client's email DNS to Cloudflare DNS settings and update webform configuration to use client email servers to send - or (if the client does not have email servers) continue to use Resend by adding Resend’s DKIM and SPF TXT records to the domain's Cloudflare DNS settings.