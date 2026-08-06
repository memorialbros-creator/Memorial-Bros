# DNS — memorialbros.com

## Current state

| | |
|---|---|
| Registrar | Wix.com Ltd. (registered 2026-07-26, renews 2027-07-25) |
| Registrar transfer lock | ICANN 60-day hold — transferable from **~2026-09-24** |
| Nameservers | Moving from `ns10/ns11.wixdns.net` → Cloudflare |
| Hosting | Cloudflare Workers static assets, project `memorialbros` |

Cloudflare nameservers assigned to this zone:

```
pam.ns.cloudflare.com
vicente.ns.cloudflare.com
```

## ⚠️ Records that MUST survive the move

### Google verification (TXT)

```
Type:  TXT
Name:  memorialbros.com   (root / @)
Value: google-site-verification=obB09GUU-1-utZJ6279ZyoXM4oiuLdn9A9pqDz-a5b0
```

This proves domain ownership to Google Search Console / Business Profile. If it's
missing after the nameserver change, Google un-verifies the domain and the Business
Profile listing can be affected. Verify it exists in Cloudflare **before** assuming
the migration is complete.

## Records being deliberately abandoned

These are Wix parking records. They should NOT be recreated in Cloudflare — they'll
conflict with the Worker's custom domain.

| Type | Host | Value |
|---|---|---|
| A | memorialbros.com | 185.230.63.107 |
| A | memorialbros.com | 185.230.63.186 |
| A | memorialbros.com | 185.230.63.171 |
| CNAME | www.memorialbros.com | cdn1.wixdns.net |

Cloudflare's zone scan imports these automatically. **Delete them in Cloudflare's
DNS → Records** before attaching the custom domain, or the attach will conflict.

## ⚠️ EMAIL — Google Workspace is live on this domain

The domain runs **Google Workspace email**. Wix's DNS panel confirms it:
"Your domain is setup to connect to a Google Workspace email account."

**Losing these records means inbound email stops arriving.** They must exist in
Cloudflare *before* the nameservers are switched.

### The complete set — three records

Queried live against Google's resolvers (8.8.8.8), so this is authoritative, not
read off a truncated UI. Recreate these three in Cloudflare exactly:

| Type | Name | Priority | Value |
|---|---|---|---|
| MX | `memorialbros.com` (root / @) | 10 | `aspmx.l.google.com` |
| TXT | `memorialbros.com` (root / @) | — | `v=spf1 include:_spf.google.com ~all` |
| TXT | `memorialbros.com` (root / @) | — | `google-site-verification=obB09GUU-1-utZJ6279ZyoXM4oiuLdn9A9pqDz-a5b0` |

Confirmed absent, so nothing to migrate: **DKIM** (`google._domainkey`) and
**DMARC** (`_dmarc`) are not published.

⚠️ Worth fixing later, separately from this migration: no DKIM and no DMARC means
outbound mail from the domain is easier to spoof and more likely to be filtered.
Google Workspace can generate a DKIM key from Admin → Apps → Google Workspace →
Gmail → Authenticate email. Add DMARC once DKIM is live.

### Cutover order (avoids any email gap)

1. In **Cloudflare → DNS → Records**, confirm all three records above exist. The zone
   scan usually imports them; add by hand whatever is missing.
2. Delete the imported Wix parking records (below).
3. **Only then** change the nameservers at Wix.

Getting Cloudflare's zone correct *before* the flip means there is no window where
mail has nowhere to go.

## Checklist

- [ ] **Google Workspace MX records present in Cloudflare** (do this FIRST)
- [ ] SPF TXT present in Cloudflare
- [ ] DKIM (`google._domainkey`) present, if it exists today
- [ ] Nameservers changed at Wix to pam/vicente
- [ ] Cloudflare zone shows **Active**
- [ ] Google TXT verification record present in Cloudflare
- [ ] Wix parking A records + www CNAME deleted in Cloudflare
- [ ] `memorialbros.com` attached to the Worker as a custom domain
- [ ] `www.memorialbros.com` attached too
- [ ] HTTPS serving, both with and without www
- [ ] Registrar transferred to Cloudflare (on/after ~2026-09-24)
