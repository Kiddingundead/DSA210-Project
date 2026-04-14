# DSA 210 Project — Can Soundtrack Audio Characteristics Predict a Single Player Game's Metacritic Score?

**Student:** Toprak Ergul | 34485 | Spring 2026

---

## Project Overview

This project investigates whether the audio style of a single player game's official soundtrack is associated with how well the game was reviewed on Metacritic. Soundtrack characteristics (orchestral, electronic, instrumental, ambient) were collected from the Last.fm API and merged with Metacritic critic scores.

---

## Research Question

> Can soundtrack audio characteristics (as tagged by Last.fm users) predict a single player game's Metacritic score?

---

## Data Collection — What We Tried and Why We Changed

### Original Plan: Spotify Audio Features API
The original proposal planned to use the Spotify Web API to collect quantitative audio features (energy, valence, tempo, instrumentalness, acousticness) for each game's official soundtrack album.

However, after setting up the Spotify app and running the script, the API returned a **403 Forbidden error** on the `/v1/audio-features` endpoint. After investigating, we found that Spotify deprecated this endpoint in **November 2024** for all new applications. Only apps created before that date still have access.

### Second Attempt: ReccoBeats API
We then tried ReccoBeats, a third-party alternative that claims to restore access to Spotify-style audio features. However, we immediately ran into a **request rate limit** that blocked data collection at scale. With over 11,000 games to process, this was not a viable solution.

### Final Solution: Last.fm API
We switched to the **Last.fm API**, which provides user-generated tags for albums (such as "orchestral", "electronic", "instrumental", "ambient"). While these tags are not as precise as Spotify's algorithmic audio features, they are crowd-sourced from real listeners and provide a meaningful description of what the soundtrack sounds like. The Last.fm API is free, has no deprecation issues, and has good coverage for well-known game soundtracks.

---

## Data Sources

| Source | What it provides | How collected |
|--------|-----------------|---------------|
| Metacritic (Kaggle) | Game titles, critic scores, genres, release dates | Downloaded from Kaggle |
| Last.fm API | Soundtrack tags (orchestral, electronic, etc.) per game | Collected via Last.fm API |

---

## Repository Structure

```
DSA210-Project/
│
├── analysis.ipynb          # Main notebook — data collection, EDA, hypothesis tests
├── single_player_games.csv # Filtered Metacritic dataset (single player games only)
├── ost_lastfm_raw.csv      # Raw Last.fm soundtrack data collected via API
├── final_dataset.csv       # Merged and cleaned dataset used for analysis
├── requirements.txt        # Python dependencies
└── README.md               # This file
```

---

## How to Reproduce

### 1. Clone the repository
```bash
git clone https://github.com/Kiddingundead/DSA210-Project
cd DSA210-Project
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Run the notebook
Open `analysis.ipynb` in Jupyter or VSCode and run all cells top to bottom.

> **Note:** The Last.fm API key is required to re-collect soundtrack data. The collected data is already saved in `ost_lastfm_raw.csv` so you can skip the API step and run the EDA and hypothesis test cells directly.

---

## Key Findings (Milestone 1 — EDA + Hypothesis Tests)

- **996 single player games** were successfully matched with Last.fm soundtrack data
- **4 hypothesis tests** were conducted using chi-square tests
- **Orchestral soundtracks** were significantly more common in higher-rated games (p=0.034, odds ratio=1.65)
- Electronic, instrumental, and ambient tags showed no significant association with scores
- After Bonferroni correction for multiple testing, no tags remained significant

---

## Milestones

| Date | Milestone |
|------|-----------|
| Mar 31 | Proposal submitted |
| Apr 14 | Data collection, EDA, hypothesis tests ← current |
| May 5 | ML methods |
| May 18 | Final report |
