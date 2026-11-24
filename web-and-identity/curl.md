# 'curl' in Kali Linux: Your Secret Weapon for Talking to the Internet

By **Jijo Shibu** · June 22, 2026 · [Originally published on Medium](https://medium.com/@jijoshibuwork/curl-in-kali-linux-your-secret-weapon-for-talking-to-the-internet-c4306f54eea3)

*Kali Linux for Beginners*

*You’ve used the internet your whole life — but have you ever spoken to it directly, without a browser, with full control over every single word?*

![Illustration for You’ve used the internet your whole life — but have you ever spoken to it directly, without a browser, with full control over every single word?](https://cdn-images-1.medium.com/max/1024/1*4m91likvGNIEgPy1wSySEg.png)

There’s a moment every beginner in cybersecurity hits where someone types something into a terminal, presses Enter, and — boom — the server responds. No browser. No clicking. Just raw, direct communication between you and a machine on the other side of the world.

That tool is almost always curl.

By the end of this article, you’ll know exactly what curl is, why it matters, and how to use it confidently — even if you've never touched a Linux terminal before.

## Think of It Like a Delivery Person

Picture this: you want something from a café downstairs. You could walk there yourself — that’s you opening Chrome and visiting a website. But what if you could send a fast, reliable delivery person who brings back exactly what you asked for, reads out the response, and does it all in under a second?

That delivery person is curl.

**curl** (short for *Client URL*) is a command-line tool that sends requests to websites and servers — and fetches their responses — without you ever opening a browser. It does exactly what your browser does, but with you in full control of every single detail. In Kali Linux, it comes pre-installed and ready to go.

💡 *Think about it this way: your browser is a fancy car that drives you to websites automatically.* *curl is a bicycle you steer yourself — more effort to learn, but you know exactly where every road leads.*

![What is curl — The Delivery Person Analogy](https://cdn-images-1.medium.com/max/1024/1*48tBnTLHqlmV8tdAIsY0xg.png)

What is curl — The Delivery Person Analogy

> *📖* ***Key Terms***  
>  ***curl*** *— A command-line tool that sends and receives web requests on your behalf, without a browser.* ***Command-line*** *— A text-based interface where you type instructions directly to your computer.* ***Request*** *— A message you send to a server, like placing an order at a restaurant.* ***Response*** *— The server's reply — like the waiter bringing your food.*

## Getting Started in Kali

Fire up your Kali Linux machine and open a terminal. Press Ctrl + Alt + T or click the terminal icon in your taskbar.

First, confirm curl is ready:

```bash
curl --version
```

You should see something like curl 8.x.x. If you get an error instead, install it with:

```bash
sudo apt install curl
```

Now, the golden rule. **Every single** **curl command follows one simple pattern:**

```bash
curl [options] [URL]
```

Think of it like sending a letter. curl is the postal service. [options] are the special instructions on the envelope — "urgent," "return receipt," "attach document." And [URL] is the exact address you're writing to. Everything else is just learning what different options do.

> *📖* ***Key Terms***  
>  ***URL*** *— The address of a resource on the internet, like a home address but for data.* ***Flag / Option*** *— An instruction that changes how* *curl behaves, like telling a waiter "extra sauce, no onions."* ***Terminal*** *— The text-based window where you type commands directly to your system.*

## Your First curl Commands

Let’s start with the simplest thing possible. Type this and press Enter:

```bash
curl https://example.com
```

What just happened? curl knocked on the door of example.com, said "give me your homepage," and printed the raw HTML right in your terminal. It's like asking someone to read a webpage out loud to you — a bit messy, but completely real.

**Reading raw HTML in the terminal isn’t pretty though.** Let’s save it to a file instead:

```bash
curl -o mypage.html https://example.com
```

The -o flag means "output to this file." Think of it as telling your delivery person: "Don't read it to me — write it in a notebook called mypage.html."

Want curl to use the filename already in the URL? Use capital -O:

```bash
curl -O https://example.com/manual.pdf
```

Capital -O says "use the same filename that's in the URL." Handy for downloading files directly.

But here’s where it gets really interesting — you can watch the *entire conversation* happening behind the scenes.

```bash
curl -v https://example.com
```

The -v (verbose) flag reveals every step: what you sent, what the server replied, the handshake, the headers, all of it. Lines with &gt; are what *you* sent. Lines with &lt; are what the *server* said back. It's like reading a full transcript of a phone call in real time — and it's one of the most educational things you can do as a beginner.

![Verbose Mode — The Hidden Conversation](https://cdn-images-1.medium.com/max/1024/1*e34mEttc2-MITG-8yrvF3g.png)

Verbose Mode — The Hidden Conversation

> *📖* ***Key Terms***  
>  ***HTML*** *— The code language of webpages; the raw content the server sends back when you ask for a page.* ***Verbose mode (******-v)*** *— Reveals the complete request/response conversation step by step.* ***-o /*** ***-O*** *— Flags that save the server's response to a file instead of printing it to the screen.*

## Understanding HTTP Methods

**So what actually happens when your browser visits a website?** It’s not just “asking” — it’s sending a specific *type* of message. These message types are called HTTP methods, and curl uses them too.

![HTTP Methods](https://cdn-images-1.medium.com/max/1024/1*73IdfDPsKfCB8nCaMC7m2w.png)

HTTP Methods

Think of HTTP methods like the different things you can do at a post office:

- **GET** — “Send me this.” The default. Every time you visit a webpage, your browser sends a GET. Like walking up and asking a question.
- **POST** — “Here’s some data — process it.” Used for login forms, sign-ups, creating records. Like handing a filled-out form to a bank teller.
- **PUT** — “Replace the existing data with what I’m sending.” Like swapping an old document for a freshly updated version.
- **DELETE** — “Remove this.” Like asking someone to shred an old file permanently.

By default, curl uses GET. To use any other method, add the -X flag:

```bash
# GET — no flag needed, it's the default
curl https://api.example.com/users
```

```bash
# POST — submitting a login form
curl -X POST https://api.example.com/login \
  -d "username=alex&password=test123"
```

The -d flag carries your data — the filled-in fields on the form. Without -d, there's nothing to submit.

> *📖* ***Key Terms***  
>  ***HTTP*** *— The language computers use to talk to web servers. Like a universal rulebook for web conversations.* ***GET*** *— “Send me data.” The default request type, used for fetching any page.* ***POST*** *— “Here’s data, do something with it.” Used for logins, sign-ups, and form submissions.* ***-X*** *— Tells* *curl which HTTP method to use.* ***-d*** *— The data carried inside the request — the letter, not the envelope.*

## Headers and JSON — The Envelope and the Telegram

Most modern web apps don’t speak plain text — they speak **JSON**, a structured data format that looks like this:

```json
{"username": "alex", "password": "test123"}
```

Think of JSON as a structured telegram rather than a messy freeform letter. The fixed format makes it easy for machines to read and respond to precisely.

To send JSON with curl, you need the -H flag to add a **header**:

```bash
curl -X POST https://api.example.com/login \
  -H "Content-Type: application/json" \
  -d '{"username":"alex","password":"test123"}'
```

**HTTP headers are exactly like the envelope around a letter.** They carry metadata — information *about* the message, not the message itself. An envelope might say “written in French” or “this is urgent.” Headers say the same kinds of things to a server.

```bash
# Add an authorization token — like showing a VIP pass at the door
curl -H "Authorization: Bearer eyJhbGci..." https://api.example.com/data
```

```bash
# Fetch only the response headers — just the envelope, not the letter inside
curl -I https://example.com
```

💡 *That last one —* *curl -I — is something I always run early in a security assessment. Server headers often expose the exact software name and version running behind a site, which can point directly to known, documented vulnerabilities.*

![Headers and JSON](https://cdn-images-1.medium.com/max/1024/1*VTVev-T-AAWtPU8DQQcTWA.png)

Headers and JSON

> *📖* ***Key Terms***  
>  ***JSON*** *— A structured data format used by most modern APIs. Like a standardised telegram with fixed fields.* ***Header*** *— Metadata attached to a request or response — labels on the outside of the envelope.* ***-H*** *— Adds a custom header to your* *curl request.* ***-I*** *— Fetches response headers only, skipping the body entirely.* ***API*** *— A server endpoint designed to be accessed by code, not a browser.*

## Cookies, Redirects, and Authentication

Three more tools you’ll reach for constantly — all connected by the same idea: **proving who you are**.

**Redirects** happen when a server says “what you’re looking for is actually over there.” By default, curl stops at the first address and refuses to follow. Add -L to chase the full trail — like following every clue in a treasure hunt until you reach the final destination:

```bash
curl -L https://short.url/xyz
```

**Cookies** are tiny pieces of data websites use to remember you between requests — like a stamp on your hand at an event that lets you re-enter without queuing again:

```bash
# Send a cookie with your request
curl -b "session=abc123" https://example.com/dashboard
```

```bash
# Save cookies from a response into a file
curl -c cookies.txt https://example.com/login
```

```bash
# Use those saved cookies in your next request
curl -b cookies.txt https://example.com/dashboard
```

This three-step pattern mimics a full browser login session — and it’s completely scriptable. In security testing, it’s used to verify whether authenticated pages are actually protected, or just pretending to be.

**Basic authentication** is even simpler. Use -u to attach credentials directly to the request:

```bash
curl -u alex:mypassword123 https://example.com/private
```

Think of it as showing your ID card at a security checkpoint — curl attaches your credentials to the request automatically.

![Cookies and Session Identity](https://cdn-images-1.medium.com/max/1024/1*_AvBRq3Xy6UhFtqI17YESg.png)

Cookies and Session Identity

> *📖* ***Key Terms***  
>  ***Redirect*** *— A server instruction to go to a different URL;* *-L tells* *curl to follow it automatically.* ***Cookie*** *— A small piece of data the server uses to remember your identity between separate requests.* ***Session*** *— A temporary logged-in state, maintained through cookies or tokens.* ***-u*** *— Attaches a username and password directly to the request for basic authentication.*

## How It Works, Step by Step

Here’s a complete curl workflow for testing a login endpoint — the kind you'd run during an authorised security assessment:

![Step-by-Step Login Flow](https://cdn-images-1.medium.com/max/1024/1*xmyRp2tXs-srtBYzV-U7GQ.png)

Step-by-Step Login Flow

**Step 1:** Open your terminal and confirm you can reach the target with a simple GET request first.

**Step 2:** Switch to POST and attach your JSON credentials using -X POST and -d.

**Step 3:** Add -H "Content-Type: application/json" to tell the server you're speaking JSON — like writing the language on the outside of an envelope.

**Step 4:** Add -v so the full conversation is visible — every request line sent and every server response received, nothing hidden.

**Step 5:** Add -c session.txt to capture any session cookie the server returns, saving it for follow-up requests.

**Step 6:** Run the next request with -b session.txt to reuse that cookie — effectively staying "logged in" across commands.

```bash
curl -v -X POST https://target.example.com/api/login \
  -H "Content-Type: application/json" \
  -d '{"username":"admin","password":"admin123"}' \
  -c session.txt
```

*Think of it like a fully scripted visit to a bank — you announce yourself, present your credentials, hand over your form, and pocket your visitor pass on the way in. The next time you walk up, you just show the pass.*

## Why curl Matters for Security

Most beginners think web security is about clicking around in a browser. Security professionals know the real action happens at the HTTP layer — and curl gives you a direct window into that layer.

Here’s what curl lets you do that a browser simply can't:

- Inspect raw server headers that reveal software versions and misconfigurations
- Replay and modify requests with surgical precision, with nothing intercepted or hidden
- Test whether authentication is actually enforced on every single endpoint
- Script sequences of requests — login, navigate, submit — in a single bash script
- Operate on headless servers and remote machines with no GUI whatsoever

**curl is where the shift happens** — the moment you stop *clicking* the web and start *talking* to it. I find that shift more empowering than any other tool a beginner can learn.

> *⚠️ One non-negotiable rule: always use* *curl only on systems you own or have* ***explicit written permission*** *to test. Ethical hacking begins and ends with authorisation.*

## Common Misconceptions

- **“curl is just a developer tool."** — Security researchers, CTF players, penetration testers, sysadmins, and network engineers all use curl daily. It's one of the most universal tools on the internet, full stop.
- **“The** **-k flag is fine to use anywhere."** — -k disables SSL certificate verification. It exists for controlled lab environments and CTF challenges only. Using it against real targets means you can't verify the server is who it claims to be — which defeats a core security principle.
- **“I need to memorise all the flags.”** — You don’t. Start with -v, -X, -H, and -d. Look everything else up as you need it. curl --help is always one command away.

## Key Takeaways

- curl is your direct line to the internet — it does everything your browser does, but with complete visibility and control.
- Every web interaction is a **request + response**. curl makes that conversation visible, which is the first step to understanding how the web truly works.
- **HTTP methods define intent**: GET = ask, POST = submit, PUT = update, DELETE = remove.
- **Headers are the metadata layer** — they carry context that most beginners never see, but security testers always read.
- **Verbose mode (****-v) is your best learning tool** — use it constantly when starting out; it turns curl into a real-time teacher.
- **Cookies = session identity** — saving and replaying cookies lets curl impersonate a fully logged-in browser session.
- Once curl clicks, tools like Burp Suite, ffuf, and sqlmap will feel familiar — they all speak the same HTTP language underneath.

## ⚠️ Going Deeper

*If the basics clicked and you want to go further, here’s what to explore next:*

- **HTTP status codes in depth** — Understanding exactly what 200, 301, 401, 403, 404, and 500 mean (and when servers deliberately lie about them) is a core web security skill.
- **Burp Suite** — A GUI proxy that intercepts and modifies HTTP requests in real time; everything you learned about headers, methods, and cookies applies directly here.
- **ffuf** — A web fuzzer built on the same HTTP foundations as curl; used to brute-force hidden endpoints, files, and parameters.
- **OWASP API Security Top 10** — A structured guide to the most common API vulnerabilities — broken object-level authorisation, broken auth, mass assignment — all testable with nothing more than curl.
- **curl vs wget** — Two different command-line tools for fetching content from the web; understanding the difference sharpens your instinct for which to reach for and when.

## 🧠 Quick Knowledge Check

*Try answering these before scrolling to the answer key — you’ll be surprised how much stuck!*

1. What does curl stand for, and what does it do in one sentence?
2. What command would you run to confirm curl is installed and check its version?
3. You want to download a file and save it automatically using the filename from the URL. Which flag do you use?
4. What HTTP method does curl use by default if you don't specify one with -X?
5. What is the difference between a request **header** and a request **body**? Give the analogy used in this article.
6. You want to send JSON to a login API. Which two flags are essential, and what does each one do?
7. What does curl -I https://example.com return — and why is it useful for security research?
8. What does the -c flag do, and how does it work together with the -b flag in a follow-up request?
9. Why does the article warn against using -k on real targets, not just in labs?
10. 🔍 **Research question:** curl was first released in 1998. Who created it, what was the tool it evolved from called, and approximately how many devices does curl ship on today?

## 📖 Answer Key

*No peeking before you’ve tried! Answers are here to confirm, not replace, your thinking.*

**Q1:** curl stands for *Client URL*. It's a command-line tool that sends requests to web servers and fetches their responses — without a browser.

**Q2:** curl --version. If you see a version number, you're ready. If not, run sudo apt install curl.

**Q3:** Capital -O — it tells curl to save the file using the filename already present in the URL. Lowercase -o requires you to specify your own filename manually.

**Q4:** GET — the default method used every time you fetch a page or resource without specifying -X.

**Q5:** A **header** is like the writing on the outside of an envelope — metadata about the message (its language, type, urgency). The **body** (-d) is the actual content inside — the letter itself. For a login form: the header says "this is JSON," the body contains the username and password.

**Q6:** -H "Content-Type: application/json" tells the server what data format you're sending (the label on the envelope); -d carries the actual JSON payload (the content of the letter).

**Q7:** curl -I fetches only the response headers — the envelope, not the letter. This is valuable in security research because server headers often reveal the exact web server software and version (e.g. Apache/2.4.51, PHP/8.1.2), which can point to known, documented vulnerabilities without touching the page body at all.

**Q8:** -c cookies.txt saves any session cookie returned by the server into a file. -b cookies.txt reads that file and sends the cookie in the next request — keeping you "logged in" across separate curl commands, exactly as a browser would.

**Q9:** -k disables SSL certificate verification, which means curl can no longer confirm the server is who it claims to be. In a real-world scenario, a man-in-the-middle attacker could intercept your traffic and you'd have no warning. It's safe only in controlled lab environments where the certificate situation is already known and trusted.

**Q10:** curl was created by **Daniel Stenberg**. It evolved from an earlier tool called **httpget** (later renamed urlget). Today, curl is estimated to ship on over **20 billion devices** worldwide — including cars, smart TVs, printers, routers, and gaming consoles — making it arguably the most widely distributed software tool in history.

The terminal is no longer intimidating. It’s a conversation — and now you know exactly how to speak.

*If you found this useful, give it a clap 👏 — it helps more people discover it. And if you have questions, drop them in the comments. I read every one.*

*Want to talk cybersecurity, share resources, or just connect? Find me on* [*LinkedIn*](https://www.linkedin.com/in/jijoshibu) *— I’m always happy to connect with curious minds.*

[Back to the article index](../README.md)
