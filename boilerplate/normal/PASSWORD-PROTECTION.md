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

1. **Set your username and password.** The starter login is `create` / `changeme` — change it.
   Ask Claude Code, or use any online "htpasswd generator" and paste the result into `.htpasswd`
   (the format is `username:encrypted-password`).
2. **Set the file path.** Open `.htaccess` and change `AuthUserFile` to the **absolute path** of
   `.htpasswd` on your server. Your host's file manager shows this path; on cPanel it usually
   looks like `/home/your-username/public_html/your-folder/.htpasswd`.
3. **Upload both files** into the same folder as your built site (next to `index.html`).
4. **Test:** open the site in a private browser window — it should ask for the username and
   password.

## Notes

- The starter login is `create` / `changeme`. **Change it before you go live.**
- These files protect the whole folder they're in, including everything below it.
- `.htaccess` already blocks anyone from downloading the password files themselves.
- This is for the **Normal** (static site) kit. The Advanced (backend) kit sets up protection a
  different way — through the setup prompt in its `README.md`.
