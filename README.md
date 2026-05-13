# Boss.vn

A intelligent web scraper that autonomously collects resources and learns from web content. Boss.vn extracts images, scripts, and text data from websites, processes the information, and stores it for self-learning purposes.

## Features

🤖 **Autonomous Web Scraping** - Automatically fetches and processes web content
📚 **Resource Extraction** - Identifies and collects images and assets from web pages
🧠 **Self-Learning System** - Analyzes text content and extracts meaningful keywords for learning
💾 **Persistent Storage** - Saves all collected data to a local "brain" directory
🛡️ **Error Resilience** - Gracefully handles unreachable or broken websites
🌐 **URL Processing** - Filters and validates resources to ensure quality data collection

## Installation

```bash
# Clone the repository
git clone https://github.com/gfk14oi-rgb/Boss.vn.git
cd Boss.vn

# Install dependencies
npm install
```

## Dependencies

```json
{
  "axios": "^0.27.0",
  "cheerio": "^1.0.0-rc.10"
}
```

Install with:
```bash
npm install axios cheerio
```

## Usage

### Basic Example

```javascript
const axios = require('axios');
const cheerio = require('cheerio');
const fs = require('fs');

async function bossScavenge(url) {
    try {
        const res = await axios.get(url);
        const $ = cheerio.load(res.data);
        
        // Nhặt tài nguyên ảnh để làm tài nguyên (Resources)
        let resources = [];
        $('img').each((i, el) => {
            let src = $(el).attr('src');
            if(src && src.includes('http')) resources.push(src);
        });

        // Nhặt văn bản để tự huấn luyện (Self-learning)
        let rawText = $('p').text();
        let learnedKeywords = rawText.split(' ').filter(word => word.length > 7).slice(0, 20);

        const brainOutput = {
            source: url,
            timestamp: new Date().toLocaleString(),
            foundResources: resources,
            learnedKeywords: learnedKeywords,
            status: "Sử dụng được"
        };

        // Lưu vào "kho" dữ liệu
        if (!fs.existsSync('./brain')) fs.mkdirSync('./brain');
        fs.writeFileSync(`./brain/site_${Date.now()}.json`, JSON.stringify(brainOutput, null, 2));
        
        console.log("Boss.vn đã nạp thêm dữ liệu mới!");
    } catch (err) {
        console.log("Trang web lỗi, Boss.vn đang tìm mục tiêu khác...");
    }
}

// Chạy thử với một trang lưu trữ
bossScavenge('https://example.com');
```

## How It Works

1. **Fetch** - Uses axios to retrieve the HTML content from the target URL
2. **Parse** - Cheerio loads and parses the HTML DOM structure
3. **Extract Resources** - Scans for `<img>` tags and collects all HTTP URLs
4. **Extract Knowledge** - Analyzes paragraph text and filters keywords (words longer than 7 characters)
5. **Process Data** - Combines all extracted information into a structured JSON object
6. **Store** - Saves the processed data to `./brain/site_[timestamp].json`
7. **Report** - Logs success or failure messages in Vietnamese

## Output Format

Each scraped page generates a JSON file in the `./brain/` directory:

```json
{
  "source": "https://example.com",
  "timestamp": "5/13/2026, 10:30:45 AM",
  "foundResources": [
    "https://example.com/image1.jpg",
    "https://example.com/image2.png"
  ],
  "learnedKeywords": [
    "intelligent",
    "processing",
    "technology",
    "autonomous"
  ],
  "status": "Sử dụng được"
}
```

## Directory Structure

```
Boss.vn/
├── README.md
├── package.json
├── index.js (or your main script)
└── brain/                    # Auto-created directory
    ├── site_1715592645123.json
    ├── site_1715592645456.json
    └── ...
```

The `brain/` directory stores all learned data from scraped websites.

## Error Handling

- **Network Errors** - Gracefully catches timeout and connection failures
- **Invalid URLs** - Filters resources that don't contain 'http' protocol
- **Missing Elements** - Safely handles pages without images or paragraphs
- **File System** - Automatically creates `brain/` directory if it doesn't exist

## Vietnamese Comments Guide

- **Nhặt tài nguyên ảnh** = Extract image resources
- **Tự huấn luyện** = Self-learning
- **Kho dữ liệu** = Data warehouse
- **Nạp** = Load/Ingest
- **Sử dụng được** = Usable/Functional

## Configuration

You can customize the scraper by modifying:

- **Resource Filter**: Change `word.length > 7` to adjust keyword minimum length
- **Keyword Limit**: Modify `.slice(0, 20)` to collect more/fewer keywords
- **URL Validation**: Update `src.includes('http')` for different URL schemes
- **Storage Path**: Change `./brain/` to any desired directory

## License

MIT

## Author

gfk14oi-rgb
