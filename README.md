# SHYFT — GitHub to Vercel deployment

This package contains the version 2 design and the Supabase account integration. Your existing Supabase project is configured. Moving the website to Vercel does not move or erase members or stories: the website continues using the same database.

## What you are uploading

- public/: the complete website, scripts, styling, artwork and Supabase browser library.
- vercel.json: deployment settings. Vercel serves only public/.
- database/: a reference copy of the initial database setup. YOU ALREADY RAN THIS. Do not run it again on your existing project.
- TESTING_CHECKLIST.md: full account/storage testing checklist.

The hosted edition removes the browser-based Supabase setup form. Members see the normal signup/login pages. Connection settings are read from public/config.js. That file contains only your Project URL and publishable key, which is designed for browser use. Never add a secret/service_role key, database password, or member-data export to this repository.

## 1. Extract the ZIP

Unzip SHYFT_Vercel_GitHub.zip on your computer. Open the extracted folder. Inside it you should see public, database, vercel.json and this README.

Upload the CONTENTS of this folder to GitHub, not the ZIP itself and not an extra enclosing folder.

## 2. Create your GitHub repository

1. Sign in at https://github.com/new .
2. Name your repository shyft-website (or another name you prefer).
3. Choose Private to keep your source repository private. Vercel can connect to a private repository; the website can still have a public address.
4. Create the repository. You do not need an auto-generated README because one is included here.
5. On the empty repository page, choose the link to upload an existing file. In an existing repository use Add file → Upload files.
6. Drag the extracted folder's contents, including the public and database folders, into the upload area. Preserve their folder structure. Include vercel.json and README.md at the repository's top level.
7. Select Commit changes.
8. Verify that public/index.html exists and vercel.json is at the top level. If everything is inside another SHYFT_Vercel_GitHub folder, move its contents to the repository root or select that enclosing folder as Vercel's Root Directory in the next step.

No member information or Supabase password is included in this package. The .gitignore file is optional for browser uploads but useful for future local development.

## 3. Import into Vercel

1. Sign in at https://vercel.com/new using your chosen account.
2. Connect GitHub if asked. Give Vercel access to the shyft-website repository.
3. Find that repository under Import Git Repository and select Import.
4. Use these settings:

| Setting | Value |
| --- | --- |
| Framework Preset | Other |
| Root Directory | Repository root (leave the default) |
| Build Command | Empty / no build command |
| Output Directory | public |
| Install Command | Empty / no install command |
| Environment Variables | None required for this package |

The included vercel.json sets the empty commands and public output directory. This is a static HTML/JavaScript website, not a Next.js project. There is no npm build and no package.json requirement. Adding NEXT_PUBLIC variables in Vercel alone will not change this package's configuration; edit public/config.js if you later change Supabase projects.

5. Click Deploy and wait until the deployment is Ready.
6. Open the project's stable Production domain, for example https://shyft-website.vercel.app/ . YOUR actual domain may differ. Copy the one Vercel assigned to your project, not the example and not a temporary preview/deployment URL.

Vercel plan eligibility and any commercial-use requirements depend on your account; check Vercel's plan terms before choosing a plan. This delivery does not purchase or deploy anything for you.

## 4. Update Supabase authentication URLs

1. Open https://supabase.com/dashboard/project/nvluzftdvwpmajyhpggv/auth/url-configuration .
2. Replace Site URL with your actual Vercel production address, including the final slash. Example only: https://shyft-website.vercel.app/ .
3. Under Redirect URLs, add the EXACT same address and save.
4. Keep http://127.0.0.1:4173/accounts/ as an additional redirect only if you still want local testing. The primary Site URL should now be the hosted site.
5. Do not add /accounts/ or #/login to the hosted redirect address. This package is hosted at the domain root. Its login page is /#/login, but confirmation emails return to the root URL.
6. If you later add a custom domain, repeat these settings using that domain.

