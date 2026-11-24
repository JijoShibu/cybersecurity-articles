# You’ve Been Using APIs Your Entire Life — Now Let’s Learn How to Break Them

By **Jijo Shibu** · June 5, 2026 · [Originally published on Medium](https://medium.com/@jijoshibuwork/youve-been-using-apis-your-entire-life-now-let-s-learn-how-to-break-them-9ecd8c2dba67)

> **Selected excerpt.** How an API request works and the checks developers can use to protect each endpoint.

## A beginner’s guide to API testing with zero assumed knowledge

![Illustration for A beginner’s guide to API testing with zero assumed knowledge](https://miro.medium.com/v2/resize:fit:1400/1*xZwx07nAR3zh9cAgwnCyQQ.png)

Every morning you pick up your phone. You check the weather. You scroll Instagram. You order a coffee through an app. You tap “Pay.”

You just made about fifteen API calls before breakfast.

You didn’t know it. You didn’t need to. But somewhere between your finger and that cup of coffee, a tiny conversation happened between pieces of software — and that conversation is called an API.

This article is about understanding those conversations, and learning how security testers look for problems in them. No prior security knowledge required. If you know what a website is, you’re ready.

## First Things First: What Even Is an API?

**API** stands for Application Programming Interface. That phrase sounds like it was designed to confuse people. Let’s ignore it entirely and use a better explanation.

## Think of it as a restaurant.

Imagine you walk into a restaurant. You sit down. A waiter comes to your table.

- **You** are the app or the browser — the thing making a request.
- **The waiter** is the API — the middleman who carries messages back and forth.
- **The kitchen** is the server — where all the real data and logic lives.

You never walk into the kitchen yourself. You never talk directly to the chef. You tell the waiter what you want. The waiter goes to the kitchen. The kitchen prepares it. The waiter brings it back to you.

That’s it. That’s an API.

![Illustration for Think of it as a restaurant.](https://miro.medium.com/v2/resize:fit:1400/1*fQ3Mzq6ozRtxtTdOBEWkdg.png)

When you open Instagram and your feed loads, your phone whispers to Instagram’s servers: *“Give me the latest posts for this user.”* Instagram’s API takes that message to the kitchen (the server), grabs the posts, and brings them back to your screen.

When you tap Pay on a food delivery app, the API runs to the kitchen: *“Charge this card, place this order, notify the restaurant.”* Multiple kitchen trips, all in under a second.

## What Does an API Actually Look Like?

APIs speak a language called **HTTP** — the same language your browser uses to load websites. They communicate through something called **requests** and **responses**.

## A request is like a note you hand the waiter.

It has a few parts:

**1. The Method — what kind of request is this?**

Think of methods like different types of orders you can place at a restaurant:

| Method | Restaurant equivalent | What it does |
| --- | --- | --- |
| GET | “Bring me the menu” | Fetch data |
| POST | “I’d like to order this” | Send new data |
| PUT | “Actually, change my order” | Update existing data |
| DELETE | “Cancel my order” | Remove data |
| PATCH | “Just swap the side dish” | Partially update data |

**2. The URL — where the request goes**

This is the address of the specific thing you’re asking about. For example:

```text
GET https://api.coffeeapp.com/orders/1042
```

This is asking: *“Hey, bring me order number 1042.”*

**3. The Body — extra information you’re sending**

Sometimes you need to include data with your request, like filling out a form:

```json
{
  "item": "Flat White",
  "size": "Large",
  "milk": "Oat"
}
```

**A response is what the waiter brings back.**

It includes a **status code** — a short number that tells you how things went:

| Status code | Meaning and example |
| --- | --- |
| 200 OK | Everything worked Waiter brings your food |
| 201 Created | New thing was made “Your order has been placed!” |
| 401 Unauthorized | You’re not logged in “Members only — do you have a card?” |
| 403 Forbidden | Logged in, but not allowed “You can’t enter the VIP section” |
| 404 Not Found | Doesn’t exist “We don’t serve that dish” |
| 500 Server Error | Something broke on their end Kitchen is on fire |

![Illustration for A request is like a note you hand the waiter.](https://miro.medium.com/v2/resize:fit:1400/1*KH0ToS3cQZRNVN3Zvymerw.png)

## The Fix Is (Usually) Simple

Here’s the good news: most of these vulnerabilities have straightforward fixes.

| Vulnerability | Fix |
| --- | --- |
| BOLA / IDOR | After authenticating, always check: does this user own the resource they’re requesting? |
| Mass Assignment | Whitelist the fields you accept. Reject everything else. |
| Broken Auth | Enforce authentication on every endpoint, not just the ones you remembered. |
| Injection | Never concatenate user input into a database query. Use parameterised queries. |

None of these require exotic technology. They require discipline — and knowing what questions to ask.

[Back to the article index](../README.md)
