# 📚 Robot 3 — E-Commerce Books Web Scraper

![UiPath](https://img.shields.io/badge/UiPath-Automation-FA4616?style=for-the-badge&logo=uipath&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-Windows-0078D4?style=for-the-badge&logo=windows&logoColor=white)
![Browser](https://img.shields.io/badge/Browser-Chrome-4285F4?style=for-the-badge&logo=googlechrome&logoColor=white)
![Excel](https://img.shields.io/badge/Output-Excel-217346?style=for-the-badge&logo=microsoftexcel&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

---

## 📖 Overview

**Robot 3 — E-Commerce Books Web Scraper** is a UiPath RPA automation that scrapes book data from [Books to Scrape](https://books.toscrape.com/) — a sandbox e-commerce website. The robot navigates through multiple pages, extracts detailed book information for each listing, calculates the **inventory value** (Price × Quantity Available), and exports all results into a neatly organized, auto-sorted **Excel workbook** — one sheet per book category.

---

## ✨ Features

- 🌐 **Multi-Page Scraping** — Automatically navigates through up to **10 pages** of product listings
- 📦 **Per-Listing Extraction** — Visits each book's detail page to extract rich data (up to **20 listings per page**)
- 💰 **Inventory Value Calculation** — Computes `Price × Quantity Available` for every book
- 📊 **Excel Export** — Appends results to `BooksInventoryValue.xlsx`, organized by **book category** (one sheet per category)
- 🔃 **Auto-Sort** — Sorts each category sheet by **Inventory Value** in ascending order after scraping
- 🔁 **Browser Navigation** — Automatically clicks through book listings and navigates back after each extraction
- 📝 **Detailed Logging** — Logs every key step (page index, listing number, price, quantity, category, inventory value) for easy monitoring and debugging

---

## 🗂️ Data Extracted

For every book, the robot captures the following fields:

| Field | Description | Type |
|-------|-------------|------|
| `URL` | Direct URL to the book's detail page | `String` |
| `Title` | Full title of the book | `String` |
| `Category` | Book category (e.g., Fiction, Mystery) | `String` |
| `Price` | Book price (£ symbol stripped) | `Decimal` |
| `Quantity Available` | Number of units in stock | `Int16` |
| `Inventory Value` | Calculated as `Price × Quantity Available` | `Double` |

---

## 🗺️ Workflow Architecture

```
Main Sequence
│
├── 🌐 Use Application/Browser — books.toscrape.com (Chrome)
│   │
│   └── 🔁 While Loop — Page Index < 10 (Outer: Page Iterator)
│       │
│       ├── 📝 Log Current Page Index
│       │
│       └── 🔁 While Loop — Listing Index < 20 (Inner: Listing Iterator)
│           │
│           ├── 🖱️ Click Book Title Link
│           ├── 🌐 Get Current URL
│           ├── 📄 Get Text — Book Title
│           ├── 💲 Get Text — Book Price
│           ├── 🔢 Convert Price to Decimal
│           ├── 📦 Get Text — Quantity Available
│           ├── 🔢 Parse Quantity to Int16
│           ├── 🧮 Calculate Inventory Value (Price × Quantity)
│           ├── 🏷️ Get Text — Book Category
│           ├── 📊 Build DataTable (book row)
│           ├── 📝 Write Header Row to Excel
│           ├── ➕ Add Data Row
│           ├── 📤 Append Row to Excel Workbook
│           └── ↩️ Navigate Back to Listing Page
│
│       └── ➡️ Click 'next' — Navigate to Next Page
│
└── 📊 Excel Process Scope
    └── 📂 Use Excel File — BooksInventoryValue.xlsx
        └── 🔁 For Each Sheet
            └── 🔃 Sort by Inventory Value (Ascending)
```

---

## 📁 Project Structure

```
Robot 3 E-Commerce Books Web Scraper/
│
├── Main.xaml                   # 🤖 Main automation entry point
├── Test.xaml                   # 🧪 Test workflow
├── BooksInventoryValue.xlsx    # 📊 Output Excel file (auto-generated)
├── project.json                # ⚙️ UiPath project configuration
├── project.uiproj              # 🗂️ UiPath Studio project file
└── README.md                   # 📖 This file
```

---

## ⚙️ Prerequisites

Before running this automation, make sure you have the following installed and configured:

| Requirement | Version / Details |
|-------------|-------------------|
| **UiPath Studio** | 2023.10 or later |
| **UiPath Robot** | Compatible with project version |
| **Google Chrome** | Latest stable version |
| **UiPath Chrome Extension** | Enabled in Chrome |
| **Microsoft Excel** | 2016 or later (or Office 365) |
| **.NET Framework** | Windows / .NET 6+ |

### 📦 UiPath Package Dependencies

| Package | Purpose |
|---------|---------|
| `UiPath.UIAutomation.Activities` | Browser & UI interaction |
| `UiPath.Excel.Activities` | Excel read/write/sort operations |
| `UiPath.System.Activities` | Core workflow activities |

---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/robot3-books-web-scraper.git
cd robot3-books-web-scraper
```

### 2. Open in UiPath Studio

1. Launch **UiPath Studio**
2. Click **Open Project** and select the `project.json` file
3. Studio will automatically restore all NuGet package dependencies

### 3. Verify Chrome Extension

Make sure the **UiPath Chrome Extension** is installed and enabled:
- Open Chrome → Extensions → Enable **UiPath Web Automation**
- Or install it from the [Chrome Web Store](https://chrome.google.com/webstore)

### 4. Run the Automation

1. Open `Main.xaml` in the designer
2. Press **F5** or click **Run File**
3. The robot will open Chrome, navigate to [books.toscrape.com](https://books.toscrape.com/), and begin scraping

---

## 📊 Output

The robot generates (or appends to) `BooksInventoryValue.xlsx` in the project root:

- **One sheet per book category** (e.g., `Fiction`, `Mystery`, `Nonfiction`, etc.)
- Each sheet contains all books from that category with the 6 extracted fields
- Each sheet is **sorted ascending by Inventory Value** after scraping is complete

### Sample Output (Fiction Sheet)

| URL | Title | Category | Price | Quantity Available | Inventory Value |
|-----|-------|----------|-------|--------------------|-----------------|
| https://books.toscrape.com/... | Tipping the Velvet | Fiction | 53.74 | 20 | 1074.80 |
| https://books.toscrape.com/... | Sharp Objects | Fiction | 47.82 | 20 | 956.40 |
| ... | ... | ... | ... | ... | ... |

---

## ⚙️ Configuration

You can adjust the scraping behaviour by modifying these parameters directly in `Main.xaml`:

| Parameter | Location | Default | Description |
|-----------|----------|---------|-------------|
| Max Pages | `While Page Number Limit` condition | `10` | Number of pages to scrape |
| Max Listings per Page | `While Listing Number Limit` condition | `20` | Max listings scraped per page |
| Output File | `Write/Append Range` activities | `BooksInventoryValue.xlsx` | Output Excel file name |
| Sort Direction | `Sort By Column` activity | `Ascending` | Sort order for Inventory Value |

---

## 📋 Variables Reference

| Variable | Type | Description |
|----------|------|-------------|
| `PageIndex` | `Int32` | Current page number (0-based) |
| `ListingIndex` | `Int32` | Current listing number on a page (0-based) |
| `ListingNumber` | `Int32` | Human-readable listing number (1-based) |
| `PageURL` | `String` | URL of the current book detail page |
| `BookTitle` | `String` | Scraped book title |
| `BookPrice` | `String` | Raw price string (e.g., "£53.74") |
| `DecimalPrice` | `Decimal` | Numeric price with £ symbol removed |
| `QuantityAvailable` | `String` | Raw quantity text (e.g., "In stock (20 available)") |
| `IntQuantityAvailable` | `Int16` | Parsed integer quantity |
| `InventoryValue` | `Double` | Calculated value: `DecimalPrice × IntQuantityAvailable` |
| `BookCategory` | `String` | Book category (used as Excel sheet name) |
| `dt_InventoryValue` | `DataTable` | DataTable holding current book row data |
| `dt_InventoryValueheader` | `DataTable` | DataTable used to write column headers |

---

## 🔍 Logging

The robot produces detailed log messages at each step for easy monitoring:

```
[INFO]  Scraping Page: 1
[INFO]  Scraping Listing 1
[INFO]  Scraped Quantity Available: In stock (20 available)
[INFO]  Quantity Available: 20
[INFO]  Int quantity available: 20
[INFO]  Inventory value on stock in hand: 1074.80
[INFO]  Book Category: Fiction
...
```

---

## 🛠️ Troubleshooting

| Issue | Possible Cause | Solution |
|-------|----------------|----------|
| Chrome doesn't open | Chrome extension not enabled | Enable the UiPath Chrome Extension |
| Selectors fail | Website layout changed | Re-capture selectors in UiPath Studio |
| Excel file locked | File is open in Excel | Close `BooksInventoryValue.xlsx` before running |
| Missing data in sheet | Page loaded slowly | Increase `WaitForReady` timeout on browser activities |
| Quantity parse error | Unexpected text format | Check the availability text format on the website |

---

## 🤝 Contributing

Contributions are welcome! Here's how to get started:

1. **Fork** the repository
2. Create a **feature branch**: `git checkout -b feature/your-feature-name`
3. **Commit** your changes: `git commit -m "Add: your feature description"`
4. **Push** to the branch: `git push origin feature/your-feature-name`
5. Open a **Pull Request**

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## 👤 Author

**Rambharat Patel**
- 📧 Email: [patelrambharat@gmail.com](mailto:patelrambharat@gmail.com)
- 🤖 Built with [UiPath Studio](https://www.uipath.com/product/studio)

---

## 🙏 Acknowledgements

- 🌐 [Books to Scrape](https://books.toscrape.com/) — A safe, legal sandbox website for web scraping practice
- 🤖 [UiPath](https://www.uipath.com/) — RPA platform used to build this automation

---

*⭐ If you found this project helpful, please give it a star on GitHub!*
