# storage vps for backups: what $25 a year actually buys, the APAC traffic catch, and when a VPS beats object storage

Anyone searching "storage vps for backups" is usually trying to answer one question: is a cheap big-disk VPS a sensible offsite backup target, or should I just pay for object storage? The short version: a storage VPS gives you a flat price, no per-request fees, and full root access for rsync, Borg, or Restic. GreenCloudVPS's Storage KVM line starts at **$25 per year** for 500GB on a 10Gbps port, and its 2TB plan works out to roughly $6.67 a month — with some catches around regional traffic quotas that most listings never mention.

Here's the quick version before the details.

> **Quick picks**
> - One or two servers to back up: **Storage-1** — 1TB SATA, 2 cores, 3GB RAM, 6TB traffic, **$45/year** (US/EU locations).
> - Smallest budget: **Storage-0.5TB** — 500GB, **$25/year**.
> - Need 2TB or want to run Nextcloud alongside backups: **Storage-2** — 2TB, 4 cores, 6GB RAM, **$80/year**.
> - 👉 [Check the current Storage KVM VPS lineup and pricing on GreenCloudVPS](https://greencloudvps.com/billing/aff.php?aff=6679)

## What a storage VPS actually gives you for backups

A storage VPS trades CPU and I/O speed for cheap capacity, and its main advantage over S3-style object storage is pricing predictability. As OBHost puts it on their own storage page, unlike managed cloud storage there are no per-request fees, no API call charges, and no transfer throttling — you pay a flat price and use it. For a backup workload that runs nightly and restores rarely, that pricing model almost always wins.

You also get full root access, which means you can run whatever backup stack you prefer: rsync over SSH, BorgBackup or Restic for deduplicated encrypted snapshots, Duplicati if you want a web UI, or a full Nextcloud instance if you also want a personal cloud. Deduplication matters more than people expect — vendors like AlphaVPS note that Borg-style dedup can compress your retention down to a fraction of the raw space, so a 1TB plan can hold several full backup generations of a few-hundred-GB server.

One framing note: your backup target is one leg of a 3-2-1 setup (3 copies, 2 media types, 1 offsite). A storage VPS is the offsite leg. It should not be the only copy you own, and the section below on reliability explains why that's not just a slogan.

## GreenCloudVPS storage plans: the full current lineup

The Storage KVM VPS line comes in six variants: three in US/EU datacenters (Phoenix AZ, Ogden UT, Amsterdam NL) and three in APAC (Tokyo, Singapore) with the same prices but reduced traffic. All run KVM virtualization on enterprise SATA drives in hardware RAID-10, with a 10Gbps port and NVMe boot disks in some regions. Each plan includes one IPv4 address and an IPv6 block.

| Plan | Storage | CPU / RAM | Traffic | Price | Order |
| --- | --- | --- | --- | --- | --- |
| Storage-0.5TB | 500GB SATA | 1 core / 2GB | 3TB | $25/year | [Order Storage-0.5TB](https://greencloudvps.com/billing/aff.php?aff=6679) |
| Storage-1 | 1TB SATA | 2 cores / 3GB | 6TB | $45/year | [Order Storage-1](https://greencloudvps.com/billing/aff.php?aff=6679) |
| Storage-2 | 2TB SATA | 4 cores / 6GB | 10TB | $80/year | [Order Storage-2](https://greencloudvps.com/billing/aff.php?aff=6679) |
| StorageAPAC-0.5TB | 500GB SATA | 1 core / 2GB | 1TB | $25/year | [Order StorageAPAC-0.5TB](https://greencloudvps.com/billing/aff.php?aff=6679) |
| StorageAPAC-1 | 1TB SATA | 2 cores / 3GB | 2TB | $45/year | [Order StorageAPAC-1](https://greencloudvps.com/billing/aff.php?aff=6679) |
| StorageAPAC-2 | 2TB SATA | 4 cores / 6GB | 4TB | $80/year | [Order StorageAPAC-2](https://greencloudvps.com/billing/aff.php?aff=6679) |

All plans are Linux-only and billed annually. If you need Windows, this line won't work for you — and so far as the plan pages show, there's no monthly billing option on the storage line, so you're committing for a year.

A quick note on GreenCloud's other product lines so you don't waste time: Budget KVM VPS (from $6/month) and Ryzen KVM VPS (also from $6/month) are application servers — 15GB to 150GB of SSD or NVMe. Great for running websites, useless as a backup target. For pure capacity, the Storage KVM line is the only one that makes sense. 👉 [If you want to compare the full GreenCloudVPS catalog side by side](https://greencloudvps.com/billing/aff.php?aff=6679), all lines are listed on the same site.

## Two details most reviews skip

**APAC plans cost the same but get half the traffic — and extra traffic costs five times more.** A StorageAPAC-1 gives you 2TB of traffic for $45/year; the identical US/EU plan gives 6TB. If you ever need more, additional traffic runs $3/TB/month in US/EU but $15/TB/month in APAC. The practical rule: pick APAC only if your main servers sit in Asia and backup-window latency actually matters to you. Otherwise the US/EU plans are strictly better value, and 👉 [the GreenCloudVPS order page lets you switch locations and see live stock](https://greencloudvps.com/billing/aff.php?aff=6679) before you commit.

**The storage add-on is deliberately bad math.** You can bolt on up to 20TB of extra storage at $5/TB/month. That works out to $60/TB per year. Compare: a whole second Storage-2 VPS gives you 2TB, 4 cores, 6GB RAM, and 10TB of traffic for $80/year. Adding 2TB to an existing box would cost you $120/year for storage alone. So if you outgrow 2TB, order a second VPS instead of stacking add-ons. The add-on option makes sense only for a small top-up on an otherwise well-sized box.

RAM and CPU add-ons have a cap most pages bury: you can only add up to what you already have. A 6GB plan can grow to 12GB maximum, at $3/GB/month. For a backup target, honestly, you'll rarely need it.

## What 2TB of backup storage really costs per year

A price comparison across the storage VPS market, with each figure at its vendor's own billing terms:

| Provider | Capacity | Price (as billed) | Notes |
| --- | --- | --- | --- |
| GreenCloudVPS Storage-2 | 2TB SATA | $80/year | 4 cores, 6GB RAM, 10TB traffic, 10Gbps port, annual billing |
| DediRock annual promo | 2TB HDD | $27.68–$32.68/year | 1 core, 2–3GB RAM, 4–6TB traffic, 1Gbps |
| InterServer Storage VPS | 1TB per slice | $3/month per TB ($36/year per TB), monthly billing | Scales to 32TB; managed support only at 4+ slices |
| Contabo Storage VPS 30 | 1TB SSD | €14.00/month base; 15–20% off with longer contract terms | 6 cores, 18GB RAM, 600Mbit/s port; unlimited inbound traffic with fair-use limits |
| Backblaze B2 (object storage) | 2TB | ~$144/year | API costs can add more |
| iDrive E2 (object storage) | 2TB | ~$49.50 first year, $99/year renewal | S3-compatible |

A couple of footnotes on that table. Contabo's €14.00 is the base product price from its own configuration page, with long-term contract discounts on top — renewal pricing after an initial term isn't clearly published, so treat any "first 24 months" promotional numbers you see elsewhere with caution. Contabo's traffic policy explicitly covers unlimited *inbound* transfer under a fair-use policy that reserves the right to throttle abuse; outbound terms aren't spelled out on the pricing page, which matters if you'd ever restore large volumes frequently.

This is also where honesty matters: on pure dollars-per-terabyte, DediRock's annual promos undercut everything. If your only requirement is "2TB to rsync into, cheapest possible," that's a legitimate choice and I won't pretend otherwise.

Where GreenCloudVPS earns its premium is in what surrounds the disk: a 10Gbps port (the DediRock promos are 1Gbps, and one real user on LowEndTalk measured roughly 100Mbps upload from South Korea to New York), four CPU cores and 6GB of RAM that let you run Borg compression or a Nextcloud instance alongside your backups, and Tokyo/Singapore options if your servers are in Asia. And against object storage, the 2TB Storage-2 plan is still cheaper than B2's ~$144/year with none of the per-request accounting.

## Can you trust a cheap storage box with your backups?

Treat any budget storage VPS as a single copy that can disappear, because individual failures do happen. In January 2026, a user report on HostPeers described a GreenCloudVPS 2TB storage box in Chicago going offline overnight, with support eventually confirming the Ceph pool was unrecoverable — and an eleven-hour gap before the ticket got a first reply. In April 2026, the same site documented a different storage node where disk performance degraded badly after a migration, which the author traced to the drive stack rather than CPU load. These are two individual reports, not a statistical record. GreenCloudVPS has been operating for 12+ years on its own hardware, and its pages carry LowEndTalk community awards — though, to be fair about sourcing, those award badges are self-published on GreenCloud's own site. Third-party reviews elsewhere have also noted that support responses can be strict and the account area isn't always intuitive.

But the takeaway generalizes beyond this vendor: cheap storage plans keep prices low partly because redundancy promises are thin. The fix costs nothing — use Borg or Restic so your backup archive is encrypted and verified client-side, keep a second copy somewhere else, and test a restore occasionally. A backup system where the target vanishing means data loss was never really a backup system.

## Setting it up takes about 20 minutes

Once the VPS is provisioned with its IP and root credentials, the workflow is short:

1. Add your SSH public key and disable password auth.
2. Initialize a Borg or Restic repository on the storage box.
3. Point your backup script or systemd timer at it (Restic over SFTP works out of the box).
4. Run one restore test. An untested backup is a hope, and hopes don't survive disk failures.

Since the box runs full Linux with 4 cores on the Storage-2, Borg's compression and encryption run comfortably on either end.

## FAQ

**Can I run a database on a storage VPS?**
Generally no. SATA/HDD arrays have much higher latency than NVMe, and Onidel explicitly advises against database workloads on its HDD-based storage line for that reason — the same logic applies across this product class. Back up databases to it via dumps; don't run them on it.

**Does GreenCloudVPS offer a refund on these plans?**
The plan pages don't advertise a refund window, so confirm with support before ordering — that's one legitimate reason to pause before the annual commitment. Several competitors (AlphaVPS and OBHost both offer 14-day refunds) are more explicit about this, and if a money-back guarantee matters to you, that's a fair tiebreaker. You can also ask GreenCloudVPS directly via 👉 [their support and sales channels before ordering a storage plan](https://greencloudvps.com/billing/aff.php?aff=6679).

**Which location should I pick?**
If your servers are in Europe or the US, take US/EU — you get triple the traffic at the same price. APAC makes sense mainly when your primary machines sit near Tokyo or Singapore and backup transfer latency matters.

## Which plan should you actually order

Match the plan to what you're protecting:

- Backing up one VPS under ~400GB: **Storage-0.5TB at $25/year** is plenty, especially once dedup shrinks your footprint.
- One or two servers with database dumps and images: **Storage-1 at $45/year** gives you 1TB and 6TB of traffic, which is the sweet spot for most people.
- 2TB of data, or backups plus a Nextcloud/media server: **Storage-2 at $80/year**, roughly $6.67 a month.
- Servers in Asia: the APAC plans, but only after accepting the halved traffic quota.
- Needing more than 2TB on a tight budget: DediRock's annual promos are cheaper per terabyte; needing monthly billing or elastic scaling points to InterServer's slice model. Neither is linked here — compare them on their own sites.
- Want zero maintenance and an S3 API: object storage like B2 still earns its keep for automated pipelines.

If you've landed on one of the first three, 👉 [GreenCloudVPS's Storage KVM VPS page has the current stock and prices](https://greencloudvps.com/billing/aff.php?aff=6679) — plans in popular locations do sell out, and the annual billing means it's worth picking the right size the first time rather than planning on the add-on storage.
