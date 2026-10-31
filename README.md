# dedicated cloud: How It Actually Works, What It Costs, and When Sharktech's Fixed-Resource Model Beats Pay-As-You-Go

A dedicated cloud gives you a fixed pool of compute, storage, and network resources that nobody else touches, billed at a price that doesn't move at the end of the month. That's the short answer to what most people are actually looking for when they search this term: predictable costs and single-tenant infrastructure without giving up the convenience of cloud deployment.

This guide covers how dedicated cloud differs from public and private cloud, what it actually costs at a provider built for this model, and the billing fine print that tends to surprise people after they've already signed up.

> **Quick take:** For steady workloads with a predictable budget, a fixed-resource dedicated cloud allocation is the safer billing model — you pay for what you reserved and never get hit with usage overages. On Sharktech's OpenStack platform, the entry-level cloud plan (Small) starts at **$39/month** in USD, with Medium at $79, Large at $249, and Enterprise at $499. [👉 Check current Sharktech cloud plan pricing](https://bit.ly/SharKtech)

## What is a dedicated cloud, exactly?

A dedicated cloud is single-tenant cloud infrastructure — your compute, storage, and analytics instances run on resources allocated exclusively to you, deployed outside your own on-premises environment. The term has been picking up steam partly because IDC formally retired "private cloud" in its market taxonomy and replaced it with "dedicated cloud," keeping the same underlying definition: environments dedicated to one organization.

Where the terminology gets fuzzy is the boundary with private cloud. Equinix draws a useful line: a dedicated cloud combines single tenancy with access to services from multiple cloud providers, whereas a traditional private cloud is usually a single-vendor, single-tenant stack. On Sharktech's own platform, the distinction is stated even more directly — Dedicated Cloud is a fixed resource allocation on their shared OpenStack cloud platform, while Private Cloud means exclusive physical hardware built for one organization. Same word family, very different hardware commitment and price.

The practical takeaway: if you search "dedicated cloud" and land on pages about hyperscaler dedicated instances, bare-metal-as-a-service, or fixed-allocation hosting, you're all looking at variations of the same idea. The core promise is "your resources, no neighbors, no surprise bills."

## How it differs from public cloud (and why the difference is mostly about billing)

Both models run on the same physical infrastructure in most modern hosting setups — that's true at Sharktech, where Public Cloud and Dedicated Cloud share the same OpenStack platform and the same hyper-converged architecture. The real difference is how you pay and what happens when you exceed your plan.

| What matters | Public Cloud | Dedicated Cloud |
| --- | --- | --- |
| Billing | Monthly base plus metered usage above included resources | Fixed price for your chosen resource allocation |
| Growth | Add resources at published hourly rates | Upgrade allocation through the billing platform |
| Overruns | Ceilings on Small/Medium/Large; no preset ceiling on Enterprise | No automatic overages, ever |
| Best for | Workloads with changing resource demand | Steady workloads and predictable budgets |

That last row is the entire decision in one line. Public cloud is designed around elasticity: you pay a base commitment, then get metered for anything beyond your included pool. Dedicated cloud flips it — Sharktech's own wording is "if you pay for 8 cores, you get 8 cores. No more, no less."

The cost-predictability argument for dedicated infrastructure is well documented at scale. Cloud bills balloon when instance configurations don't match workloads, when data crosses availability zones and triggers egress fees, or when per-call API usage exceeds forecasts. With a dedicated allocation, bandwidth and transfer costs are known upfront. The trade-off, as The New Stack puts it, is that you need the infrastructure expertise to tune it: "Running applications optimally on a dedicated cloud, too, requires effort and expertise." A bare-metal-class server costs more than a cloud VM but delivers far more horsepower — if your workload actually uses it.

## Sharktech's cloud plans and current pricing

The full plan lineup currently shown on Sharktech's cloud pricing page looks like this. All prices are USD, monthly billing:

