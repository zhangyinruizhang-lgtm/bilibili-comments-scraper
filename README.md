Extract viewer comment threads (`video_comments`), comment like counts, reply counts, user levels (`LV0–LV6`), UP creator pinned/liked badges, and **standardized English IP province locations (`regionEn`)** from **Bilibili (哔哩哔哩 / bilibili.com)** — **with 100% no login, no cookies, and no VPN required**.

Built for **Chinese consumer sentiment analysis, regional market-share mapping (province-level geolocation), game & tech launch feedback audits, and LLM dialogue corpora** (n8n, Make, Dify, Claude MCP). Users on the **Apify Free plan ($5 monthly credit) can extract up to 1,111 Bilibili comments per month for free**.

---

## What Can This Bilibili Comments Scraper Do? (Features)

| Bilibili Comments Capability | Built-In Advantage |
| :--- | :--- |
| **Dual-Endpoint Comment Extraction (`video_comments`)** | Automatically queries both `/x/v2/reply/main` and `/x/v2/reply` with WBI signing and `buvid3` fingerprinting so comment threads are extracted reliably without login. |
| **Standardized English IP Geolocation (`regionEn`)** | Automatically maps every commenter's Chinese IP location tag (`IP属地：广东`, `IP属地：上海`) into clean English province/country names (`Guangdong`, `Shanghai`, `Beijing`, `United States`). |
| **Creator Endorsement & User Level Signals** | Captures commenter account level (`authorLevel`: `0`–`6`), whether the comment is pinned to the top (`isPinned`), and whether the UP creator liked (`upLiked`) or replied (`upReplied`). |
| **Smart Keyword Auto-Discovery** | Don't have specific `BV` video IDs? Leave `videoIds` empty and enter any English or Chinese brand/topic in `searchQuery` (e.g., `Black Myth Wukong`, `Tesla`) to auto-discover videos and extract their comments. |
| **Parent Video Metrics Mode (`video_details`)** | Optionally switch `mode` to `video_details` to inspect the parent video's views, likes, coins, favorites, shares, and `coinPerViewRatio`. |
| **256 MB RAM Real-Time Streaming** | Streams comment records into your Apify Dataset in real-time batches of 10 rows on just **256 MB RAM**, supporting scheduled runs (`deltaMode`) and **JSON, CSV, Excel, or Parquet** exports. |

---

## How to Scrape Bilibili Comments in 3 Easy Steps

1. **Enter Target Videos (`videoIds`) OR a Keyword (`searchQuery`)** — Paste Bilibili `BV` IDs (e.g., `BV1Cb4TenEV6`) or full video URLs; OR leave `videoIds` empty and type a brand/topic in `searchQuery`.
2. **Set `maxResults`** — Choose the maximum number of comments to extract (`1` to `10,000`).
3. **Click "Start"** — Export your comment dataset with English IP province locations to **JSON, CSV, Excel, or Parquet**, or connect via REST API, n8n, Make, or Claude MCP.

---

## Bilibili Comments Scraper Use Cases

| Buyer Profile | Bilibili Comment & IP Geolocation Use Case |
| :--- | :--- |
| **Brand & Consumer Insights Teams** | Measure authentic Gen-Z buyer reactions, product pros/cons, and price sensitivity in the comment sections of Bilibili tech reviews and unboxings |
| **Regional Marketing & Retail Analysts** | Map where in China your audience is concentrated using `regionEn` (`Guangdong`, `Zhejiang`, `Jiangsu`, `Sichuan`, `Beijing`) to optimize regional campaigns |
| **AI / LLM Dialogue & SFT Teams** | Harvest high-upvote (`likesCount`), LV5/LV6 community replies and multi-turn discussions for Chinese LLM training |
| **Gaming Publishers & PR Desks** | Track player bug reports, patch feedback, and community sentiment under official game trailers |

---

## Input

All parameters are configured in the **Input** tab. Below is the complete input parameter reference table matching the schema and JSON input examples:

| Input Field | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **`videoIds`** | `array` | `["BV1Cb4TenEV6"]` | Target Bilibili BV IDs, av numbers, or full video URLs. Leave empty `[]` to auto-discover videos via `searchQuery`. |
| **`maxResults`** | `integer` | `100` | Maximum number of comment records to return (`1` to `10,000`). |
| **`searchQuery`** | `array` | `["Black Myth Wukong"]` | Brand, game, or topic keywords in English or Chinese used to auto-discover videos when `videoIds` is empty. |
| **`autoLocalize`** | `boolean` | `true` | Automatically translates English keywords into Simplified Chinese during video auto-discovery. |
| **`deltaMode`** | `boolean` | `false` | Remembers scraped `commentId` values across scheduled runs so you only pay for brand-new comments. |
| **`mode`** | `string` | `"video_comments"` | `"video_comments"` (default) to extract comment threads, or `"video_details"` to inspect parent video stats. |

### 1. Extract Comments & IP Provinces from Specific Videos (JSON)
```json
{
  "mode": "video_comments",
  "videoIds": [
    "BV1Cb4TenEV6",
    "https://www.bilibili.com/video/BV1r94y1H77U"
  ],
  "maxResults": 200,
  "deltaMode": false
}
```

### 2. Auto-Discover Videos by Brand Keyword & Harvest Comments (JSON)
```json
{
  "mode": "video_comments",
  "videoIds": [],
  "searchQuery": ["Black Myth Wukong"],
  "autoLocalize": true,
  "maxResults": 150
}
```

---

## Output

