#  🎶 Unlocking Music Trends with Data: Advanced Spotify Global Top 50 Dashboard 
### *A Comprehensive Power BI & DAX Query View Project by Bhaskar Pal*  

---

## 📑 Table of Contents
1. [Overview](#overview)  
2. [Business Problems](#business-problems)  
3. [Dataset](#dataset)  
4. [Tools & Technologies](#tools--technologies)  
5. [Dashboards Explained](#dashboards-explained)  
6. [Project Steps](#project-steps)  
7. [Detailed Business Insights](#detailed-business-insights)  
8. [Author & Contact](#author--contact)  
9. [Tags & Keywords](#tags--keywords)  

---

## 📌 Overview  
Spotify, a Swedish audio streaming giant, has become a hub for analyzing global music trends. This project builds an **advanced Power BI dashboard** on the *Spotify Global Top 50 dataset*.  

The goal was to transform raw Spotify data into **interactive visualizations** that highlight:  
- Listening habits & popularity trends  
- Explicit vs non-explicit content patterns  
- Artist dominance and hit-making consistency  
- Song duration, album types, and year-wise performance  

By leveraging **Power BI’s new DAX Query View**, I automated measure creation, saving hours of repetitive work. This approach turned the dashboard into a **professional-grade analytical tool** for music stakeholders.  

---

## ❗ Business Problems  
From the stakeholder perspective (music analysts, playlist managers, marketing teams), the following challenges existed:​:contentReference[oaicite:0]{index=0}  

- **No KPI Monitoring:** Difficult to track key metrics like songs, artists, popularity, or duration.  
- **Lack of Explicit vs Non-Explicit Analysis:** No insights on how explicit content performs.  
- **Song/Album Distribution Gaps:** Hard to track albums vs singles.  
- **Trend Visibility Missing:** No monthly/yearly analysis of song performance.  
- **Disconnected Artist & Song Insights:** No drill-down capability between overview, artists, and songs.  
- **Decision-Making Gaps:** Teams lacked a quick way to identify which content resonates with audiences.  

The Power BI dashboard solves all these problems by combining global datasets with advanced DAX measures.  

---

## 📊 Dataset  
- **Source:** Global *Spotify Top 50 World* dataset (`spotify-top-50-world.csv`)  
- **Format:** CSV  
- **Key Columns:**  
  - `song` → Track name  
  - `artist` → Artist(s)  
  - `popularity` → Popularity score  
  - `duration_ms` → Duration in milliseconds  
  - `is_explicit` → Explicit (True/False)  
  - `position` → Ranking in Top 50  
  - `album_type` → Album/Single  
  - `year`, `month`, `quarter` → Release info  

---

## ⚙ Tools & Technologies  
- **Power BI Desktop** → Data modeling & dashboarding  
- **DAX (Data Analysis Expressions)** → Complex measures & KPIs  
- **Power Query (M Language)** → Data cleaning & transformations  
- **CSV Dataset** → Spotify Top 50 World Data  
- **Power BI DAX Query View** → Bulk measure creation and testing  

---

## 📈 Dashboards Explained  

### 1️⃣ **Overview Dashboard**  
- KPIs: Total Songs, Distinct Artists, Average Popularity, Avg Duration  
- Donut Chart: Explicit vs Non-Explicit Songs  
- Donut Chart: Songs by Album Type (Single, Album, Compilation)  
- Line & Column Chart: Avg Popularity & Distinct Songs by Year/Month  
- Tables: Top Songs & Top Artists by Popularity  

### 2️⃣ **Artist Dashboard**  
- Bar Chart: Top Artists by Popularity  
- Scatter Plot: Tracks per Album vs Songs by Artist  
- Drill-Down: Artist-level insights (Songs, Release Date, Avg Popularity, Avg Position, Duration)  
- KPIs: Artists with Consistent #1 Hits  

### 3️⃣ **Song Dashboard**  
- Ranking: Top Songs by Popularity  
- Donut Chart: Songs Distribution by Album Type  
- Comparison: Songs by Count vs Distinct Artists  
- Detailed Song Table: Song Name, Release Date, Popularity, Position, Duration  

---

## 🛠 Project Steps  

### Step 1: Data Import  
- Imported Spotify Top 50 dataset (`CSV`) into Power BI Desktop.  
- Renamed dataset as `Top-50-world` for consistency.  

### Step 2: Traditional vs Modern Approach  
- **Old Way:** Manually creating one DAX measure at a time.  
- **New Way:** Using **DAX Query View** to define all measures in a single script.  

### Step 3: DAX Query View (Bulk Measure Creation)  
- Enabled DAX Query View under *Preview Features*.  
- Defined **30+ measures** including:  
  - KPIs (Total Songs, Distinct Songs, Distinct Artists, Avg Popularity, Avg Duration)  
  - Explicit vs Non-Explicit comparisons  
  - Artist-specific & Year-specific measures  
  - Position-based rankings (#1 Hits) 

---
```dax
DEFINE
    MEASURE 'Top-50-world'[Total Songs] = COUNTROWS('Top-50-world')
    MEASURE 'Top-50-world'[Distinct Songs] = DISTINCTCOUNT('Top-50-world'[song])
    MEASURE 'Top-50-world'[Distinct Artists] = DISTINCTCOUNT('Top-50-world'[artist])
    MEASURE 'Top-50-world'[Avg Popularity] = AVERAGE('Top-50-world'[popularity])
    MEASURE 'Top-50-world'[Max Popularity] = MAX('Top-50-world'[popularity])
    MEASURE 'Top-50-world'[Min Popularity] = MIN('Top-50-world'[popularity])


    MEASURE 'Top-50-world'[Avg Duration Minutes] = AVERAGE('Top-50-world'[duration_ms]) / 60000
    MEASURE 'Top-50-world'[Max Duration Minutes] = MAX('Top-50-world'[duration_ms]) / 60000
    MEASURE 'Top-50-world'[Min Duration Minutes] = MIN('Top-50-world'[duration_ms]) / 60000

    MEASURE 'Top-50-world'[Explicit Songs] = CALCULATE(COUNTROWS('Top-50-world'), 'Top-50-world'[is_explicit] = TRUE())
    MEASURE 'Top-50-world'[Non-Explicit Songs] = CALCULATE(COUNTROWS('Top-50-world'), 'Top-50-world'[is_explicit] = FALSE())
    MEASURE 'Top-50-world'[Pct Explicit Songs] = DIVIDE([Explicit Songs], [Total Songs], 0)
    MEASURE 'Top-50-world'[Avg Popularity Explicit] = CALCULATE(AVERAGE('Top-50-world'[popularity]), 'Top-50-world'[is_explicit] = TRUE())
    MEASURE 'Top-50-world'[Avg Popularity NonExplicit] = CALCULATE(AVERAGE('Top-50-world'[popularity]), 'Top-50-world'[is_explicit] = FALSE())

    MEASURE 'Top-50-world'[Avg Position] = AVERAGE('Top-50-world'[position])
    MEASURE 'Top-50-world'[Position 1 Songs] = CALCULATE(COUNTROWS('Top-50-world'), 'Top-50-world'[position] = 1)
    MEASURE 'Top-50-world'[Position 1 Artists] = CALCULATE(DISTINCTCOUNT('Top-50-world'[artist]), 'Top-50-world'[position] = 1)

    MEASURE 'Top-50-world'[Avg Tracks per Album] = AVERAGE('Top-50-world'[total_tracks])
    MEASURE 'Top-50-world'[Album Type Count] = DISTINCTCOUNT('Top-50-world'[album_type])
    MEASURE 'Top-50-world'[Singles Count] = CALCULATE(COUNTROWS('Top-50-world'), 'Top-50-world'[album_type] = "single")
    MEASURE 'Top-50-world'[Albums Count] = CALCULATE(COUNTROWS('Top-50-world'), 'Top-50-world'[album_type] = "album")

    -- Artist-scoped
    MEASURE 'Top-50-world'[Songs per Artist] = COUNTROWS('Top-50-world')
    MEASURE 'Top-50-world'[Distinct Songs per Artist] = DISTINCTCOUNT('Top-50-world'[song])
    MEASURE 'Top-50-world'[Avg Popularity per Artist] = AVERAGE('Top-50-world'[popularity])
    MEASURE 'Top-50-world'[Position1 Hits per Artist] = CALCULATE(COUNTROWS('Top-50-world'), 'Top-50-world'[position] = 1)

    -- Time-scoped
    MEASURE 'Top-50-world'[Songs per Year] = COUNTROWS('Top-50-world')
    MEASURE 'Top-50-world'[Avg Popularity per Year] = AVERAGE('Top-50-world'[popularity])
    MEASURE 'Top-50-world'[Avg Duration per Year] = AVERAGE('Top-50-world'[duration_ms]) / 60000
    MEASURE 'Top-50-world'[Pct Explicit per Year] = DIVIDE(
        CALCULATE(COUNTROWS('Top-50-world'), 'Top-50-world'[is_explicit] = TRUE()),
        [Songs per Year], 
        0
    )

EVALUATE
    SUMMARIZECOLUMNS(
        "Total Songs", [Total Songs],
        "Distinct Songs", [Distinct Songs],
        "Distinct Artists", [Distinct Artists],
        "Avg Popularity", [Avg Popularity],
        "Max Popularity", [Max Popularity],
        "Min Popularity", [Min Popularity],
        "Avg Duration Minutes", [Avg Duration Minutes],
        "Max Duration Minutes", [Max Duration Minutes],
        "Min Duration Minutes", [Min Duration Minutes],
        "Explicit Songs", [Explicit Songs],
        "Non-Explicit Songs", [Non-Explicit Songs],
        "Pct Explicit Songs", [Pct Explicit Songs],
        "Avg Popularity Explicit", [Avg Popularity Explicit],
        "Avg Popularity NonExplicit", [Avg Popularity NonExplicit],
        "Avg Position", [Avg Position],
        "Position 1 Songs", [Position 1 Songs],
        "Position 1 Artists", [Position 1 Artists],
        "Avg Tracks per Album", [Avg Tracks per Album],
        "Album Type Count", [Album Type Count],
        "Singles Count", [Singles Count],
        "Albums Count", [Albums Count],
        "Songs per Artist", [Songs per Artist],
        "Distinct Songs per Artist", [Distinct Songs per Artist],
        "Avg Popularity per Artist", [Avg Popularity per Artist],
        "Position1 Hits per Artist", [Position1 Hits per Artist],
        "Songs per Year", [Songs per Year],
        "Avg Popularity per Year", [Avg Popularity per Year],
        "Avg Duration per Year", [Avg Duration per Year],
        "Pct Explicit per Year", [Pct Explicit per Year]
    )
```
---
   
### Step 4: Apply All Measures  
- Clicked **Apply All Updates** to add all measures into the data model in seconds.  

### Step 5: Dashboard Design  
- Created KPI Cards, Bar Charts, Column Charts, Donut Charts, Area Charts, and Detailed Tables.  
- Ensured each page (Overview, Artist, Song) provided **both summary & drill-down insights**.  

---

## 📊 Detailed Business Insights  

1. **Explicit vs Non-Explicit:** Explicit songs formed a noticeable share, but non-explicit songs dominated global reach.  
2. **Artist Dominance:** Few artists consistently appeared with multiple hits, showing strong brand influence.  
3. **Song Duration:** Most popular songs fell within a 3–4 minute duration.  
4. **Popularity Trends:** Popularity fluctuated by month and year, reflecting seasonality in global listening.  
5. **Album Type:** Singles had higher presence compared to full albums.  
6. **#1 Hits Analysis:** Only select artists managed repeated #1 rankings, a key factor for Spotify promotions.  
7. **Year-Wise Trends:** Newer years showed higher song diversity but not always higher popularity.  

---

## 👤 Author & Contact  

**Bhaskar Pal**  
*Aspiring Data Analyst*  

📧 **Email:** [bhaskarpal.official@gmail.com](mailto:bhaskarpal.official@gmail.com)  
🔗 **LinkedIn:** [linkedin.com/in/bhaskar-pal-2k02](https://linkedin.com/in/bhaskar-pal-2k02)  
🌐 **Portfolio:** [bhaskarpal1707.github.io/portfolio](https://bhaskarpal1707.github.io/portfolio)  

---

## 🏷 Tags & Keywords  
`#PowerBI` `#SpotifyDashboard` `#DataAnalytics` `#DAX` `#MusicAnalytics` `#BusinessIntelligence` `#DataVisualization` `#ArtistInsights` `#SongTrends` `#ExplicitVsNonExplicit`  

---

If you found this project helpful or interesting, a ⭐️ would mean a lot!
