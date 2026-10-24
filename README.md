# real estate data scraper: which tools actually beat Zillow's anti-bot walls, what each request really costs, and how to pick a plan without burning credits

Most people searching for a real estate data scraper hit the same wall within an hour: Zillow serves an empty JavaScript shell, Realtor.com sits behind Cloudflare, and a homemade script gets its IP blocked after a handful of requests. The real question is not "is there a scraper for real estate" — dozens exist — but "which one actually succeeds on the site you care about, and what does each successful request cost you."

Short answer up front: if your main target is Zillow or Redfin, ScraperAPI is one of the strongest options in its price range — independent benchmarks put its Zillow success rate between 94% and 100%. If your main target is Realtor.com, it performs badly (12–24% in recent benchmarks) and you'd be better served elsewhere, such as Bright Data, which maintains pre-built Realtor scrapers.

What people actually do with scraped property data falls into a handful of recurring jobs:

- **Investment tracking** — price history, days on market, distressed listings
- **Rental market analysis** — average rents and vacancy patterns
- **Lead generation** — agent contacts, FSBO and expired listings
- **Comparative market analysis** — matching comps by square footage, neighborhood, and recent sales
- **Portfolio monitoring** — continuous watch on new listings within a geographic radius

The tool you pick should match the job. Here's the full picture.

