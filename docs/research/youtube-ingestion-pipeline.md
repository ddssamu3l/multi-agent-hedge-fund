# YouTube Video Ingestion Pipeline: Technical Research

> Research date: 2026-03-21

This document covers the technical specifics of programmatically fetching new videos from YouTube channels, extracting transcripts, and assembling them into an automated morning feed for the agent system.

---

## 1. YouTube Data API v3 -- Listing Recent Videos from a Channel

### The Naive Approach: `search.list` (Expensive)

**Endpoint:**
```
GET https://www.googleapis.com/youtube/v3/search
```

**Key parameters:**

| Parameter | Required | Description |
|-----------|----------|-------------|
| `part` | Yes | Must be `snippet` |
| `key` | Yes | Your API key |
| `channelId` | No* | Restrict to a specific channel |
| `type` | No | Set to `video` to exclude channels/playlists |
| `order` | No | `date` for newest first (default: `relevance`) |
| `maxResults` | No | 0-50 (default: 5) |
| `publishedAfter` | No | RFC 3339 datetime, e.g. `2026-03-20T00:00:00Z` |
| `publishedBefore` | No | RFC 3339 datetime |
| `pageToken` | No | For pagination |

**Example request:**
```
GET https://www.googleapis.com/youtube/v3/search
  ?part=snippet
  &channelId=UCHnyfMqiRRG1u-2MsSQLbXA
  &type=video
  &order=date
  &publishedAfter=2026-03-20T00:00:00Z
  &maxResults=10
  &key=YOUR_API_KEY
```

**Response structure (abbreviated):**
```json
{
  "items": [
    {
      "id": { "videoId": "dQw4w9WgXcQ" },
      "snippet": {
        "publishedAt": "2026-03-20T14:30:00Z",
        "channelId": "UCHnyfMqiRRG1u-2MsSQLbXA",
        "title": "Video Title Here",
        "description": "First ~120 chars of description...",
        "channelTitle": "Veritasium"
      }
    }
  ],
  "nextPageToken": "CDIQAA",
  "pageInfo": { "totalResults": 42, "resultsPerPage": 10 }
}
```

**Quota cost: 100 units per call.** This is extremely expensive.

### The Better Approach: `playlistItems.list` (100x Cheaper)

Every YouTube channel has a hidden "uploads" playlist. The playlist ID follows a deterministic convention:

> **Replace the `UC` prefix in the channel ID with `UU`.**
>
> Channel ID: `UCHnyfMqiRRG1u-2MsSQLbXA`
> Uploads playlist: `UUHnyfMqiRRG1u-2MsSQLbXA`

This means you can skip calling `channels.list` entirely.

**Endpoint:**
```
GET https://www.googleapis.com/youtube/v3/playlistItems
```

**Key parameters:**

| Parameter | Required | Description |
|-----------|----------|-------------|
| `part` | Yes | `snippet` (or `snippet,contentDetails`) |
| `playlistId` | Yes | The `UU...` uploads playlist ID |
| `maxResults` | No | 0-50 (default: 5) |
| `pageToken` | No | For pagination |
| `key` | Yes | Your API key |

**Example request:**
```
GET https://www.googleapis.com/youtube/v3/playlistItems
  ?part=snippet
  &playlistId=UUHnyfMqiRRG1u-2MsSQLbXA
  &maxResults=10
  &key=YOUR_API_KEY
```

**Response structure (abbreviated):**
```json
{
  "items": [
    {
      "snippet": {
        "publishedAt": "2026-03-20T14:30:00Z",
        "title": "Video Title Here",
        "description": "Full video description text...",
        "resourceId": { "videoId": "dQw4w9WgXcQ" },
        "channelTitle": "Veritasium"
      }
    }
  ],
  "nextPageToken": "EAAaBlBUOkNESQ",
  "pageInfo": { "totalResults": 850, "resultsPerPage": 10 }
}
```

**Quota cost: 1 unit per call.** This is 100x cheaper than `search.list`.

**Limitation:** `playlistItems.list` does NOT support `publishedAfter` filtering. You must fetch the most recent items and filter by date client-side. Since videos are returned in reverse chronological order (newest first), you can stop paginating once you see a video older than your cutoff.

