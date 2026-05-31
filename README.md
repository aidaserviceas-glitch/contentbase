# ContentBase — Free AI Listing Optimizer

> Paste your Etsy, Shopify, or Amazon listing. Get an optimized version with better title, description, and 13 keyword-targeted tags in 10 seconds.

**[Try it free →](https://contentbase-crw.pages.dev)** · No signup · 5 free/day · Pro $9/month

---

## The problem

Most e-commerce listings are written from a maker's perspective, not a buyer's search perspective.

| ❌ Before | ✓ After |
|---|---|
| "Handmade ceramic mug, blue, 12oz" | "Handmade Ceramic Coffee Mug \| Blue Stoneware 12oz \| Gift for Coffee Lovers" |
| Ranks for almost nothing | Ranks for a dozen buyer searches |

Same product. One gets found. One doesn't.

## Products

### ContentBase — Free listing optimizer
- Rewrites title (keyword-structured, platform-specific format)
- Rewrites description (buyer psychology first)
- Generates 13 keyword-targeted tags
- **Free:** 5/day, no signup · **Pro:** $9/month unlimited

→ **[contentbase-crw.pages.dev](https://contentbase-crw.pages.dev)**

### ShopScan — $19 Etsy SEO Audit
- Full 12-page audit of a specific listing
- Score (A-F), keyword gap analysis, full copy rewrite
- Action plan with prioritized improvements
- Delivered to your email within 24 hours

→ **[shopscan-eff.pages.dev](https://shopscan-eff.pages.dev)**

---

## Stack

| Layer | Tech |
|---|---|
| API / Business logic | Cloudflare Workers |
| Rate limiting + token storage | Cloudflare KV |
| Frontend hosting | Cloudflare Pages |
| AI | Anthropic Claude (Haiku for optimize, Sonnet for audits) |
| Transactional email | Brevo |
| Payments | Stripe Payment Links |
| Infrastructure cost | $0/month |

## Architecture highlights

### Pro activation without webhooks

After Stripe payment, the customer is redirected to:
```
https://contentbase-crw.pages.dev?session_id=cs_live_...
```

The frontend POSTs the session_id to `/activate-pro`. The Worker validates the format (unguessable `cs_live_` prefix), generates a token, stores in KV, emails via Brevo. No webhook endpoint, no signing secret needed.

### Rate limiting via KV

```javascript
async function checkRateLimit(ip, env) {
  const key = `rate:${ip}:${new Date().toISOString().split('T')[0]}`;
  const count = parseInt(await env.CB_RATE.get(key) || '0');
  if (count >= 5) return { allowed: false };
  await env.CB_RATE.put(key, String(count + 1), { expirationTtl: 86400 });
  return { allowed: true };
}
```

Keys auto-expire after 24h. No cleanup, no database.

---

## SEO Resources

- [Free Etsy Title Generator](https://contentbase-crw.pages.dev/etsy-title-generator.html)
- [Etsy SEO Checklist 2026](https://contentbase-crw.pages.dev/etsy-seo-checklist-2026.html)
- [Best Free Etsy SEO Tools 2026](https://contentbase-crw.pages.dev/best-etsy-seo-tools-2026.html)

---

Built by [A.D. Rasmussen](mailto:andersrasmussen250@gmail.com)