Keep email confirmation enabled. Existing members do not have to create new accounts. They will need to log in again on the new domain because browser login sessions are separate by website address. Old emails may still point to localhost: request a fresh confirmation/reset email from the hosted website after changing these settings.

## 5. Make email work for other testers

Your alpha_invites table controls who may sign up. It does NOT configure email delivery.

Supabase's default email sender is restricted to addresses belonging to the Supabase project team and has small sending limits. Before inviting external members, configure custom SMTP in Supabase Authentication using an email provider you control. Follow https://supabase.com/docs/guides/auth/auth-smtp . Keep provider credentials in Supabase's SMTP settings, never in GitHub or public/config.js. Do not add customers as Supabase administrators to solve email delivery.

Add each approved tester's email using the SQL Editor:

```sql
insert into public.alpha_invites(email)
values (lower('tester@example.com'))
on conflict do nothing;
```

Replace the example before running. This approves signup; it does not create their account or send an invitation email. Send them your hosted website address yourself, then they can sign up and confirm their email.

## 6. Verify before sharing

1. Open the production website in a browser. Check the landing page, artwork, menu and FAQ.
2. Open /#/login and log in with your existing SHYFT account. Existing saved information should appear.
3. Create a Private test story, reload, log out and log in. Confirm it remains saved.
4. Add an update with a small PNG/JPEG/WebP and check it after reloading.
5. Test a password-reset email. Its link must return to the hosted domain, not localhost.
6. With a second invited test account, check that Private stories are inaccessible, Public stories can be read, and Unlisted stories require the sharing link. Complete TESTING_CHECKLIST.md.
7. Open the production URL in a private/incognito window. If Vercel asks visitors for a Vercel login, review your project's Deployment Protection and intended production audience. A private GitHub repository and Vercel deployment protection are separate settings.
8. Share the stable production URL only after the checks pass. Do not share localhost or the Supabase dashboard link with members.

## 7. Future updates

Upload changed website files to the same GitHub repository and commit them to its production branch (normally main). Vercel automatically deploys new commits. Story/member changes in Supabase do not require website redeployment. A Vercel rollback restores website code, not database data.

## Troubleshooting

- Build asks for npm/package.json: choose Framework Other, empty Build/Install commands and Output Directory public.
- 404 on the homepage: check public/index.html and repository Root Directory.
- Missing artwork: ensure assets/ and vendor/ were uploaded inside public/.
- Email not confirmed: use the latest confirmation email or resend from the hosted website.
- Email goes to localhost: correct Site URL and Redirect URLs, then request a fresh email.
- Signup database error: confirm the exact email is approved in alpha_invites and inspect Supabase Auth logs.
- Email not authorized / sending limit: configure custom SMTP; invitation-table approval is separate.
- Tables not found: confirm public/config.js points to your existing configured project. Do not reinstall the schema blindly.
- You see an owner setup form: you deployed an older package; use the public/ folder from this ZIP.

## Current scope and validation

This is a private-alpha implementation. Profiles, stories, text drafts, updates, uploads, follows, helpful reactions, reports, blocks and email preferences are included. n8n/Google Sheets sync and story-notification email sending are not connected. Reports require manual review in Supabase. There is no admin dashboard or self-service account deletion. Data loading uses the default 1,000-row limit per table; add pagination before expanding beyond that. Provide final terms, privacy notice, support contact and a data-deletion process before a wider release.

The source JavaScript syntax and deployment-package structure were checked locally. Earlier embedded PostgreSQL access tests passed; email confirmation was reported working in your local setup. This package has not yet been deployed or tested on Vercel. Run the live checks above after deployment.

Official documentation:
- https://vercel.com/docs/git/vercel-for-github
- https://vercel.com/docs/builds/configure-a-build
- https://supabase.com/docs/guides/auth/redirect-urls
- https://supabase.com/docs/guides/auth/auth-smtp