### Quota Budget Analysis

| Resource | Default |
|----------|---------|
| Daily quota | 10,000 units |
| Quota reset | Midnight Pacific Time |

**Cost per channel per day (playlistItems approach):**
- 1 call to `playlistItems.list` with `maxResults=50` = **1 unit**
- Most channels post 0-2 videos/day, so 1 page is sufficient

**Budget for subscribed channels:**
- 10,000 units / 1 unit per channel = **10,000 channels per day**
- Even with overhead (occasional extra pages, `videos.list` calls for richer metadata), you can comfortably monitor **hundreds of channels** without worrying about quota

**Comparison with `search.list`:**
- 10,000 units / 100 units per call = **100 calls per day total**
- For 50 channels, that is only 2 calls/day, leaving no room for anything else

### Python Example: Fetch Recent Videos via API

```python
from datetime import datetime, timedelta, timezone
import requests

API_KEY = "YOUR_API_KEY"
BASE_URL = "https://www.googleapis.com/youtube/v3/playlistItems"

def channel_to_uploads_playlist(channel_id: str) -> str:
    """Convert channel ID (UC...) to uploads playlist ID (UU...)."""
    if channel_id.startswith("UC"):
        return "UU" + channel_id[2:]
    raise ValueError(f"Unexpected channel ID format: {channel_id}")

def fetch_recent_videos(channel_id: str, hours_ago: int = 24) -> list[dict]:
    """Fetch videos published within the last `hours_ago` hours."""
    playlist_id = channel_to_uploads_playlist(channel_id)
    cutoff = datetime.now(timezone.utc) - timedelta(hours=hours_ago)

    videos = []
    page_token = None

    while True:
        params = {
            "part": "snippet",
            "playlistId": playlist_id,
            "maxResults": 50,
            "key": API_KEY,
        }
        if page_token:
            params["pageToken"] = page_token

        resp = requests.get(BASE_URL, params=params)
        resp.raise_for_status()
        data = resp.json()

        for item in data["items"]:
            published = datetime.fromisoformat(
                item["snippet"]["publishedAt"].replace("Z", "+00:00")
            )
            if published < cutoff:
                # Videos are in reverse chronological order; stop here
                return videos

            videos.append({
                "video_id": item["snippet"]["resourceId"]["videoId"],
                "title": item["snippet"]["title"],
                "description": item["snippet"]["description"],
                "published_at": published.isoformat(),
                "channel_title": item["snippet"]["channelTitle"],
            })

        page_token = data.get("nextPageToken")
        if not page_token:
            break

    return videos
```

---

## 2. YouTube RSS Feeds -- Zero-Cost Alternative

### URL Format

```
https://www.youtube.com/feeds/videos.xml?channel_id=CHANNEL_ID
```

Example:
```
https://www.youtube.com/feeds/videos.xml?channel_id=UCHnyfMqiRRG1u-2MsSQLbXA
```

### Status

**Still fully functional as of March 2026.** No authentication or API key required.

### What You Get

The feed is in Atom XML format and returns the **15 most recent videos** from the channel. Each entry contains:

```xml
<entry>
  <id>yt:video:dQw4w9WgXcQ</id>
  <yt:videoId>dQw4w9WgXcQ</yt:videoId>
  <yt:channelId>UCHnyfMqiRRG1u-2MsSQLbXA</yt:channelId>
  <title>Video Title Here</title>
  <link rel="alternate" href="https://www.youtube.com/watch?v=dQw4w9WgXcQ"/>
  <published>2026-03-20T14:30:00+00:00</published>
  <updated>2026-03-20T15:00:00+00:00</updated>
  <media:group>
    <media:title>Video Title Here</media:title>
    <media:description>Full description text...</media:description>
    <media:thumbnail url="https://i1.ytimg.com/vi/dQw4w9WgXcQ/hqdefault.jpg"
                     width="480" height="360"/>
    <media:community>
      <media:starRating count="123456" average="4.89" min="1" max="5"/>
      <media:statistics views="9876543"/>
    </media:community>
  </media:group>
</entry>
```

### Key Properties

