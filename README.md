# 🕸️ WebVault — AI-Powered Web Scraper & Data Analyzer

<div align="center">

![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![PyQt5](https://img.shields.io/badge/PyQt5-41CD52?style=for-the-badge&logo=qt&logoColor=white)
![OpenRouter](https://img.shields.io/badge/OpenRouter-LLM-7C3AED?style=for-the-badge&logo=openai&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)
![Version](https://img.shields.io/badge/Version-2.0-blue?style=for-the-badge)

**A full-featured desktop web scraper with a modern PyQt5 GUI, multi-threaded crawling, AI-powered content analysis via OpenRouter LLM, interactive sitemap visualization, and one-click JSON/XML export.**

[Features](#-features) • [Screenshots](#-interface-overview) • [Installation](#-installation) • [Usage](#-usage) • [Architecture](#-architecture) • [Contributing](#-contributing)

</div>

---

## 🌟 Overview

WebVault is a professional-grade desktop web scraping application that combines multi-threaded crawling, intelligent HTML parsing, and LLM-powered analysis into a single polished PyQt5 interface. Crawl any website to any depth, explore the resulting sitemap as an interactive tree, preview scraped pages visually, and chat with an AI assistant about your data — all without leaving the app.

---

## ✨ Features

### 🕷️ Web Crawling Engine
- **Multi-threaded crawling** — up to 10 concurrent threads via `ThreadPoolExecutor`
- **Unlimited depth & pages** — no artificial page limits
- **Smart URL normalization** — handles `www.` prefixes, fragments, trailing slashes
- **Duplicate detection** — thread-safe visited URL tracking with a lock
- **Graceful stop** — interrupt crawling mid-session without data loss
- **Rate limiting** — 100ms delay per request to avoid server overload
- **Desktop notifications** — system tray alerts on completion or error

### 🖥️ Modern PyQt5 GUI
- Dark navy theme with clean card-based layout
- **5 tabbed panels**: Web Scraping, Data View, Analysis, Sitemap, AI Analysis
- Real-time progress updates during crawling
- Color-coded status bar (blue → green → red)
- Responsive splitter layout for content/visual preview

### 📊 Data View & Filtering
- Sortable table with Title, URL, Depth, Links, Words, Load Time
- Live search + domain dropdown filter
- Double-click any row to preview content in text or HTML view
- Visual HTML rendering via `PyQtWebEngine` (optional)

### 🗺️ Sitemap Visualization
- Interactive tree widget showing parent-child page relationships
- Icons, tooltips with metadata (depth, link count)
- Double-click to open any URL in the system browser
- Export sitemap as `sitemap.xml` or hierarchical `sitemap_tree.json`

### 🤖 AI Analysis Chat
- Chat interface powered by **OpenRouter** (DeepSeek R1 model)
- Bubble-style chat UI with Markdown rendering
- Quick-action buttons: structure analysis, SEO suggestions, theme detection, performance comparison
- Context-aware — AI receives scraped page summaries automatically
- Falls back gracefully when API is unavailable

### 📤 Export Options
- `scraped_data.json` — full structured export with metadata
- `sitemap.xml` — standard XML sitemap format
- `sitemap_tree.json` — hierarchical JSON tree
- `chat_history_*.json` — auto-saved on app close

---

## 🖼️ Interface Overview

```
┌────────────────────────────────────────────────────────────┐
│  🕸️ WebVault — Web Scraper & Data Analyzer          v2.0  │
├──────────┬──────────┬──────────┬──────────┬───────────────┤
│  Scraping │ Data View│ Analysis │  Sitemap │  AI Analysis  │
├──────────┴──────────┴──────────┴──────────┴───────────────┤
│                                                             │
│  [URL Input]              [Depth: 3]   [Start] [Stop]      │
│                                                             │
│  Progress: ████████████░░░░ Scraped: /about (depth 2)      │
│                                                             │
│  Results Log:                                               │
│  ✓ Scraped: Home (depth 0)                                 │
│  ✓ Scraped: About Us (depth 1)                             │
│  ✓ Scraped: Contact (depth 2)                              │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Installation

### Prerequisites
- Python 3.8+
- Google Chrome (for optional HTML preview)
- OpenRouter API key (for AI analysis)

### Setup

```bash
# Clone the repository
git clone https://github.com/yourusername/webvault-ai-scraper.git
cd webvault-ai-scraper

# Create virtual environment
python3 -m venv venv
source venv/bin/activate        # Linux/macOS
venv\Scripts\activate           # Windows

# Install dependencies
pip install -r requirements.txt
```

### Dependencies

```txt
PyQt5
PyQtWebEngine       # Optional - for HTML visual preview
openai              # For AI chat via OpenRouter
python-dotenv
markdown
```

### Environment Setup

Create a `.env` file in the project root:

```env
OPENROUTER_API_KEY=your_openrouter_api_key_here
```

> ⚠️ Never hardcode API keys in source files. Always use `.env` and add it to `.gitignore`.

Get your free API key at [openrouter.ai](https://openrouter.ai).

---

## 🚀 Usage

### Launch the App

```bash
python web_scraper_app.py
```

### Step-by-Step

**1. Scraping Tab**
- Enter a target URL (e.g. `https://example.com`)
- Set max crawl depth (1–100)
- Click **Start Scraping** — progress updates in real time
- Click **Stop** to gracefully interrupt at any point

**2. Data View Tab**
- Browse all scraped pages in the table
- Use the search bar to filter by title/URL/content
- Filter by domain using the dropdown
- Double-click any row to preview in the Analysis tab

**3. Analysis Tab**
- View aggregate statistics (pages, words, links, load times)
- See domain breakdown
- Preview text content and rendered HTML side-by-side

**4. Sitemap Tab**
- Explore the crawled site as an interactive tree
- Hover over nodes for metadata tooltips
- Double-click any node to open the URL in your browser
- Export as `sitemap.xml` or `sitemap_tree.json`

**5. AI Analysis Tab**
- Ask questions about your scraped data in plain English
- Use quick-action buttons for instant insights
- AI context is automatically populated with scraped page summaries

---

## 🏗️ Architecture

```
webvault-ai-scraper/
│
├── web_scraper_app.py          # Main PyQt5 application
│   ├── ScrapingThread          # QThread wrapper for non-blocking crawl
│   ├── ModernButton            # Custom styled QPushButton
│   ├── ChatBubbleWidget        # Markdown-rendered chat message widget
│   └── WebScraperApp           # Main QMainWindow with 5 tabs
│
├── module.py                   # Core scraping engine (no dependencies)
│   ├── HTMLParser              # Custom html.parser subclass
│   ├── Website                 # Data class: title, URL, content, links
│   └── WebScraper              # Multi-threaded crawler with URL dedup
│
├── test_scraper.py             # Test suite for core functionality
├── .env                        # API keys (gitignored)
└── requirements.txt
```

### Crawling Pipeline

```
Start URL
    ↓
Normalize URL (strip www., fragments, trailing slashes)
    ↓
Thread Pool (10 workers) → Fetch HTML → Parse with HTMLParser
    ↓
Extract links → Normalize → Deduplicate → Add to next depth queue
    ↓
Repeat until max_depth or no new URLs
    ↓
Emit results → Update GUI → Save to websites[]
```

### AI Analysis Pipeline

```
User question + scraped page summaries
    ↓
OpenRouter API (DeepSeek R1 model)
    ↓
Markdown-rendered response in chat bubble UI
```

---

## ⚙️ Configuration

| Setting | Default | Description |
|---|---|---|
| Max Depth | 3 | How many link levels deep to crawl |
| Max Workers | 10 | Concurrent scraping threads |
| Request Timeout | 15s | Per-page fetch timeout |
| Rate Limit Delay | 100ms | Delay between requests |
| AI Model | `deepseek-r1-0528-qwen3-8b:free` | OpenRouter model |
| AI Max Tokens | 2000 | Max response length |

---

## 🧪 Running Tests

```bash
python test_scraper.py
```

Tests cover:
- HTML parser extraction (title, links, text)
- `Website` class methods (word count, domain, search)
- URL normalization (`www.` stripping, fragment removal)
- Domain filtering with normalized domains

---

## 📈 Roadmap

- [ ] Proxy rotation support for large-scale crawling
- [ ] Login/session handling for authenticated pages
- [ ] Custom CSS selector targeting for structured extraction
- [ ] PostgreSQL backend for persistent scrape history
- [ ] Scheduled / recurring crawls
- [ ] CSV export alongside JSON
- [ ] Playwright integration for JavaScript-heavy sites
- [ ] Docker containerization

---

## 🤝 Contributing

```bash
# Fork and clone
git clone https://github.com/yourusername/webvault-ai-scraper.git

# Create feature branch
git checkout -b feature/your-feature

# Commit and push
git commit -m "Add: your feature"
git push origin feature/your-feature
```

Please ensure no API keys are present in any commits and follow PEP 8 for Python code.

---

## 📄 License

MIT License — see [LICENSE](LICENSE) for details.

---

## 🙌 Acknowledgements

- [PyQt5](https://riverbankcomputing.com/software/pyqt/) — desktop GUI framework
- [OpenRouter](https://openrouter.ai/) — unified LLM API gateway
- [DeepSeek](https://deepseek.com/) — AI model for content analysis
- Python standard library — `urllib`, `html.parser`, `concurrent.futures`

---

<div align="center">
  <b>Crawl the web. Understand it. Export it. 🕸️</b>
</div>
