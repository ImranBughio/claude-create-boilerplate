# Password protection (Private mode)

This kit includes two small files that lock your site behind a username and password using
Apache's built-in HTTP Basic Auth — no code, no database. It's the simplest way to keep a static
site private.

- `.htaccess` — turns protection on for the folder it sits in.
- `.htpasswd` — holds the username and an encrypted password.

## What to do (or just ask Claude Code to do it for you)

**The fastest path:** open your project in Claude Code and say:

> "Set up the password protection for my site. Use the username and password I give you, and tell
> me exactly which folder path to put in the `.htaccess` file."

If you'd rather do it by hand:

1. **Set your username and password.** The starter login is `create` / `JLXv2b0OiDE6` — change it.
   Ask Claude Code, or use any online "htpasswd generator" and paste the result into `.htpasswd`
   (the format is `username:encrypted-password`).
2. **Set the file path.** Open `.htaccess` and change `AuthUserFile` to the **absolute path** of
   `.htpasswd` on your server. Your host's file manager shows this path; on cPanel it usually
   looks like `/home/your-username/public_html/your-folder/.htpasswd`.
3. **Upload both files** into the same folder as your built site (next to `index.html`).
4. **Test:** open the site in a private browser window — it should ask for the username and
   password.

## When you deploy — check the lock actually works

Password protection is enforced by the **server**, and servers differ, so after you upload you
must **verify it** (don't assume). Open the deployed page in a **private/incognito window**:

- ✅ **You get a login prompt** → working. Sign in and you're done.
- ⚠️ **"Internal Server Error" (500)** → the `AuthUserFile` path is wrong for this server. Set it
  to the real absolute path of `.htpasswd`. Ask Claude Code — on a PHP host it can drop a one-line
  `<?php echo __DIR__;` file to reveal the exact path, then remove it.
- ⚠️ **A 404 or the wrong page, no login box** → something at the site root (often a CMS like
  WordPress) is swallowing the login challenge. The shipped `.htaccess` already guards against this
  (`DirectoryIndex`, a rewrite reset, and `ErrorDocument 401`) — confirm the **whole** `.htaccess`
  uploaded and isn't being overridden, then clear any CDN/WAF cache (e.g. Sucuri, Cloudflare).
- ⚠️ **The page loads with no prompt at all** → `.htaccess` isn't being read: the host may have
  `AllowOverride` off for this folder, or it isn't an Apache server. Check with your host.

You're only actually private once the login prompt appears **and** the wrong password is refused.
The quickest way to run all of this: ask Claude Code to *"deploy-check my password protection —
confirm an unauthenticated visit returns a login prompt (401), not a 404 or 500, and fix it."*

## Notes

- The starter login is `create` / `JLXv2b0OiDE6`. **Change it before you go live.**
- These files protect the whole folder they're in, including everything below it.
- `.htaccess` already blocks anyone from downloading the password files themselves.
- If your site sits in a subfolder of a WordPress (or other) site, the included `.htaccess`
  already handles it — it forces a real login prompt instead of the app's 404 page.
- This is for the **Normal** (static site) kit. The Advanced (backend) kit sets up protection a
  different way — through the setup prompt in its `README.md`.