| Property | Value |
|----------|-------|
| API key required | No |
| Cost | Free, zero quota |
| Number of entries | ~15 most recent videos |
| Includes video ID | Yes (`<yt:videoId>`) |
| Includes publish date | Yes (ISO 8601) |
| Includes description | Yes (full, via `<media:description>`) |
| Includes view count | Yes |
| Includes star rating | Yes |
| Rate limits | No documented limit, but be polite (1 req/sec) |

### Limitations

- Only the ~15 most recent videos (no pagination, no historical access)
- No filtering by date server-side (filter client-side)
- No detailed video metadata (duration, tags, category)
- Can lag a few minutes behind the actual upload time

### Python Example: Parse RSS Feed

```python
from datetime import datetime, timedelta, timezone
import feedparser  # pip install feedparser

def fetch_recent_videos_rss(channel_id: str, hours_ago: int = 24) -> list[dict]:
    """Fetch recent videos from a channel's RSS feed (free, no API key)."""
    url = f"https://www.youtube.com/feeds/videos.xml?channel_id={channel_id}"
    feed = feedparser.parse(url)
    cutoff = datetime.now(timezone.utc) - timedelta(hours=hours_ago)

    videos = []
    for entry in feed.entries:
        published = datetime.fromisoformat(entry.published)
        if published < cutoff:
            continue

        videos.append({
            "video_id": entry.yt_videoid,
            "title": entry.title,
            "description": entry.get("media_description", ""),
            "published_at": published.isoformat(),
            "link": entry.link,
            "views": entry.get("media_statistics", {}).get("views", "N/A"),
        })

    return videos
```

### RSS vs. API Comparison

| Factor | RSS Feed | Data API (playlistItems) |
|--------|----------|--------------------------|
| Cost | Free | 1 unit/call (10k units/day) |
| API key needed | No | Yes |
| Videos returned | ~15 most recent | Up to 50/page, paginated |
| Date filtering | Client-side | Client-side |
| Rich metadata | Limited | Full |
| Reliability | Undocumented, could break | Official, SLA-backed |
| Rate limits | Unofficial | Official quota system |

**Recommendation:** Use RSS as the primary discovery mechanism. Fall back to the API only if you need historical depth (>15 videos) or richer metadata.

---

## 3. `youtube-transcript-api` (Python Library)

### Overview

A Python library that extracts transcripts/subtitles directly from YouTube without requiring an API key or browser automation. It works by hitting the same internal endpoints that the YouTube web player uses.

- **PyPI:** https://pypi.org/project/youtube-transcript-api/
- **GitHub:** https://github.com/jdepoix/youtube-transcript-api
- **Latest version:** 1.2.4 (January 2026)
- **License:** MIT

### Installation

```bash
pip install youtube-transcript-api
```

### Basic Usage

```python
from youtube_transcript_api import YouTubeTranscriptApi

ytt_api = YouTubeTranscriptApi()

# Fetch transcript (pass VIDEO ID, not URL)
transcript = ytt_api.fetch("dQw4w9WgXcQ")

for entry in transcript:
    print(f"[{entry.start:.1f}s] {entry.text}")
```

### Output Format

Each transcript is a list of `FetchedTranscriptSnippet` objects with three fields:

```python
{
    "text": "Hey there",       # The spoken text
    "start": 0.0,              # Start time in seconds (float)
    "duration": 1.54           # Duration in seconds (float)
}
```

Access raw data as list of dicts via `transcript.to_raw_data()`.

### Auto-Generated Captions

**Fully supported.** The library works with both manual and auto-generated captions. You can distinguish between them:

```python
transcript_list = ytt_api.list("dQw4w9WgXcQ")

# Find auto-generated transcript
transcript = transcript_list.find_generated_transcript(["en"])
fetched = transcript.fetch()

# Find manually created transcript
transcript = transcript_list.find_manually_created_transcript(["en"])
fetched = transcript.fetch()
```

### Language Priority

```python
# Try English first, fall back to Spanish, then French
transcript = ytt_api.fetch("dQw4w9WgXcQ", languages=["en", "es", "fr"])
```

### Translation

```python
transcript_list = ytt_api.list("dQw4w9WgXcQ")
transcript = transcript_list.find_transcript(["ja"])
translated = transcript.translate("en")
result = translated.fetch()
```

