<div align="center">

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=30&pause=1000&color=FF6B6B&center=true&vCenter=true&width=600&lines=Anime+Data+Feature+Extraction;MyAnimeList+Top+50+Analysis;Python+%7C+Pandas+%7C+Data+Wrangling" alt="Typing SVG" />

<br/>

![Python](https://img.shields.io/badge/Python-3.13-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-2.x-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=for-the-badge&logo=jupyter&logoColor=white)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=for-the-badge)

<br/>

> **Turning messy scraped data into clean, structured insights — one feature at a time.**

</div>

---

## 🎯 Problem Statement

Real-world data is never clean. This project tackles a classic data engineering challenge — the entire dataset was scraped from **MyAnimeList (MAL)** with all information crammed into a **single raw text column**:

```
Fullmetal Alchemist: BrotherhoodTV (64 eps)Apr 2009 - Jul 20103,218,472 membersManga Store...
```

The goal: **extract meaningful features** from this unstructured mess and make it analysis-ready.

---

## 🗂️ Project Structure

```
anime-feature-extraction/
│
├── 📓 FeatureExtraction.ipynb   # Main analysis notebook
├── 📄 anime.csv                 # Raw scraped dataset (MAL Top 50)
└── 📘 README.md                 # You're here!
```

---

## 🔧 Feature Engineering Pipeline

The raw `Title` column contained everything — this is what was extracted:

```
RAW:  "GintamaTV (201 eps)Apr 2006 - Mar 20101,034,412 members"
              │              │          │
              ▼              ▼          ▼
         anime name      Episodes   Total Time
         "Gintama"          201    "Apr 2006 - Mar 2010"
                                          │
                                          ▼
                                    Total Months
                                         48
```

| Feature | Extraction Logic | Output Type |
|---------|-----------------|-------------|
| `Episodes` | Parse text between `(` and `)`, strip `"eps"` | `int` |
| `Total Time` | Grab 20 chars after `)` | `string` |
| `Total Months` | `dateutil.parser` → month difference formula | `int` |

**Month Calculation Formula:**
```python
(end.year - start.year) * 12 + (end.month - start.month) + 1
```

---

## 📊 Key Insights Uncovered

| 🏆 Query | 🎌 Result | Value |
|----------|-----------|-------|
| Highest Rated Anime | Fullmetal Alchemist: Brotherhood | ⭐ 9.10 |
| Most Episodes | Gintama | 📺 201 eps |
| Longest Running (months) | Ginga Eiyuu Densetsu | 📅 111 months (1988–1997) |

---

## 💻 Core Code Snippets

**1. Extracting Episode Count**
```python
def extract_episodes(txt):
    check = False
    data = ""
    for i in txt:
        if i == ")":
            return data
        if check:
            data += i
        if i == "(":
            check = True

df["Episodes"] = df["Title"].apply(extract_episodes)
df["Episodes"] = df["Episodes"].str.replace("eps", "").astype(int)
```

**2. Extracting Air Date Range**
```python
def extract_time(txt):
    for i in range(len(txt)):
        if txt[i] == ")":
            return txt[i+1 : i+21]

df["Total Time"] = df["Title"].apply(extract_time)
```

**3. Calculating Total Months**
```python
from dateutil import parser

def extract_months(date_range):
    parts = date_range.split(" - ")
    start = parser.parse("1 " + parts[0].strip())
    end   = parser.parse("1 " + parts[1].strip())
    return (end.year - start.year) * 12 + (end.month - start.month) + 1

df["Total Months"] = df["Total Time"].apply(extract_months)
```

---

## 🚀 Getting Started

```bash
# Clone the repository
git clone https://github.com/your-username/anime-feature-extraction.git
cd anime-feature-extraction

# Install dependencies
pip install pandas python-dateutil jupyter

# Launch notebook
jupyter notebook FeatureExtraction.ipynb
```

---

## 🧠 Skills Demonstrated

- ✅ **Data Wrangling** — cleaning and parsing unstructured string data
- ✅ **Feature Engineering** — creating new columns from raw text
- ✅ **String Manipulation** — custom Python functions for text parsing
- ✅ **Date Handling** — converting date strings to numeric durations
- ✅ **Exploratory Analysis** — querying for insights using Pandas `.max()` filtering

---

## 📈 Dataset Snapshot

| Rank | Anime | Score | Episodes | Total Time | Total Months |
|------|-------|-------|----------|------------|-------------|
| 1 | Fullmetal Alchemist: Brotherhood | 9.10 | 64 | Apr 2009 – Jul 2010 | 16 |
| 8 | Hunter x Hunter | 9.04 | 148 | Oct 2011 – Sep 2014 | 36 |
| 12 | Ginga Eiyuu Densetsu | 9.02 | 110 | Jan 1988 – Mar 1997 | **111** |
| 16 | Gintama | 8.94 | **201** | Apr 2006 – Mar 2010 | 48 |

---

<div align="center">



</div>
