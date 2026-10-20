# Merch/Ops Dashboard

Internal operations dashboard for a heavyweight blank merch brand.

## Stack
- **Frontend:** Next.js 14 + React 18
- **Database:** Supabase (PostgreSQL)
- **Hosting:** Vercel
- **Auth:** Supabase RLS (anon key = read-only; n8n writes via service role key)

## Pages
| Tab | What it shows |
|---|---|
| Overview | Today's revenue, MTD revenue, net profit, AOV, 14-day bar chart, latest orders |
| Orders | Full order table with estimated profit per order and fulfillment status |
| Products | SKUs, all-in cost, retail price, gross margin %, units sold |
| Content | TikTok/Instagram/X post performance — views, clicks, sales attributed |
| Creators | Affiliate CRM — followers, commission %, videos posted, sales, ROI |
| Traffic | Tracked /go/ bio links + click counts by source (no social API needed) |
| Profit | MTD revenue, gross margin, ads+refunds, net after expenses + expense log |

## Tracked bio links
Put these in each profile bio — every click logs to Supabase automatically:
- `/go/tt` → TikTok
- `/go/ig` → Instagram
- `/go/x` → X
- `/go/shop` → TikTok Shop storefront

Update destinations in `app/go/[src]/route.js` once brand handles exist.

## n8n integration
n8n writes to Supabase using the **service role key** (from Supabase project settings → API).
The dashboard reads using the anon key (already in `lib/supabase.js`).

## Deploy to Vercel
1. Push this repo to GitHub
2. Vercel dashboard → Add New Project → Import → select this repo
3. Vercel auto-detects Next.js — click Deploy
4. Done. No build config needed.

## Wipe demo data before go-live
```sql
truncate orders, order_items, daily_metrics, content_posts, creators, traffic_events, expenses restart identity cascade;
```
Run this in Supabase → SQL Editor once real orders start flowing.

## Supabase project
- Project: `merch-ops`
- Region: us-east-1
- URL: https://bjmgcfybzyfafpitbprp.supabase.co