### Output Formatters

```python
from youtube_transcript_api.formatters import (
    JSONFormatter,
    TextFormatter,
    WebVTTFormatter,
    SRTFormatter,
)

formatter = TextFormatter()
plain_text = formatter.format_transcript(transcript)
```

### Error Types

| Exception | When |
|-----------|------|
| `TranscriptsDisabled` | Video has subtitles turned off |
| `NoTranscriptFound` | No transcript in requested language(s) |
| `VideoUnavailable` | Video is private, deleted, or region-locked |
| `RequestBlocked` | YouTube blocked the request |
| `IpBlocked` | YouTube blocked the IP (common with cloud providers) |
| `NotTranslatable` | Transcript cannot be translated to requested language |
| `TranslationLanguageNotAvailable` | Target language not available |

### Rate Limits and IP Blocking

- **No official rate limit**, but YouTube throttles heavy usage
- Practical limit: ~100-250 requests/minute before errors start appearing
- **Cloud provider IPs are frequently blocked** -- this is the biggest operational concern
- Mitigation: residential proxies via `WebshareProxyConfig` integration, or run on a residential IP

```python
from youtube_transcript_api import YouTubeTranscriptApi
from youtube_transcript_api.proxies import WebshareProxyConfig

ytt_api = YouTubeTranscriptApi(
    proxy_config=WebshareProxyConfig(
        proxy_username="user",
        proxy_password="pass",
    )
)
```

### Known Limitations

| Video Type | Works? | Notes |
|------------|--------|-------|
| Standard videos with captions | Yes | Both manual and auto-generated |
| Videos without any captions | No | Raises `TranscriptsDisabled` |
| Age-restricted videos | No | Cookie auth is currently broken |
| Members-only videos | No | Requires authentication not available |
| Live streams (completed) | Partial | Works if YouTube generated captions post-stream |
| Live streams (ongoing) | No | No real-time transcript access |
| YouTube Shorts | Yes | If captions exist (many Shorts lack them) |
| Private/unlisted | No | Raises `VideoUnavailable` |

---

## 4. `yt-dlp` as Alternative Transcript Extraction

### Overview

`yt-dlp` is a feature-rich command-line video downloader (fork of `youtube-dl`). It can extract subtitles without downloading the video itself.

### Installation

```bash
pip install yt-dlp
# or
brew install yt-dlp
```

### Extract Subtitles Without Downloading Video

**List available subtitles:**
```bash
yt-dlp --list-subs "https://www.youtube.com/watch?v=dQw4w9WgXcQ"
```

**Download auto-generated subtitles only:**
```bash
yt-dlp \
  --write-auto-subs \
  --sub-langs en \
  --sub-format vtt \
  --skip-download \
  --output "%(id)s" \
  "https://www.youtube.com/watch?v=dQw4w9WgXcQ"
```
This creates a file like `dQw4w9WgXcQ.en.vtt`.

**Download manual subtitles (preferred when available):**
```bash
yt-dlp \
  --write-subs \
  --sub-langs en \
  --sub-format vtt \
  --skip-download \
  --output "%(id)s" \
  "https://www.youtube.com/watch?v=dQw4w9WgXcQ"
```

**Download both (manual preferred, auto-generated as fallback):**
```bash
yt-dlp \
  --write-subs \
  --write-auto-subs \
  --sub-langs en \
  --sub-format vtt \
  --skip-download \
  --output "%(id)s" \
  "https://www.youtube.com/watch?v=dQw4w9WgXcQ"
```

### Converting VTT to Plain Text (Python)

```python
import re

def vtt_to_text(vtt_content: str) -> str:
    """Convert WebVTT subtitle content to plain text."""
    lines = vtt_content.strip().split("\n")
    text_lines = []
    seen = set()

    for line in lines:
        # Skip headers, timestamps, and empty lines
        if line.startswith("WEBVTT") or line.startswith("Kind:") or line.startswith("Language:"):
            continue
        if "-->" in line:
            continue
        if not line.strip():
            continue
        # Remove HTML tags
        clean = re.sub(r"<[^>]+>", "", line).strip()
        if clean and clean not in seen:
            seen.add(clean)
            text_lines.append(clean)

    return "\n".join(text_lines)
```

