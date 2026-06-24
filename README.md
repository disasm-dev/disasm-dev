<div align="center">

<img src="./assets/banner.png" alt="DISASM: Bot protection, reverse-engineered." width="100%" />

<br/>

**Request-based anti-bot bypass for DataDome and Incapsula.**
No headless browsers. No infrastructure to run. Solves in milliseconds.

[![Website](https://img.shields.io/badge/Website-disasm.dev-f59e0b?style=for-the-badge&labelColor=171717)](https://disasm.dev)
[![Docs](https://img.shields.io/badge/Docs-docs.disasm.dev-f59e0b?style=for-the-badge&labelColor=171717)](https://docs.disasm.dev)
[![Blog](https://img.shields.io/badge/Blog-read-f59e0b?style=for-the-badge&labelColor=171717)](https://disasm.dev/blog)
[![Discord](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fdiscord.com%2Fapi%2Finvites%2FukxJm45r7Q%3Fwith_counts%3Dtrue&query=%24.approximate_member_count&label=Discord&suffix=%20members&style=for-the-badge&logo=discord&logoColor=white&color=171717&labelColor=5865F2)](https://discord.gg/ukxJm45r7Q)

[Quick start](#quick-start) · [Coverage](#coverage) · [Why request-based](#why-request-based) · [Examples](#examples) · [Blog](#from-the-blog)

</div>

---

## What is DISASM?

DISASM is a request-based anti-bot bypass API. We reverse-engineer the major bot-protection systems and expose them as a single HTTP call: send a request, get back the sensor, token, or cookie you need. No browser farm, no fingerprint maintenance, and none of the cat-and-mouse on your side.

Built for web scraping, automation, testing, and bot development against protected sites. The hard part, keeping up with each vendor's detection changes, is ours.

---

## Quick start

1. Sign up at the [dashboard](https://dashboard.disasm.dev) and create an API key
2. Skim the [docs](https://docs.disasm.dev) for the service you need
3. Send your key in the `x-api-key` header to the service host
4. Get a solve back in milliseconds, no browser involved

---

## Coverage

| Protection | What we solve |
| --- | --- |
| **Incapsula / Imperva** | `reese84` sensors, `___utmvc` cookies, dynamic script handling |
| **DataDome** | Interstitial and slider-CAPTCHA solves, payload and cookie generation |

**Incapsula / Imperva**
- `reese84` sensor solving
- `___utmvc` cookie generation
- Dynamic challenge-script handling

**DataDome**
- Interstitial ("Verifying your device") solves
- Slider CAPTCHA solves, including audio mode
- `tags` payload and `datadome` cookie generation
- Matching client hints returned with every solve

---

## Why request-based?

The usual approach is to point a real browser at the challenge. In 2026 that is slow, expensive, and the first thing the vendors catch.

<table>
<tr>
<th width="50%">Headless browsers</th>
<th width="50%">Request-based (DISASM)</th>
</tr>
<tr>
<td valign="top">

- Slow: 5 to 10+ seconds per solve
- Heavy RAM and CPU for every browser
- Complex infrastructure to run and babysit
- Hard to scale past a handful of sessions
- Automation and fingerprint leaks get flagged
- Breaks every time the vendor ships an update

</td>
<td valign="top">

- Fast: roughly 50 to 100 ms per solve
- Minimal resources, no browser to run
- A single HTTP call, nothing to host
- Scales to thousands of concurrent solves
- No browser fingerprint surface to leak
- We maintain the reverse engineering, not you

</td>
</tr>
</table>

---

## Examples

Every call authenticates with your key in the `x-api-key` header. Grab one from the [dashboard](https://dashboard.disasm.dev). Full reference at [docs.disasm.dev](https://docs.disasm.dev).

**DataDome (interstitial)**

```bash
curl -X POST https://dd.antibotapi.com/interstitial \
  -H "x-api-key: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "device_check_link": "https://geo.captcha-delivery.com/interstitial/?initialCid=...",
    "device_check_page": "<base64 of the challenge page>",
    "user_agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) ...",
    "ip_address": "<your proxy IP>"
  }'
```

Returns a payload to submit plus the exact client hints to send.

**Incapsula (reese84)**

```bash
curl "https://incap.antibotapi.com/incapsula/reese84?url=https://example.com/<challenge-path>" \
  -H "x-api-key: YOUR_API_KEY"
```

Returns a solution; submit it to the site's reese84 endpoint to receive the cookie.

---

## From the blog

We write up how these systems actually work, and how we get past them.

- [How to Bypass DataDome in 2026: Interstitial & Slider CAPTCHA](https://disasm.dev/blog/how-to-bypass-datadome/)
- [Writing a JavaScript VM in Go](https://disasm.dev/blog/writing-a-javascript-vm-in-go/)

---

## Get started

1. [Read the docs](https://docs.disasm.dev)
2. [Grab a key](https://dashboard.disasm.dev)
3. [Ask us in Discord](https://discord.gg/ukxJm45r7Q)
