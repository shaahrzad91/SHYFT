# Live Supabase acceptance checklist

Run with fictional stories and two approved test accounts in different browser sessions. This checklist has NOT yet been completed against your Supabase project.

1. Add both test emails to alpha_invites. Signup with an uninvited email must fail. Invited signup must send confirmation. Unconfirmed login must fail when confirmation is enabled.
2. Confirm email, log in, reload, and log out. Logged-out visitors must not see member data. Login again: the same profile and stories should return. Check password reset email, mismatched passwords, successful reset and old-password rejection.
3. Start a Shyft, type, wait for “Draft saved to your account.” Refresh and check Drafts. Disconnect networking: saving must show an error, not a success. Reconnect and retry. Cancel or use Home, then return to the draft. Images are not part of autosaved drafts.
4. Preview and publish a Private story. Check it from the other account: it must not appear in Discover and its direct link must not expose it. Try querying it via the Supabase API with the other account: zero rows. Try changing its owner or editing it with the other account: denied.
5. Publish a Public story. The other account can read, follow, and mark it helpful. Add an update as owner; refresh the follower's feed and verify unread status. Open it and verify the unread marker clears.
6. Publish an Unlisted story. It must not appear in Discover or be obtainable from the other account by querying all stories. Copy its sharing link as owner; open as the other account and sign in. It should become readable. Wrong token must fail. Change it to Private: the other account must lose access after refresh, including image downloads.
7. Upload a JPEG, PNG or WebP smaller than 1 MB. Check the image after logout/login. Reject larger images and other formats. Confirm private-image downloads fail as the other account and as an anonymous visitor.
8. Edit a story status. A timeline entry must appear. Publish an update with a new status: there should be one update entry, not two. Repeat clicks while publishing must not create duplicates.
9. Report a story. Check the report is stored in Supabase and appears in the submitter's Settings. Check it is not readable by other members. Remember the owner must review reports manually.
10. Block the other member: stories must disappear and direct links must stop displaying them. Unblock in Settings: shared/Public access should return.
11. Save name, bio and email preferences; reload and confirm persistence. No notification email is expected until n8n is connected.
12. Test every navigation item, browser Back/Forward, repeated Home clicks, logo → landing, landing → Start, and mobile menu. Check at narrow phone width. Confirm signed-out forms lead to login and do not save member content locally.
13. For remote testers, repeat confirmation/reset in the actual hosted HTTPS environment. Localhost links from your computer are not remote share links.

Record the project URL (not secrets), date, browser, result and any errors. Do not mark the real-account release ready until these tests pass.