### Using yt-dlp Programmatically in Python

```python
import subprocess
import json
import tempfile
import os

def extract_transcript_ytdlp(video_id: str) -> str | None:
    """Extract transcript using yt-dlp without downloading video."""
    url = f"https://www.youtube.com/watch?v={video_id}"

    with tempfile.TemporaryDirectory() as tmpdir:
        output_template = os.path.join(tmpdir, "%(id)s")

        result = subprocess.run([
            "yt-dlp",
            "--write-subs",
            "--write-auto-subs",
            "--sub-langs", "en",
            "--sub-format", "vtt",
            "--skip-download",
            "--output", output_template,
            url,
        ], capture_output=True, text=True, timeout=30)

        # Look for the subtitle file
        for fname in os.listdir(tmpdir):
            if fname.endswith(".vtt"):
                with open(os.path.join(tmpdir, fname)) as f:
                    return vtt_to_text(f.read())

    return None
```

### yt-dlp vs. youtube-transcript-api Comparison

| Factor | youtube-transcript-api | yt-dlp |
|--------|----------------------|--------|
| Python-native | Yes (library) | No (subprocess/CLI) |
| API key needed | No | No |
| Auto-generated captions | Yes | Yes |
| Output format | Structured (text + timestamps) | VTT/SRT file (needs parsing) |
| Speed | Fast (single HTTP request) | Slower (spawns process, writes file) |
| IP blocking risk | High on cloud | High on cloud |
| Error handling | Clean Python exceptions | Exit codes + stderr parsing |
| Maintained | Yes, active | Yes, very active |
| Batch processing | Native | Via playlist URLs or xargs |

**Recommendation:** Use `youtube-transcript-api` as primary (cleaner API, structured output). Use `yt-dlp` as fallback when the library fails on specific videos.

---

## 5. Complete Pipeline Design

### Architecture Overview

```
 +------------------+     +-------------------+     +--------------------+
 | Channel Registry |---->| Video Discovery   |---->| Transcript Extract |
 | (channel IDs)    |     | (RSS + API)       |     | (transcript-api)  |
 +------------------+     +-------------------+     +--------------------+
                                                             |
                                                             v
                                                    +--------------------+
                                                    | LLM Summarization  |
                                                    | (cheap model)      |
                                                    +--------------------+
                                                             |
                                                             v
                                                    +--------------------+
                                                    | Morning Feed       |
                                                    | (structured output)|
                                                    +--------------------+
```

### Step 1: Channel Registry

```python
# config/youtube_channels.py

SUBSCRIBED_CHANNELS = [
    {
        "channel_id": "UCHnyfMqiRRG1u-2MsSQLbXA",
        "name": "Veritasium",
        "category": "science",
    },
    {
        "channel_id": "UCvjgXvBlCQOV7HDRT_L8HYQ",
        "name": "Bloomberg Television",
        "category": "finance",
    },
    {
        "channel_id": "UCEAZeUIeJs0IjQiqTCdVSIg",
        "name": "Bloomberg Podcasts",
        "category": "finance",
    },
    # ... more channels
]
```

### Step 2: Video Discovery (RSS-First, API Fallback)