All scraped comments are stored in your Apify **Dataset** and can be exported to JSON, CSV, Excel, XML, or Parquet. Below is a sample JSON output matching the `"BV1Cb4TenEV6"` input above:

```json
{
  "mode": "video_comments",
  "commentId": "243557812673",
  "bvid": "BV1Cb4TenEV6",
  "aid": 113235369920154,
  "title": null,
  "text": "这首曲子就是完整的故事。00:17 到 00:25是最终boss战的leitmotif作为悲壮的一种情绪基调……顶级的音乐叙事，我的评价是：不堕轮回万古传！",
  "authorName": "墨迹鹤",
  "authorId": "1398298370",
  "authorAvatar": "https://i2.hdslb.com/bfs/face/e53805c54b0b2777003bca62c0b21fd7662cda15.jpg",
  "authorLevel": 6,
  "likesCount": 4050,
  "repliesCount": 29,
  "regionName": "上海",
  "regionEn": "Shanghai",
  "isPinned": false,
  "upLiked": true,
  "upReplied": false,
  "videoUrl": "https://www.bilibili.com/video/BV1Cb4TenEV6",
  "authorUrl": "https://space.bilibili.com/1398298370",
  "createdAtIso": "2024-10-03T08:19:56Z",
  "scrapedAtIso": "2026-10-28T10:35:00Z"
}
```

### Output Field Definitions
* **`commentId`**: Unique Bilibili comment ID (`rpid_str`, deduplicated within and across runs).
* **`bvid` / `aid`**: Parent Bilibili video BV ID and numeric archive ID.
* **`text`**: Clean comment text in Simplified Chinese.
* **`authorName` / `authorId` / `authorLevel`**: Commenter nickname, numeric MID, and Bilibili community level (`0` to `6`).
* **`likesCount` / `repliesCount`**: Number of upvotes and sub-replies on the comment.
* **`regionName` / `regionEn`**: Commenter's IP province in Chinese (`上海`) and standardized English (`"Shanghai"`).
* **`isPinned` / `upLiked` / `upReplied`**: Boolean flags indicating whether the comment was pinned, liked, or replied to by the video's creator.

---

## Automated Daily Comment Thread Monitoring (Schedules & Delta Mode)

Monitor viral Bilibili videos for new viewer comments automatically:
1. Go to **Apify Console → Schedules → Create new schedule** and select **`axon-studio/bilibili-comments-scraper`**.
2. Enable **`deltaMode: true`** in your input configuration. Previously delivered `commentId` values are saved in your Key-Value Store and skipped on future runs—so you only pay for brand-new comments.

### Python & Synchronous REST API Integration

```python
from apify_client import ApifyClient

client = ApifyClient("YOUR_APIFY_API_TOKEN")
run = client.actor("axon-studio/bilibili-comments-scraper").call(run_input={
    "mode": "video_comments",
    "videoIds": ["BV1Cb4TenEV6"],
    "maxResults": 100,
})
for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(item["commentId"], item["regionEn"], item["likesCount"], item["text"][:60])
```

```bash
curl -X POST "https://api.apify.com/v2/acts/axon-studio~bilibili-comments-scraper/run-sync-get-dataset-items?token=YOUR_APIFY_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"mode":"video_comments","videoIds":["BV1Cb4TenEV6"],"maxResults":50}'
```

---

## How Much Does It Cost to Scrape Bilibili Comments? (Pricing)

**$4.50 per 1,000 comments (`$0.0045` per comment) + `$0.02` per Actor start (Pay-Per-Event)**

* **Free Plan Credits**: Every Apify account includes **$5.00 in free monthly credits**, covering up to **1,111 Bilibili comments every month for free**.
* **Volume Discounts**: Automatic lower unit prices for Apify **Bronze / Starter**, **Silver / Scale**, and **Gold / Business** plan subscribers.
* **No Extra Server Compute Fees**: Runs on just 256 MB RAM with platform compute usage fully included.
* **Typical Run Costs**: 100 comments = **`$0.47`** · 500 comments = **`$2.27`** · 1,000 comments = **`$4.52`**.

---

## FAQ & Legal Compliance

### Do I need a Bilibili account or cookie to extract comments and IP provinces?
**No.** This Actor uses a built-in managed session pool paired with automatic `buvid3` fingerprinting and WBI request signing.

### Is it legal to scrape public Bilibili comments?
This Actor extracts publicly visible comments and IP province labels displayed on `bilibili.com` and does not access private user accounts. Under **GDPR** and **PIPL**, ensure you have a lawful basis for market research or NLP analysis. *Not affiliated with Bilibili Inc.*

---

## More Chinese Social Intelligence Scrapers by Axon Studio

* **[Bilibili Scraper 哔哩哔哩 (No Cookie) — All-in-One Videos, Danmaku, Comments & Creators](https://apify.com/axon-studio/bilibili-scraper)** — Full 8-mode Bilibili intelligence suite.
* **[Bilibili Danmaku Scraper — XML Bullet Comments & Peak Minute Extractor](https://apify.com/axon-studio/bilibili-danmaku-scraper)** — Extract time-aligned XML bullet comments (`list.so`).
* **[Bilibili Creator Videos & UP Profile Scraper](https://apify.com/axon-studio/bilibili-creator-videos-scraper)** — Extract UP creator video uploads and KOL follower profiles without `-352` blocks.
* **[Weibo Scraper 微博 (No Cookie) — Search, Posts, Comments & BlackCat](https://apify.com/axon-studio/weibo-scraper)** — 9-mode Sina Weibo & BlackCat social listening suite.