| Plan | Base price (USD/mo) | Billing model | Buy |
| --- | --- | --- | --- |
| Small | $39.00 | Base commitment + metered usage above included pool | [ Configure the Small plan](https://bit.ly/SharKtech) |
| Medium | $79.00 | Base commitment + metered usage above included pool | [ Configure the Medium plan](https://bit.ly/SharKtech) |
| Large | $249.00 | Base commitment + metered usage above included pool | [ Configure the Large plan](https://bit.ly/SharKtech) |
| Enterprise | $499.00 | Base commitment + metered usage, no preset resource ceiling | [ Configure the Enterprise plan](https://bit.ly/SharKtech) |
| Custom | Quoted | Tailored allocation via sales team | [ Request a custom quote](https://bit.ly/SharKtech) |

A few things worth knowing about how these plans work before you click anything:

- **The base price covers an included resource allocation.** Anything beyond it is charged at published rates — compute by the hour, storage on a 730-hour estimated month, transfer and IPs monthly.
- **Resource ceilings are not spending caps.** This is stated explicitly in Sharktech's own FAQ, and it's the single most misunderstood line on the page. A ceiling on Small/Medium/Large limits how many extra resources you can add, not how much money you can spend. Enterprise has no preset ceiling at all.
- **Storage keeps billing while a VM is stopped.** Even powered-off machines hold their allocated storage for the estimated month. If you plan to spin VMs up and down frequently, factor that in.
- **Third-party reviews list the headline specs** for each tier — Small at roughly 4–16 cores, 8–32GB RAM, 100GB+ SSD and 5TB bandwidth; Enterprise at 64+ cores, 128GB+ RAM, 5,000GB storage and 20TB bandwidth — but the official calculator prices by allocation, so your actual VM configuration is set after ordering. [👉 See the full plan breakdown on Sharktech's site](https://bit.ly/SharKtech)

One note on published hourly rates, since they determine what "usage above the pool" actually costs: compute runs $0.0025 per CPU core/hour and $0.0035 per GB of RAM/hour. Storage runs from $0.000020/GB/hr on HDD up to $0.000090/GB/hr on NVMe. IPv4 addresses are $1.50/month each (up to 16), and data transfer is billed at $0.002/GB.

## The egress problem, and where dedicated cloud providers actually differ from the big clouds

Data transfer fees are the line item that makes people rethink hyperscalers. Outbound (egress) traffic in particular can quietly become a large share of a monthly bill, and high egress pricing effectively locks data in — moving it out gets expensive.

Sharktech's approach, per their own product pages: no charges for ingress traffic, and egress at $0.002/GB, which they position as substantially lower than the major clouds. Combined with the fixed-allocation model, the effect is that a dedicated cloud bill here is knowable in advance — the same allocation, the same price, every month.

There's a second structural difference: no proprietary lock-in. The platform is OpenStack — the same open-source stack that powers many enterprise private clouds — with RESTful APIs covering compute (Nova), block and object storage (Cinder and Swift), networking (Neutron), and identity (Keystone). You can upload your own VM images and ISOs, and pull them back out. If you later decide to move, your images and automation scripts travel with you. One restriction to know: API access is limited to approved source IPs by default, so you'll need to whitelist your management hosts before wiring up automation.

## Who a dedicated cloud is actually for

Straight answer: teams with steady workloads and someone who knows their way around Linux.

The strongest fits, based on the documented use cases:

- **Databases and latency-sensitive business apps** that suffer from noisy-neighbor variance on shared public cloud VMs. Single tenancy means your performance doesn't depend on what a stranger's cron job is doing.
- **Development and staging environments** that run 24/7 at known capacity — the fixed bill is cheaper than metered surprises.
- **Home-lab-style remote infrastructure** for learning, testing operating systems, or building networks you don't want in your closet.
- **Multi-layer enterprise architectures** needing isolated private networks, security groups, load balancers, and storage tiering (NVMe for hot data, SSD for general use, HDD for archives).

Who should skip it: if your resource demand swings wildly month to month — spiky batch jobs, seasonal traffic, short-lived experiments — the pay-as-you-go public cloud model will likely cost you less, because you're not paying for reserved capacity sitting idle. And if you want managed services up the stack (managed databases, serverless, turnkey AI endpoints), dedicated cloud providers generally don't offer them; you build on infrastructure primitives or bring your own tooling. That's the deal everywhere in this category, not just here.

One more angle worth knowing: the platform includes per-IP DDoS protection built into the base price rather than sold as an add-on, with third-party reviews citing 60Gbps per IP standard. If your workload has ever been taken offline by an attack and billed extra for "mitigation," that pricing structure matters.

## FAQ

**Is a dedicated cloud the same as a private cloud?**
No. A dedicated cloud is a fixed, single-tenant resource allocation on a shared cloud platform. A private cloud is exclusive physical infrastructure built for one organization. IDC now uses "dedicated cloud" as the umbrella market term for the latter category, but individual providers use the words differently — always check which one a pricing page means.

**Do I still get surprise bills on a dedicated cloud?**
Not from usage overages — there are no automatic overage charges on a fixed allocation. Your bill changes only when you deliberately upgrade the allocation through the billing platform.

**Can I split one dedicated cloud allocation across multiple VMs?**
Yes. Billing is calculated on the whole resource pool, not per named VM plan. An allocation of 8 vCPUs, 8GB RAM, and 300GB SSD can be divided across any number of virtual machines, and you can run Linux or Windows.

**Are there promo codes for Sharktech cloud plans?**
Third-party reviews report a code called WHTFALL giving 33% recurring off cloud virtual data center services, which would bring entry-level pricing to around $26/month. I can't verify that against Sharktech's own site, so treat it as unconfirmed and check at checkout.

**How many data center locations are available?**
Five: Los Angeles, Las Vegas, Denver, Chicago, and Amsterdam. You pick the location when ordering your resource pool, and all locations run the same OpenStack platform with 24/7 support.

## Before you order: a quick decision rule

Running steady workloads — a production database, always-on app servers, a home lab you'll keep for months — pick a fixed dedicated cloud allocation and enjoy the bill that never moves. Testing things, running spiky jobs, or genuinely unsure of your footprint? Start on the pay-as-you-go side with the Small plan at $39/month and let a month of real usage data tell you what allocation you'd actually need. If that month shows flat, predictable consumption, upgrade to the fixed model; if it swings, stay metered.

Custom requirements — more compute, storage, or bandwidth than the published tiers — go through the sales team rather than the checkout. And if you already know which side of that line you're on: [👉 deploy your Sharktech cloud in one of five data centers](https://bit.ly/SharKtech) and confirm the final configuration in the client portal before the first bill lands.