```python
# pipeline/video_discovery.py

from datetime import datetime, timedelta, timezone
from dataclasses import dataclass
import feedparser
import requests
import logging

logger = logging.getLogger(__name__)

@dataclass
class VideoInfo:
    video_id: str
    title: str
    description: str
    published_at: datetime
    channel_id: str
    channel_name: str
    category: str

def discover_via_rss(channel: dict, cutoff: datetime) -> list[VideoInfo]:
    """Free, no API key needed. Returns up to ~15 most recent videos."""
    url = f"https://www.youtube.com/feeds/videos.xml?channel_id={channel['channel_id']}"

    try:
        feed = feedparser.parse(url)
        if feed.bozo:  # Parse error
            logger.warning(f"RSS parse error for {channel['name']}: {feed.bozo_exception}")
            return []
    except Exception as e:
        logger.error(f"RSS fetch failed for {channel['name']}: {e}")
        return []

    videos = []
    for entry in feed.entries:
        try:
            published = datetime.fromisoformat(entry.published)
        except (ValueError, AttributeError):
            continue

        if published < cutoff:
            continue

        videos.append(VideoInfo(
            video_id=entry.yt_videoid,
            title=entry.title,
            description=getattr(entry, "media_description", ""),
            published_at=published,
            channel_id=channel["channel_id"],
            channel_name=channel["name"],
            category=channel.get("category", "general"),
        ))

    return videos

def discover_via_api(channel: dict, cutoff: datetime, api_key: str) -> list[VideoInfo]:
    """Fallback using YouTube Data API. Costs 1 quota unit per page."""
    playlist_id = "UU" + channel["channel_id"][2:]

    videos = []
    page_token = None

    while True:
        params = {
            "part": "snippet",
            "playlistId": playlist_id,
            "maxResults": 50,
            "key": api_key,
        }
        if page_token:
            params["pageToken"] = page_token

        resp = requests.get(
            "https://www.googleapis.com/youtube/v3/playlistItems",
            params=params,
            timeout=10,
        )
        resp.raise_for_status()
        data = resp.json()

        for item in data.get("items", []):
            snippet = item["snippet"]
            published = datetime.fromisoformat(
                snippet["publishedAt"].replace("Z", "+00:00")
            )

            if published < cutoff:
                return videos  # Newest-first ordering, safe to stop

            videos.append(VideoInfo(
                video_id=snippet["resourceId"]["videoId"],
                title=snippet["title"],
                description=snippet.get("description", ""),
                published_at=published,
                channel_id=channel["channel_id"],
                channel_name=channel["name"],
                category=channel.get("category", "general"),
            ))

        page_token = data.get("nextPageToken")
        if not page_token:
            break

    return videos

def discover_new_videos(
    channels: list[dict],
    hours_ago: int = 24,
    api_key: str | None = None,
) -> list[VideoInfo]:
    """Discover new videos across all subscribed channels.

    Strategy: RSS first (free), API fallback if RSS fails.
    """
    cutoff = datetime.now(timezone.utc) - timedelta(hours=hours_ago)
    all_videos = []

    for channel in channels:
        # Try RSS first (free)
        videos = discover_via_rss(channel, cutoff)

        # Fallback to API if RSS returned nothing and we have a key
        if not videos and api_key:
            logger.info(f"RSS empty for {channel['name']}, trying API...")
            try:
                videos = discover_via_api(channel, cutoff, api_key)
            except Exception as e:
                logger.error(f"API failed for {channel['name']}: {e}")

        all_videos.extend(videos)
        logger.info(f"{channel['name']}: {len(videos)} new videos")

    # Sort by publish date, newest first
    all_videos.sort(key=lambda v: v.published_at, reverse=True)
    return all_videos
```

### Step 3: Transcript Extraction (with Fallback Chain)

