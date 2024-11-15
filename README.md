This snippet is cloned from the https://github.com/NikolaiT/struktur repo for sake of exploring and extending.

# Struktur.js

A JavaScript library for detecting and extracting structured content from HTML pages.

## Description

Struktur.js is a powerful tool that analyzes rendered HTML pages to identify and extract structured content based on visual alignment and semantic patterns. It's particularly useful for web scraping, content analysis, and automated data extraction.

## Features

- Detects structurally similar HTML elements based on visual alignment
- Extracts three types of content:
  - Links with text
  - Images with sources
  - Visible text nodes
- Configurable structure detection parameters
- Optional visual highlighting of detected structures
- MIT licensed

## Installation

```javascript
// Include the script in your HTML
<script src="struktur.js"></script>
```

## Usage

```javascript
// Basic usage with default configuration
const result = struktur();

// Usage with custom configuration
const config = {
  N: 6,                    // Minimum number of similar elements
  minWidth: 200,           // Minimum width of structure candidates
  minHeight: 100,          // Minimum height of structure candidates
  highlightStruktur: true, // Highlight detected structures
  fulltext: false,         // Extract full text or individual nodes
  errorMargin: 0.125       // Alignment error margin
};

const result = struktur(config);
```

## Configuration Options

| Option | Default | Description |
|--------|---------|-------------|
| N | 6 | Minimum number of similar elements required |
| minWidth | 200 | Minimum width in pixels for structure candidates |
| minHeight | 100 | Minimum height in pixels for structure candidates |
| structureTags | ['div', 'article', 'p', ...] | Allowed HTML tags for structure detection |
| highlightStruktur | false | Visually highlight detected structures |
| highlightContent | false | Highlight detected content within structures |
| noDataImgSrc | true | Ignore data URLs in image sources |
| addClass | false | Include element classes in output |
| fulltext | false | Extract complete text content instead of individual nodes |
| onlyObjectsWithLinks | true | Only include objects containing links with text |
| errorMargin | 0.125 | Alignment error margin percentage |

## Return Value

The function returns a JSON string containing:
- Detected structures with extracted content
- Number of candidate structures found
- Execution time statistics

## Usage with Puppeteer

Here's how to use Struktur.js with Puppeteer for web scraping:

```javascript
const puppeteer = require('puppeteer');

async function scrapeWithStruktur() {
  // Launch browser with specific configurations
  const browser = await puppeteer.launch({
    headless: false,
    args: [
      '--window-size=1920,1040',
      '--start-fullscreen',
      '--hide-scrollbars',
      '--disable-notifications'
    ]
  });

  // Create new page and configure viewport
  const page = await browser.newPage();
  await page.setBypassCSP(true);
  await page.setViewport({ 
    width: 1920, 
    height: 1040 
  });

  // Navigate to target URL
  await page.goto('https://duckduckgo.com', {
    waitUntil: 'networkidle0'
  });

  // Wait for content to load
  await page.waitForTimeout(1000);

  // Inject Struktur.js into the page
  await page.addScriptTag({path: 'struktur.js'});

  // Execute Struktur with custom configuration
  const result = await page.evaluate(() => {
    const config = {
      N: 6,
      minWidth: 200,
      minHeight: 100,
      highlightStruktur: true,
      fulltext: false,
      errorMargin: 0.125
    };
    return struktur(config);
  });

  await browser.close();
  return result;
}
```

## License

MIT License - Copyright (c) 2019 Nikolai Tschacher (incolumitas.com)
