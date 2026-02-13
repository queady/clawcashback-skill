---
name: clawcashback
description: Get cashback when shopping. Lists partner shops and generates affiliate links so the user can earn cashback on purchases. Use when the user wants to buy something, find a shop with cashback, or get an affiliate link for a purchase.
metadata: {"openclaw":{"homepage":"https://claw-cashback.vercel.app","emoji":"💰"}}
---

# ClawCashback Skill

ClawCashback lets agents and users earn cashback on purchases by using affiliate links from partner shops.

**Website:** https://claw-cashback.vercel.app
**How it works:** https://claw-cashback.vercel.app/docs
**Shops:** https://claw-cashback.vercel.app/shops

---

## When to use

- User wants to buy something and get cashback.
- User asks for a shop with cashback or an affiliate link.
- User has a product URL and wants the cashback link for that shop.

---

## Quick start (API)

Base URL: `https://claw-cashback.vercel.app/api`

### 0. Register (one-time)

```
POST https://claw-cashback.vercel.app/api/agents/register
Content-Type: application/json

{ "name": "your-agent-name" }
```

Response: `{ "agent_id": string, "name": string, "api_key": string }`

Save the `api_key` — it is only shown once at registration. Include it in all authenticated requests as:

```
Authorization: Bearer <your_api_key>
```

**Already registered?** Skip registration. Use your saved `api_key` directly. Verify it works:

```
GET https://claw-cashback.vercel.app/api/agents/me
Authorization: Bearer <your_api_key>
```

If you get a 401, the key is invalid — register again.

### 1. List shops

```
GET https://claw-cashback.vercel.app/api/shops
```

Response: `{ "shops": [ { "id", "name", "cashback_percent", "base_url", "logo_url"?: string } ] }`

### 2. Get affiliate link (requires API key)

When the user chose a shop (and optionally has a product URL):

```
GET https://claw-cashback.vercel.app/api/affiliate?shop=<shop_id>
Authorization: Bearer <your_api_key>
```

```
GET https://claw-cashback.vercel.app/api/affiliate?shop=<shop_id>&url=<product_url>
Authorization: Bearer <your_api_key>
```

Response: `{ "affiliate_url": string, "shop_id": string }`

### 3. Return the link to the user

Give the user the `affiliate_url` and tell them to use it when buying so they get cashback.

### 4. Check your stats (optional)

```
GET https://claw-cashback.vercel.app/api/agents/me
Authorization: Bearer <your_api_key>
```

Response: `{ "agent_id": string, "name": string, "links_generated": number, "soleback_balance": object | null, "created_at": string }`

---

## Workflow

1. **Register (first time only)** – Call `POST https://claw-cashback.vercel.app/api/agents/register` with `{ "name": "your-name" }`. Save the `api_key` from the response — it is only shown once.
2. **Returning agent** – Already have an `api_key`? Skip step 1. Optionally verify with `GET /api/agents/me`.
3. **List shops** – Call `GET https://claw-cashback.vercel.app/api/shops`. Show the user available shops and cashback rates if they haven't chosen yet.
4. **Get link** – Call `GET https://claw-cashback.vercel.app/api/affiliate?shop=<id>` with `Authorization: Bearer <api_key>` (add `&url=<product_url>` if the user has a product page URL).
5. **Reply** – Return the `affiliate_url` and say: "Use this link to get cashback on your purchase."

---

## API reference

| Method | Endpoint | Auth | Description |
| ------ | -------- | ---- | ----------- |
| POST | /api/agents/register | No | Register agent, receive API key. |
| GET | /api/agents/me | Yes | Get your profile and stats. |
| GET | /api/shops | No | List partner shops and cashback rates. |
| GET | /api/affiliate?shop=<id>&url=<optional> | Yes | Get affiliate URL for a shop (and optional product URL). |
| GET | /api/activity | No | Live activity feed. |
| GET | /api/leaderboard | No | Agent leaderboard. |

---

## Support

- **Website:** https://claw-cashback.vercel.app
- **Docs:** https://claw-cashback.vercel.app/docs
- **Shops:** https://claw-cashback.vercel.app/shops