```python
# pipeline/transcript_extraction.py

import time
import logging
from youtube_transcript_api import YouTubeTranscriptApi
from youtube_transcript_api.formatters import TextFormatter

logger = logging.getLogger(__name__)

ytt_api = YouTubeTranscriptApi()
text_formatter = TextFormatter()

def extract_transcript(video_id: str) -> str | None:
    """Extract transcript with fallback chain:
    1. youtube-transcript-api (manual captions)
    2. youtube-transcript-api (auto-generated captions)
    3. yt-dlp (as last resort)
    """
    # Attempt 1: youtube-transcript-api
    try:
        transcript_list = ytt_api.list(video_id)

        # Prefer manual captions
        try:
            transcript = transcript_list.find_manually_created_transcript(["en"])
            fetched = transcript.fetch()
            return text_formatter.format_transcript(fetched)
        except Exception:
            pass

        # Fall back to auto-generated
        try:
            transcript = transcript_list.find_generated_transcript(["en"])
            fetched = transcript.fetch()
            return text_formatter.format_transcript(fetched)
        except Exception:
            pass

        # Try any language and translate to English
        for t in transcript_list:
            try:
                translated = t.translate("en")
                fetched = translated.fetch()
                return text_formatter.format_transcript(fetched)
            except Exception:
                continue

    except Exception as e:
        logger.warning(f"youtube-transcript-api failed for {video_id}: {e}")

    # Attempt 2: yt-dlp fallback
    try:
        return _extract_via_ytdlp(video_id)
    except Exception as e:
        logger.warning(f"yt-dlp also failed for {video_id}: {e}")

    return None

def _extract_via_ytdlp(video_id: str) -> str | None:
    """Fallback: use yt-dlp to extract subtitles."""
    import subprocess, tempfile, os, re

    url = f"https://www.youtube.com/watch?v={video_id}"

    with tempfile.TemporaryDirectory() as tmpdir:
        output_template = os.path.join(tmpdir, "%(id)s")
        result = subprocess.run(
            [
                "yt-dlp",
                "--write-subs", "--write-auto-subs",
                "--sub-langs", "en",
                "--sub-format", "vtt",
                "--skip-download",
                "--output", output_template,
                url,
            ],
            capture_output=True, text=True, timeout=30,
        )

        for fname in os.listdir(tmpdir):
            if fname.endswith(".vtt"):
                with open(os.path.join(tmpdir, fname)) as f:
                    return _vtt_to_text(f.read())

    return None

def _vtt_to_text(vtt_content: str) -> str:
    """Strip VTT formatting to plain text."""
    import re
    lines = vtt_content.strip().split("\n")
    text_lines = []
    seen = set()

    for line in lines:
        if line.startswith("WEBVTT") or line.startswith("Kind:") or line.startswith("Language:"):
            continue
        if "-->" in line or not line.strip():
            continue
        clean = re.sub(r"<[^>]+>", "", line).strip()
        if clean and clean not in seen:
            seen.add(clean)
            text_lines.append(clean)

    return "\n".join(text_lines)

def extract_transcripts_batch(
    video_ids: list[str],
    delay: float = 1.0,
) -> dict[str, str | None]:
    """Extract transcripts for multiple videos with rate limiting."""
    results = {}
    for i, vid in enumerate(video_ids):
        logger.info(f"Extracting transcript {i+1}/{len(video_ids)}: {vid}")
        results[vid] = extract_transcript(vid)
        if i < len(video_ids) - 1:
            time.sleep(delay)  # Respect rate limits
    return results
```

### Step 4: LLM Summarization

```python
# pipeline/summarization.py

from openai import OpenAI

# Use a cheap, fast model for summarization
client = OpenAI()  # or any compatible provider

SYSTEM_PROMPT = """You are a research analyst summarizing YouTube video transcripts
for a hedge fund's morning briefing. Focus on:
- Key claims, data points, and numbers mentioned
- Market-relevant insights or predictions
- Novel frameworks or mental models presented
- Any actionable information

Be concise. Use bullet points. Skip filler content."""

def summarize_transcript(
    title: str,
    channel: str,
    transcript: str,
    model: str = "gpt-4o-mini",  # cheap and fast
    max_transcript_chars: int = 30_000,
) -> str:
    """Summarize a single video transcript."""
    # Truncate very long transcripts to control cost
    if len(transcript) > max_transcript_chars:
        transcript = transcript[:max_transcript_chars] + "\n\n[...transcript truncated...]"

    response = client.chat.completions.create(
        model=model,
        messages=[
            {"role": "system", "content": SYSTEM_PROMPT},
            {
                "role": "user",
                "content": f"**Video:** {title}\n**Channel:** {channel}\n\n"
                           f"**Transcript:**\n{transcript}",
            },
        ],
        max_tokens=500,
        temperature=0.3,
    )

    return response.choices[0].message.content
```

### Step 5: Morning Feed Assembly

