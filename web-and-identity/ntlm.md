# NTLM Explained in Plain English

By **Jijo Shibu** · June 30, 2026 · [Originally published on System Weakness](https://systemweakness.com/ntlm-explained-in-plain-english-17ef3ee02f66)

*Windows Security Explained*

*The authentication protocol that’s been quietly running inside Windows networks for decades — and why attackers still love to abuse it.*

![Illustration for The authentication protocol that’s been quietly running inside Windows networks for decades — and why attackers still love to abuse it.](https://cdn-images-1.medium.com/max/1024/1*OJG6Nb0zojUmLvV1fAG5Xw.png)

Imagine you’re trying to get into a building you’ve never visited before. There’s a security guard at the front desk. You tell him your name, but he doesn’t just take your word for it — he picks up the phone, calls your boss, and asks *“Does this person actually work here?”* Your boss doesn’t tell the guard your password. Instead, they give the guard a secret puzzle only someone who knows your password could solve. You solve the puzzle. The guard lets you in. Nobody ever said your password out loud.

That, in a nutshell, is how **NTLM** — Windows New Technology LAN Manager — works. It’s been quietly running inside Windows networks for decades, handling login requests behind the scenes. Most users have never heard of it. Most attackers, however, know it extremely well.

## What Is NTLM, Exactly?

NTLM is an **authentication protocol** developed by Microsoft. Authentication is just a fancy word for the process of *proving who you are* to a computer system. When you log into a Windows machine or access a shared folder on a company network, something has to verify that you are who you claim to be. NTLM is one of the systems that handles that job.

The name itself sounds intimidating, but let’s break it down. “LAN Manager” was the original authentication system from the early days of Windows networking — old, weak, and long retired. “NT” refers to Windows NT, the modern Windows line that replaced it. “New Technology LAN Manager” is simply the upgraded authentication system that came with Windows NT. The “new” in the name is almost ironic at this point, because NTLM has been around since the early 1990s.

Think of NTLM as the secret handshake protocol for Windows computers. When two machines want to talk to each other and one needs to confirm the other’s identity, NTLM is often the mechanism that facilitates the handshake.

> *📖* ***Key Terms*** ***Authentication*** *— The process of verifying the identity of a user or system before granting access.* ***Protocol*** *— A set of agreed-upon rules that computers follow to communicate with each other.* ***LAN*** *— Local Area Network; a group of computers connected together in a shared space, like an office.*

## The Secret Handshake: How NTLM Actually Works

Here’s where NTLM gets clever. Most people assume logging in means sending a password somewhere. With NTLM, the password *never travels over the network*. Instead, NTLM uses a system called **challenge-response authentication** — and it’s worth understanding exactly what that means.

Imagine a spy movie where two agents need to verify each other’s identity without revealing their code words out loud. One agent says *“complete this phrase.”* The other finishes it correctly, proving they know the secret without ever speaking it. That’s the spirit of challenge-response.

Here’s how the actual NTLM exchange unfolds:

![Challenge-Response Overview](https://cdn-images-1.medium.com/max/1024/1*LW48MJKF__5fHxIYjySW-w.png)

Challenge-Response Overview

## How It Works, Step by Step

**Step 1:** The user tries to access a resource — say, a shared folder on a server. The client machine sends a message saying *“Hello, I’m trying to log in as this user.”* No password yet.

**Step 2:** The server sends back a **challenge** — a random piece of data, essentially a one-time puzzle. Think of it as the guard handing you a sealed envelope with a unique question inside.

**Step 3:** The client takes the user’s password, runs it through a hashing function to create what’s called an **NT hash**, and then uses that hash to encrypt the challenge. The result is called a **response**. Crucially, the original password never leaves the machine.

**Step 4:** The client sends the encrypted response back to the server.

**Step 5:** The server (or a domain controller, in a network environment) checks whether the response matches what it would expect if the user knew the correct password. If it matches, access is granted.

Think of it like this: you’re asked to solve a puzzle that only someone who read the same secret textbook could answer. You solve it. You never had to show the textbook.

> *📖* ***Key Terms*** ***Challenge-Response*** *— An authentication method where the verifier sends a random challenge and the claimant proves identity by responding correctly without revealing the secret itself.* ***Hashing*** *— A one-way mathematical function that converts data into a fixed-length string; you can’t reverse it back to the original.* ***NT Hash*** *— The specific hash of a Windows password that NTLM uses during authentication.* ***Domain Controller*** *— A server in a Windows network that manages user accounts and handles authentication requests.*

## NTLMv1 vs NTLMv2 — Not All Versions Are Equal

If you start reading about NTLM in security contexts, you’ll quickly run into two versions: **NTLMv1** and **NTLMv2**. The difference matters enormously from a security standpoint.

Think of NTLMv1 as the original door lock on an old house — functional when it was installed, but not designed for modern threats. NTLMv2 is the deadbolt that was added later: stronger, more complex, much harder to pick.

**NTLMv1** was the original. It used a shorter, weaker response calculation, which made it vulnerable to attacks even on modest hardware. Security researchers showed decades ago that NTLMv1 responses could be cracked relatively quickly.

**NTLMv2**, introduced with Windows NT 4.0 SP4, added extra data into the response calculation — including a timestamp and a random client-generated value — making the response significantly harder to reproduce or crack. It’s the version still encountered in modern Windows environments.

***“So if NTLMv2 is stronger, why do people still talk about NTLM as a security problem?”*** That’s exactly the right question — and the answer is both fascinating and a little uncomfortable.

> *📖* ***Key Terms*** ***NTLMv1*** *— The original, weaker version of the NTLM protocol, now considered insecure.* ***NTLMv2*** *— The improved version of NTLM that uses stronger cryptographic calculations and is still found in modern Windows systems.*

## Why Attackers Still Love NTLM

Here’s where it gets interesting.

Even NTLMv2 has a fundamental structural weakness — one that isn’t about the math being breakable, but about *how the protocol is designed to work*. And attackers have spent years building attack techniques around that design.

The most well-known attacks against NTLM are:

**Pass-the-Hash.** Remember how the NT hash is used in the authentication response? An attacker who manages to steal that hash from a compromised machine can use it *directly* to authenticate as that user — without ever knowing the actual password. It’s like stealing a key impression from wax and using it to open a lock. The original key never left the victim’s keychain.

![Pass-the-Hash Attack](https://cdn-images-1.medium.com/max/1024/1*xFGZWCBHCSbqGSS7_Cd-Iw.png)

Pass-the-Hash Attack

**NTLM Relay.** This is arguably the most dangerous modern NTLM attack. Because NTLM authentication can be triggered over the network, an attacker in the middle can *intercept* an authentication attempt from a victim and *forward* it to another server. The server thinks the attacker is the victim. The attacker gains access. The victim has no idea any of this happened.

💡 Think about it this way: imagine you call your bank to verify your identity, but without realising it, a stranger picks up the phone in your house, listens to you complete the verification challenge, and then immediately calls the bank themselves using your answers. They never needed your password — they just needed to be there at the right moment.

![NTLM Relay Attack](https://cdn-images-1.medium.com/max/1024/1*Udw3nYBwP8Cbf3_W8jKNhw.png)

NTLM Relay Attack

**Offline Hash Cracking.** When NTLM authentication hashes are captured over the network, they can be taken offline and subjected to brute-force or dictionary attacks. Tools like Hashcat can test billions of password guesses per second against captured hashes.

These attack categories are precisely why NTLM has a complicated reputation: the design itself creates opportunities that clever adversaries have learned to exploit.

> *📖* ***Key Terms*** ***Pass-the-Hash*** *— An attack technique where a stolen password hash is used directly to authenticate, bypassing the need to know the actual password.* ***NTLM Relay*** *— An attack where an adversary intercepts an NTLM authentication attempt and relays it to a different target to gain unauthorized access.* ***Brute-Force*** *— Trying every possible password or hash combination until the correct one is found.*

## NTLM vs Kerberos — Why Both Still Exist

If NTLM has these problems, why hasn’t it been replaced? Well — mostly, it *has* been. In modern Active Directory environments, **Kerberos** is the preferred authentication protocol. Kerberos is more secure, more efficient for large networks, and doesn’t have the relay vulnerability that plagues NTLM.

Think of Kerberos like a trusted ticketing system at an airport. When you check in, you get a boarding pass (a ticket). Every time you need to access a lounge, a gate, or a flight, you show that ticket — nobody keeps calling back to headquarters to verify you. The verification happens once, and the ticket carries that proof forward.

NTLM, by contrast, requires a fresh challenge-response exchange every time you access something. There’s no ticket, no cached proof — just repeated handshakes.

So if Kerberos is better, why does NTLM still exist in 2025? Because real-world networks are messy. NTLM is used as a **fallback** whenever Kerberos isn’t available — for example, when a machine isn’t joined to the domain, when a hostname can’t be resolved, when accessing a resource by IP address instead of hostname, or when connecting to very old systems. As long as those scenarios exist, NTLM stays in the picture.

![NTLM vs Kerberos](https://cdn-images-1.medium.com/max/1024/1*HIib8Tl872AlhthB29ELyw.png)

NTLM vs Kerberos

> *📖* ***Key Terms***   
> ***Kerberos*** *— A ticket-based authentication protocol that is more secure than NTLM and preferred in modern Windows domain environments.* ***Active Directory*** *— Microsoft’s directory service for managing users, computers, and policies in a Windows network.* ***Fallback*** *— A secondary mechanism that activates when the primary one can’t be used.*

## Real-World Impact: When NTLM Goes Wrong

NTLM relay attacks have been behind some significant real-world compromises. The **MS08–068** vulnerability, for instance, allowed attackers to relay NTLM authentication back to the originating machine itself — essentially letting a machine authenticate against itself as an administrator. Microsoft patched it, but the underlying relay concept has continued to resurface in new forms.

More recently, tools like **Responder** (a network poisoner) and **ntlmrelayx** (part of the Impacket toolkit) have made NTLM relay attacks accessible and highly automated. Penetration testers routinely use these in red team engagements to demonstrate how quickly an internal network can be compromised if NTLM is not properly restricted.

The pattern is always the same: trigger an NTLM authentication from a victim machine, capture or relay it, gain access to something you shouldn’t.

## Why It Matters — Even If You’re Not an Attacker

You might be thinking: *“I’m not a security professional. Why does any of this affect me?”*

If you use a Windows computer at work — especially in a company with a domain network — NTLM is almost certainly running somewhere in your environment. It’s one of the first things a red team looks for when they’re given access to an internal network. It’s also one of the first things a defender should look to restrict.

Understanding NTLM helps you appreciate why security teams push for things like SMB signing (which prevents relay attacks), why IT administrators disable NTLMv1, and why you should always use hostnames rather than IP addresses when connecting to internal resources.

And if you’re heading into cybersecurity yourself — as a blue teamer, red teamer, or sysadmin — NTLM is foundational knowledge. You’ll encounter it constantly.

## Common Misconceptions

- **“NTLM sends your password over the network.”** It doesn’t. The challenge-response design is specifically built to avoid transmitting the password. What can be captured is the hash and the response — which is still dangerous, but for different reasons.
- **“NTLM is completely broken and no one uses it.”** NTLM is still widely deployed as a fallback in real-world Windows environments. It hasn’t disappeared; it just coexists awkwardly with Kerberos.
- **“Switching to NTLMv2 solves all the problems.”** NTLMv2 is stronger than v1, but the relay attack vector still exists in NTLMv2. Disabling NTLM entirely (where possible) and enforcing SMB signing are the more complete mitigations.

## Key Takeaways

- NTLM is a Windows authentication protocol that uses a challenge-response mechanism to verify identity without transmitting the actual password.
- The password is converted into an NT hash, which is used to encrypt a server-issued challenge — the resulting response proves identity.
- NTLMv2 is stronger than NTLMv1 but both remain vulnerable to relay attacks and, to varying degrees, offline cracking.
- In modern networks, Kerberos is preferred, but NTLM persists as a fallback in many real-world scenarios.
- The most critical NTLM attack techniques — Pass-the-Hash and NTLM Relay — exploit the protocol’s design rather than breaking its cryptography.

## ⚠️ Going Deeper

If the basics clicked and you want to go further, here’s what to explore next:

- **SMB Signing** — Understand how enabling SMB signing on Windows networks blocks NTLM relay attacks at the transport layer.
- **Responder & Impacket** — Explore how these open-source tools are used in penetration tests to capture and relay NTLM hashes (in a lab environment).
- **MS-NLMP Specification** — Microsoft’s official protocol documentation for NTLM; dry reading, but essential if you want to understand the byte-level detail.
- **CVE-2019–1040 (Drop the MIC)** — A vulnerability that allowed bypassing NTLM message integrity protections, illustrating how the protocol’s edge cases can be weaponised.
- **Kerberoasting** — The analogous attack category for Kerberos, to understand why “switching to Kerberos” is also not a silver bullet.

## 🧠 Quick Knowledge Check

*Try answering these before scrolling to the answer key — you’ll be surprised how much stuck!*

1. What does NTLM stand for?
2. Does NTLM send your password over the network? Why or why not?
3. What is the NT hash, and what role does it play in NTLM authentication?
4. What is a “challenge” in the context of challenge-response authentication?
5. What is the key difference between NTLMv1 and NTLMv2?
6. Explain the Pass-the-Hash attack in one sentence.
7. How does an NTLM relay attack work?
8. Why does NTLM still exist in modern Windows networks if Kerberos is more secure?
9. What is a domain controller and why is it relevant to NTLM?
10. 🔍 **Research question:** What is “SMB signing” and how does enabling it protect against NTLM relay attacks on a Windows network?

## 📖 Answer Key

*No peeking before you’ve tried! Answers are here to confirm, not replace, your thinking.*

**Q1:** New Technology LAN Manager — Microsoft’s challenge-response authentication protocol used in Windows networks.

**Q2:** No. NTLM uses challenge-response so the password itself never travels over the network. Instead, a hash derived from the password is used to generate an encrypted response.

**Q3:** The NT hash is the result of running the user’s password through a hashing function. During authentication, this hash is used to encrypt the server’s challenge, producing the response that proves identity.

**Q4:** A challenge is a random piece of data sent by the server during the login attempt. The client must use its knowledge of the password (via the NT hash) to correctly transform this challenge into a response.

**Q5:** NTLMv2 incorporates additional data — including a timestamp and a client-generated random value — into the response calculation, making it significantly harder to crack offline than NTLMv1’s simpler computation.

**Q6:** Pass-the-Hash is an attack where a stolen NT hash is used directly to authenticate as a user, bypassing the need to crack or know the actual password.

**Q7:** In an NTLM relay attack, the attacker intercepts an authentication attempt from a victim and forwards it to a different server, tricking that server into granting the attacker access as if they were the victim.

**Q8:** NTLM persists because modern networks often have scenarios where Kerberos cannot function — such as IP-address-based connections, non-domain machines, or legacy systems — so NTLM acts as a fallback.

**Q9:** A domain controller is a server that manages user accounts and authentication in a Windows domain environment. In NTLM, it may be involved in verifying credentials on behalf of other servers.

**Q10:** SMB signing adds a cryptographic signature to Server Message Block (SMB) network packets, ensuring they haven’t been tampered with. When SMB signing is required, an NTLM relay attacker can’t simply forward captured authentication responses to an SMB server because the server can detect that the packets weren’t legitimately signed by the original sender. Look up Microsoft’s documentation on “Require SMB Security Signatures” for implementation details.

Security is never just about having a strong password. Sometimes it’s about understanding the handshake happening underneath — and knowing who might be listening.

*If you found this useful,* ***give it a clap 👏*** *— it helps more people discover it. And if you have questions, drop them in the* ***comments****. I read every one.*

*Want to talk cybersecurity, share resources, or just connect?* ***Find me on*** [***LinkedIn***](https://www.linkedin.com/in/jijoshibu) *— I’m always happy to connect with curious minds.*

[Back to the article index](../README.md)