> **Quick verdict**
> For Zillow- and Redfin-focused scraping, the **Hobby plan at $49/month** (100,000 credits, 20 concurrent requests) is the realistic starting point — with annual billing it drops to $44.10/month. The 7-day trial gives you 5,000 free credits with no credit card, so you can test your actual target URLs before paying anything.
> 👉 [Try ScraperAPI free for 7 days — 5,000 credits, no card required](https://www.scraperapi.com/?fp_ref=coupons)

## Why scraping property portals is harder than scraping most websites

Real estate sites sit near the top of the difficulty scale for a structural reason: the data is extremely valuable and the sites know it. Zillow has 228 million monthly active users and recorded 9.3 billion site visits in 2024, covering more than 130 million US homes — all protected by a dual-layer anti-bot stack of PerimeterX plus Cloudflare that ScrapeOps rates 8 out of 10 for difficulty. Most DIY scripts fail within seconds of the first request. Realtor.com runs Cloudflare too, and Redfin uses aggressive rate limiting plus JavaScript-rendered content, so a plain HTTP request comes back with nothing useful.

That means any real estate data scraper worth paying for has to handle four things at once: JavaScript rendering (otherwise you get empty HTML), residential proxy rotation (datacenter IPs die in minutes on these sites), CAPTCHA solving, and geotargeting if the listings you see depend on where the request appears to come from. Building all four yourself is a multi-month project; this is exactly the problem scraping APIs sell as a one-line solution.

One caveat on legality and access: Zillow's terms restrict automated collection, and the company actively enforces its protections. Property data scraping is widely practiced for investment analysis, rental market research, and lead generation, but it operates in a contested zone — use the data responsibly, and don't assume a technical workaround is a legal green light.

## ScraperAPI plans and pricing: the full picture

ScraperAPI uses a credit system. Every plan includes the same core feature set — JavaScript rendering, premium residential and mobile IPs, advanced anti-bot bypassing, structured data parsing, the no-code DataPipeline scheduler, and pay-as-you-go overflow. What changes between tiers is the credit allowance, concurrency, and geotargeting depth.

| Plan | Monthly price | Annual (10% off) | Credits | Concurrency | Get started |
| --- | --- | --- | --- | --- | --- |
| Free | $0 | — | 1,000/month | 5 | [Create a free account](https://www.scraperapi.com/?fp_ref=coupons) |
| Hobby | $49 | $44.10/mo | 100,000 | 20 | [Start on Hobby](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Startup | $149 | $134.10/mo | 1,000,000 | 50 | [Start on Startup](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Business | $299 | $269.10/mo | 3,000,000 | 100 | [Start on Business](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Scaling | $475 | $427.50/mo | 5,000,000 | 200 | [Start on Scaling](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Professional | $975 | $877.50/mo | 10,500,000 | 300 | [Start on Professional](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Advanced | $1,975 | $1,777.50/mo | 21,500,000 | 500 | [Start on Advanced](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Enterprise | Custom | Custom | 22,000,000+ | 500+ | [Talk to sales about Enterprise](https://www.scraperapi.com/contact-sales/?fp_ref=coupons) |

A few details the pricing page doesn't put in the headline:

- **Annual billing saves 10% across every tier.** On Hobby that's $529.20/year instead of $588 — a real $58.80 difference, not a rounding error.
- **Over the credit limit?** Monthly plans on Hobby through Professional can opt into auto-upgrade, which bumps you to the next tier at a better per-credit rate. Scaling and above get pay-as-you-go at a fixed per-credit rate, capped at a monthly spending limit you set yourself.
- **Concurrency is a hard wall.** Exceed it and the API returns a 429 — requests are refused, not queued, and you're not charged for the refusals. If your pipeline fires thousands of parallel requests, concurrency matters as much as the credit pool.
- **There's a 7-day no-questions-asked refund policy**, and you can cancel anytime from the dashboard without being charged. That makes the first month essentially risk-free.

## What a "1 credit" page actually costs — the math most reviews skip

Here's the part that trips people up, and it's the single most useful thing to understand before choosing a plan: credits are not 1:1 with pages.

A standard page on a simple HTML site costs 1 credit. But real estate portals almost never qualify. The stacking rules look like this:

- Base request: 1 credit
- JavaScript rendering (`render=true`): +10
- Premium proxy (`premium=true`): +10
- Automatic anti-bot bypass (triggers on Cloudflare/DataDome/PerimeterX sites): +10
- Premium proxy **and** JS rendering together: +25 — the stack is non-linear, not +20

Run the numbers on a realistic case. A Cloudflare-protected property portal with JavaScript rendering costs a minimum of 21 credits per request. The Hobby plan's 100,000 credits therefore covers about **4,760 actual property pages per month** — nowhere near 100,000. For a Zillow search page with rendering enabled, you're looking at 11 credits per page, which works out to roughly 9,000 pages on Hobby and 270,000+ on Business with identical feature settings.

Three tools in the dashboard keep this under control:

1. **Domain Cost Estimator** — check the exact credit cost of any URL before you commit a monthly budget to it.
2. **`max_cost` parameter** — caps the credit spend per request, so a batch job can't silently drain your plan by wandering onto an unexpectedly expensive target.
3. **DataPipeline awareness** — the no-code scheduler is convenient for recurring jobs like "3BR under $500K in Austin, listed in the last 30 days," but each DataPipeline request costs 6 credits versus 1 via the standard API. If you're comfortable writing a cron job, the standard API is six times cheaper.

This credit math is the gap between most reviews and reality. The sticker price of a plan tells you almost nothing; the credits-per-page figure on *your specific target site* tells you everything.

## Benchmark reality: excellent on Zillow, poor on Realtor.com

Independent testing matters more than marketing here, and the two available benchmark sources agree on the broad shape.

Scrapeway's September 2026 benchmark (September 11–25) measured ScraperAPI across 11 target sites: 52% overall success rate against an industry average of 54.2%, ranking 4th of 7 APIs tested, with a 7.5-second average response and a cost of $4.05 per 1,000 requests. But the site-level breakdown is where the real estate story lives: **zillow.com at 94% success** with a $0.49/1k cost, versus **realtor.com at 24%**. An earlier Scrapeway benchmark in April 2026 measured the same pattern even more starkly — 100% on Zillow, 12% on Realtor.com.

The conclusion from that testing is blunt, and worth quoting: *"If Realtor.com is your primary target, ScraperAPI is not the right tool. Its 12% success rate there means 88% of your credits evaporate on failed requests."* (In fairness, the API only bills successful requests — status 200 and 404 — so failed attempts don't drain credits directly, but they do waste your time and throughput.)

Other scrapers have built dedicated Realtor.com scrapers that sidestep this problem. That's not a flaw in ScraperAPI so much as a specialization: its custom bypass logic clearly favors some targets over others, and real estate is not evenly covered.

## How ScraperAPI compares with the main alternatives

Since picking a real estate data scraper is inherently comparative, here's the verified landscape (information only — check each vendor's site for current details):

| Tool | Starting price | Free tier | Known for |
| --- | --- | --- | --- |
| Bright Data | $1.50/1K requests | 5,000 records/mo | 98.44% avg success in independent benchmarks; pre-built Zillow and Realtor scrapers |
| Apify | $49/month | $5 free credits | No-code "actors" with built-in scheduling |
| Oxylabs | $49/month | Free trial | Enterprise-grade reliability |
| [ See ScraperAPI plans](https://www.scraperapi.com/pricing/?fp_ref=coupons) | $49/month | Free trial (1,000 credits/mo) | One-line API integration; 94–100% on Zillow |
| ScrapingBee | $49/month | 1,000 free credits | Fast setup for prototypes |
| Zyte | $0.13/1K responses | $5 free credits | Long-term pipelines on Scrapy |
| Outscraper | PAYG (credit-based) | Free trial | One-off extractions without code |

Where does that leave ScraperAPI? Bright Data's own roundup of Zillow scrapers classifies it as the pick for "small-scale projects needing simple one-line API integration," starting at $49/month. That framing holds up. Bright Data is measurably stronger at raw success rate (98.44% average across all its scrapers in third-party testing) and has the broadest target coverage; ScraperAPI is easier to integrate, cheaper at the entry tier for API-style usage, and fast on the targets it handles well. On a pure cost-per-successful-Zillow-page basis, ScraperAPI's $0.49/1k is genuinely competitive — the $4.05/1k overall average is dragged up by expensive targets like LinkedIn that a real estate user will never touch.

A couple of functional limits are worth knowing before you commit, since they only show up in the documentation: sessions are persistent for 15 minutes (fine for paginated listing results, but you'll need to manage longer crawls yourself), and the API doesn't support custom JavaScript execution, browser-level control, or screenshots. If your workflow depends on clicking through an interactive map search, an API that returns rendered HTML won't cover it.

One more data point for context: Capterra shows ScraperAPI at 4.6 stars from 62 reviews, which is solid if not spectacular, and the company acquired Traject Data in April 2026, adding structured SERP and e-commerce data APIs to the platform.

## The Redfin shortcut most reviews don't mention

If Redfin is on your target list, don't scrape it the generic way. ScraperAPI maintains four structured data endpoints for Redfin — Search, Agent Details, Rental Properties, and For Sale listings — that return parsed JSON directly: price, beds, baths, square footage, address, agent name, days on market, and listing URL. You skip HTML parsing entirely, and more importantly, you skip parser maintenance. When Redfin redesigns its markup, ScraperAPI fixes the parser on their side; your code keeps working.

For anyone monitoring listings over weeks or months (the portfolio-tracking and market-analysis use cases), that reliability difference compounds fast.

## Who shouldn't buy ScraperAPI for real estate data

Honest limits, based on verified numbers:

- **Realtor.com-first projects.** A 12–24% success rate means most of your requests fail. Bright Data's pre-built Realtor scraper is the better-documented path for this specific target.
- **Social media or travel sites.** Twitter/X and Instagram both measured 0% in the September benchmark, and Booking.com managed 1%. Don't buy on the promise of "any site."
- **Very small, occasional jobs.** If you need 200 listings once a month, the free plan (1,000 credits/month, 5 concurrent) plus occasional pay-as-you-go may cover you forever. Paying $49 monthly for that volume doesn't make sense.
- **Budget-critical projects needing the lowest unit cost.** Zyte's $0.13/1K responses undercuts everyone at scale, though it assumes you can handle more of the parsing and session management yourself.

## FAQ

**Does the free trial need a credit card?**
No. Sign-up gives you 5,000 API credits for the first 7 days with no card, and the standing free plan adds 1,000 credits per month (5 concurrent connections) indefinitely. If you're still evaluating after that, support can extend test credits.

**Can I get a refund?**
Yes — ScraperAPI offers a 7-day no-questions-asked refund. Contact support and they process it. Combined with the free trial, that's two ways to test without financial commitment.

**What happens when I run out of credits mid-month?**
By default, scraping pauses until the next billing cycle. On Hobby through Professional you can enable auto-upgrade to jump to the next tier automatically; on Scaling and above, pay-as-you-go continues at a fixed per-credit rate up to a monthly cap you define — no surprise invoices.

**How do I know my target site will work before paying?**
This is the single most important step: register the free tier, run your actual Zillow search URLs and listing pages through the API, and check both the success rate and the credit cost per request in the Domain Cost Estimator. Five minutes of testing beats any review — including this one.

## The bottom line for different situations

- **Monitoring Zillow and Redfin listings weekly** → Hobby at $49/month (or $44.10 on annual), with `render=true` budgeted at ~11 credits per search page. That's roughly 9,000 Zillow pages a month.
- **Multi-city or multi-portal research across Zillow, Etsy-style commerce, and general sites** → Startup ($149) or Business ($299), where per-credit cost drops and concurrency stops being the bottleneck.
- **Production pipelines needing 5M+ credits/month or overflow headroom** → Scaling ($475) or above, where pay-as-you-go and 200+ concurrency keep jobs running during traffic spikes.
- **Realtor.com is your primary target** → ScraperAPI isn't the right tool at current success rates; look at providers with dedicated Realtor scrapers instead.
- **One-off extraction, no developers** → a no-code option like Outscraper or Apify fits better than an API.

Before you commit to any paid tier, do one thing: test your real target URLs on the free trial and read the credits-per-request number. If Zillow pages come back at 11 credits with a 94% success rate, the math works. If your target sits on the wrong side of ScraperAPI's bypass logic, you'll know it in the first five requests instead of the third invoice.

👉 [See all ScraperAPI plans and start your free 5,000-credit trial](https://www.scraperapi.com/?fp_ref=coupons)
