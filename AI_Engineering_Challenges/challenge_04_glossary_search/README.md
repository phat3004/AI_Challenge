# Challenge 04 — Insurance Glossary Search App

This is a searchable, modern web application for insurance terminology built for **Papaya Insurance** using **HTML5, Tailwind CSS, Outfit and Inter Google Fonts**. It compiles over 40 standard insurance terms divided into key categories, featuring instant full-text filtering, term highlighting, an A-Z quick-jump side index, and clickable related terms for cross-navigation.

## Features

- **40+ Accurate Insurance Terms**: Organized across 6 distinct categories (Claims, General Insurance, Coverage, Life & Health, Reinsurance, and Regulatory).
- **As-You-Type Search**: Instant full-text filtering across term names and definitions. Matching search queries are dynamically highlighted using a custom CSS highlight style.
- **Interactive Categories**: Sidebar with category filters displaying term counts for quick scoping.
- **Alphabet Sidebar Jump**: Quick-scroll letters index (A–Z) to filter and jump directly to terms starting with the selected letter.
- **Cross-Referenced Related Terms**: Detailed sidebar panel renders term definitions and links to related terminology. Clicking a related term transitions the view and scrolls to the selected word.
- **Offline Capable**: All data is bundled client-side, making the app 100% functional without an active network connection.

## Getting Started

Open the application instantly in any modern web browser.

### Method 1: Double-click HTML (Direct File Open)
Double-click the `index.html` file or drag it directly into a browser window.

```bash
open index.html
```

### Method 2: Serve Locally (Optional)
Serve the directory locally using Python:

```bash
python3 -m http.server 8000
```
Then open [http://localhost:8000](http://localhost:8000).
