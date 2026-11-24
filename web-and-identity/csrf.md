# What Is CSRF (Cross-Site Request Forgery)? Your Browser Is a Weapon — You Just Don’t Know It Yet

By **Jijo Shibu** · June 10, 2026 · [Originally published on System Weakness](https://systemweakness.com/what-is-csrf-cross-site-request-forgery-your-browser-is-a-weapon-you-just-dont-know-it-yet-02f5f4cc4aab)

> **Selected excerpt.** The glossary and the original discussion of server-side and browser defenses.

## First, a Quick Glossary

Three terms you’ll see throughout this article — simple, but essential.

🍪 **Cookie** — A tiny token your browser stores after you log in, like a concert wristband: proof you’re already in, so you don’t need to show ID on every page.

🔑 **Session** — The window of time you’re actively logged in. Your cookie keeps it alive until you log out or it expires.

🌐 **HTTP Request** — Every click, form submit, or page load sends a message to the server. Think of it as your browser passing a note: *“Hey, please do this.”*

![Cookie, Session & HTTP Request](https://cdn-images-1.medium.com/max/1024/1*yT-YcRcF0ddWamhvaK6I0A.png)

Cookie, Session & HTTP Request

> *💡* ***Did you know?*** *CSRF has appeared on the OWASP Top 10 — the definitive list of the web’s most critical security risks. It’s not a niche edge case. Keep reading to see why.*

## Common Defences Against CSRF

## 🛡️ Defence 1: The CSRF Token

This is the most widely used defence. When a server sends you a form, it secretly embeds a unique random code — a **CSRF token** — that only your session knows:

```html
<form action="/change-email" method="POST">
  <!-- Token is unique to your session — attacker can't know it -->
  <input type="hidden" name="csrf_token" value="a3F9kLmQ2pXrZw..." />
  <input type="email" name="new_email" />
  <button type="submit">Update Email</button>
</form>
```

The server stores a copy of this token tied to your account. When you submit the form, the server checks: *does this token match the one I issued?* Correct → processed ✅. Missing or wrong → rejected ❌.

Attackers can fire a request to the server, but they cannot *read* your CSRF token. The browser’s **Same-Origin Policy** — a built-in security rule — prevents JavaScript on evil.com from reading any content belonging to socialhub.com. Without the correct token, every forged request hits a dead end.

*Think of it as a secret handshake. Your browser and the server share it. The attacker is never in the room.*

## 🍪 Defence 2: The SameSite Cookie

This browser setting tells a cookie: *only travel with requests that started from our own site.*

```http
Set-Cookie: session=abc123; SameSite=Strict
```

| Setting | What it does |
| --- | --- |
| Strict | Cookie never sent on cross-site requests — maximum protection |
| Lax | Cookie only sent when the user clicks a link — stops hidden form attacks |
| None | Cookie always sent — leaves CSRF wide open |

With SameSite=Strict, even if the hidden form fires, the browser leaves the session cookie behind. The server sees an unauthenticated request — rejected.

> *💡 Modern browsers now default to* *Lax, which already blocks most CSRF attacks automatically.*

## 🔍 Defence 3: Origin & Referer Header Checks

Every request carries headers that reveal its source. A server can check these and reject anything from an untrusted domain:

```text
Origin: https://evil.com       →  BLOCKED ✗
Origin: https://socialhub.com  →  ALLOWED ✓
```

Not foolproof alone (headers can occasionally be missing), but a solid extra layer alongside tokens.

## 🔁 Defence 4: The Double-Submit Cookie

Here’s a clever trick used in certain web applications: when you load a page, the server plants a random value as a cookie in your browser. JavaScript on the page then copies that exact value and includes it as an extra field in every request. The server compares the two — if they match, the request is genuine.

Why can’t the attacker fake this? Because the Same-Origin Policy prevents their page from reading your cookies. They can’t copy a value they can’t see. The comparison fails. Request rejected.

## 🔐 Defence 5: Re-authenticate for Dangerous Actions

For the most sensitive actions — changing your password, updating your email address, deleting your account, or making a large payment — the smartest approach is simply asking the user to confirm they’re really there. Options include re-entering a password, entering a one-time code sent to their phone (2FA), or completing a CAPTCHA.

A CSRF attack can make your browser *send* a request. But it absolutely cannot make you *type in your password*. That one human step creates a wall that no automated forged request can climb.

*This is why your bank asks for your password again before big transfers. It’s not friction — it’s a firewall.*

![Illustration for 🔐 Defence 5: Re-authenticate for Dangerous Actions](https://cdn-images-1.medium.com/max/1024/1*OY4TBHu6nWmAK-G1aG-tOg.png)

## The Bigger Picture

CSRF exploits something *good* about the web. Browsers automatically manage your login state so you never have to re-enter credentials on every click. It’s a convenience the entire modern internet depends on. CSRF turns that convenience into a liability.

The attack needs no malware, no stolen password, no suspicious download — just your browser doing exactly what it was built to do. The attacker never logs in as you. They just borrow your browser for one quiet moment.

Every defence we covered answers the same core question: *did the real user actually intend this specific request?* CSRF tokens, SameSite cookies, Origin header checks, and re-authentication all add proof where the original web design assumed trust was enough.

Once you see the pattern, it applies everywhere in security: the moment a system assumes trust without verifying it, someone will find a way to exploit that gap. CSRF is one of the clearest, most teachable examples of exactly that.

> **Two habits that protect you as a user:** Log out of sensitive sites when you’re done, and stay sceptical of unsolicited links in emails and DMs — especially ones that feel urgent.

[Back to the article index](../README.md)
