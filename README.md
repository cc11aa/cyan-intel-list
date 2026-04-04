# Cyan Intel - Baseline Source List 🌐

Welcome to **cyan-intel-list**, the official baseline collection of Cyber Threat Intelligence (CTI) sources for the **Cyan Intel** platform. 

This repository contains the primary testing list used to populate the local Cyan Intel ingestion engine. It includes a curated mix of web scrapers, RSS feeds, and YouTube podcasts designed to provide a high-signal stream of threat intelligence.

---

## 🛠️ File Structure & Parsing Expectations

The Cyan Intel ingestion worker parses text files using explicit block markers to categorize and route URLs to the correct internal scraping module. 

To add or modify sources, you must strictly wrap the URLs in their corresponding `--START [TYPE]--` and `--END [TYPE]--` markers. Each URL must be on its own line.

### Supported Blocks

* **`--START WEB SCRAPER--` / `--END WEB SCRAPER--`**
    * **Purpose:** Standard HTML scraping. The worker will attempt to fetch the page, strip out noise, and extract core text for AI analysis. Best for press releases, blogs, and static advisory pages.
* **`--START RSS FEED--` / `--END RSS FEED--`**
    * **Purpose:** XML/Atom feed parsing. This is the most efficient ingestion method. The worker relies on batched inserts and caches requests using `ETag`/`Last-Modified` headers to minimize database write overhead and bandwidth.
* **`--START YOUTUBE PODCAST--` / `--END YOUTUBE PODCAST--`**
    * **Purpose:** YouTube playlist or channel ingestion. (Requires appropriate extraction logic in the worker to handle video metadata or transcripts).
* **`--START SOCIAL MEDIA--` / `--END SOCIAL MEDIA--`**
    * **Purpose:** Designated for social media profiles. 
    * **⚠️ IMPORTANT:** Cyan Intel enforces a native denylist for social media domains (e.g., `x.com`, LinkedIn, Facebook) to prevent accidental crawling. Even if placed in this block, domains on the denylist will be ignored or blocked by the ingestion worker.

---