```python
# pipeline/morning_feed.py

import logging
from datetime import datetime, timezone
from dataclasses import dataclass

from config.youtube_channels import SUBSCRIBED_CHANNELS
from pipeline.video_discovery import discover_new_videos, VideoInfo
from pipeline.transcript_extraction import extract_transcripts_batch
from pipeline.summarization import summarize_transcript

logger = logging.getLogger(__name__)

@dataclass
class FeedItem:
    video: VideoInfo
    transcript: str | None
    summary: str | None

def generate_morning_feed(
    hours_ago: int = 24,
    api_key: str | None = None,
) -> list[FeedItem]:
    """Full pipeline: discover -> transcribe -> summarize."""

    # Step 1: Discover new videos
    logger.info("Discovering new videos...")
    videos = discover_new_videos(SUBSCRIBED_CHANNELS, hours_ago, api_key)
    logger.info(f"Found {len(videos)} new videos")

    if not videos:
        return []

    # Step 2: Extract transcripts
    logger.info("Extracting transcripts...")
    video_ids = [v.video_id for v in videos]
    transcripts = extract_transcripts_batch(video_ids, delay=1.0)

    # Step 3: Summarize
    logger.info("Summarizing...")
    feed_items = []
    for video in videos:
        transcript = transcripts.get(video.video_id)
        summary = None

        if transcript:
            try:
                summary = summarize_transcript(
                    title=video.title,
                    channel=video.channel_name,
                    transcript=transcript,
                )
            except Exception as e:
                logger.error(f"Summarization failed for {video.video_id}: {e}")

        feed_items.append(FeedItem(
            video=video,
            transcript=transcript,
            summary=summary,
        ))

    return feed_items

def format_feed_as_text(feed_items: list[FeedItem]) -> str:
    """Format the feed as a readable text briefing."""
    now = datetime.now(timezone.utc).strftime("%Y-%m-%d %H:%M UTC")
    lines = [
        f"# YouTube Morning Briefing -- {now}",
        f"## {len(feed_items)} new videos from subscribed channels\n",
    ]

    for i, item in enumerate(feed_items, 1):
        status = "Summarized" if item.summary else (
            "Transcript only" if item.transcript else "No transcript"
        )
        lines.append(f"### {i}. [{item.video.category.upper()}] {item.video.title}")
        lines.append(f"**Channel:** {item.video.channel_name}")
        lines.append(f"**Published:** {item.video.published_at.strftime('%Y-%m-%d %H:%M UTC')}")
        lines.append(f"**Link:** https://www.youtube.com/watch?v={item.video.video_id}")
        lines.append(f"**Status:** {status}\n")

        if item.summary:
            lines.append(item.summary)
        elif item.transcript:
            # Show first 500 chars of transcript as preview
            preview = item.transcript[:500] + "..." if len(item.transcript) > 500 else item.transcript
            lines.append(f"*Transcript preview:* {preview}")
        else:
            lines.append("*No transcript available for this video.*")

        lines.append("\n---\n")

    return "\n".join(lines)

# Entry point for scheduled execution
if __name__ == "__main__":
    import os
    logging.basicConfig(level=logging.INFO)

    feed = generate_morning_feed(
        hours_ago=24,
        api_key=os.environ.get("YOUTUBE_API_KEY"),
    )
    print(format_feed_as_text(feed))
```

### Cost Analysis

**Per-day costs assuming 50 subscribed channels, ~20 new videos/day:**

| Component | Cost |
|-----------|------|
| Video discovery (RSS) | Free |
| Video discovery (API fallback) | ~5 quota units (rare) |
| Transcript extraction | Free (no API key) |
| LLM summarization (gpt-4o-mini) | ~$0.02 (20 videos x ~4K tokens input x $0.15/1M + ~500 tokens output x $0.60/1M) |
| **Total daily cost** | **~$0.02/day** |

### Scheduling

Run via cron, systemd timer, or a task scheduler:

```bash
# crontab: run at 6:00 AM local time every day
0 6 * * * cd /path/to/project && python -m pipeline.morning_feed >> /var/log/youtube-feed.log 2>&1
```

Or integrate into the agent system as a tool that the agent can invoke on demand.

### Error Handling and Resilience

1. **RSS failure** -- falls back to API automatically
2. **API quota exhausted** -- log warning, skip remaining channels, use cached results
3. **Transcript extraction failure** -- log the video, include it in feed without summary (title + description still useful)
4. **IP blocking** -- rotate proxies, or schedule extraction from a residential IP machine
5. **LLM failure** -- include raw transcript snippet instead of summary
6. **Deduplication** -- maintain a SQLite or JSON file of previously seen video IDs to avoid reprocessing

### Dependencies

```
# requirements.txt (add to existing)
feedparser>=6.0
youtube-transcript-api>=1.2.0
yt-dlp>=2024.01.01
requests>=2.31
openai>=1.0
```
