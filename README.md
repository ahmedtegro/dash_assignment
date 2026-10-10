# Premier League Analytics Dashboard

An interactive dashboard built with **Dash** and **Plotly** that shows the current Premier League table, team comparisons and top scorers. The data is collected with a **BeautifulSoup** web scraper and stored in CSV files.

## Project Overview

The project has two parts:

1. **Web scraper:** downloads the league table and the top scorers from the web, cleans them with pandas and saves them as CSV files.
2. **Dash dashboard:** reads the CSV files and shows KPI cards, charts, a filter and the league table. The dashboard reads the saved CSV files, not the website, so it keeps working even if the website is slow or unreachable.

Everything is in one Jupyter notebook: `premier_league_dashboard.ipynb`.

## Data Source

| Data | Source |
|------|--------|
| League table | https://www.bbc.com/sport/football/premier-league/table |
| Top scorers | https://www.bbc.com/sport/football/premier-league/top-scorers |
| League table (backup if BBC fails) | https://www.espn.com/soccer/standings/_/league/eng.1 |

Responsible scraping:
- `robots.txt` of each site is checked before scraping and the pages are allowed.
- The scraper identifies itself with a User-Agent, uses a timeout and pauses between requests.
- In the live version the data is refreshed only every 10 minutes.

The scraped data is saved in the `data/` folder:

- `data/pl_table.csv`: position, team, played, won, drawn, lost, goals_for, goals_against, goal_diff, points, scraped_at, source
- `data/pl_scorers.csv`: player, team, goals, scraped_at

## Dashboard Features

- **4 KPI cards:** league leader, best attack, best defence and top scorer.
- **Filter:** a multi-select dropdown to choose which teams to show.
- **Charts:**
  - Points by team (team points comparison)
  - Goals scored vs goals conceded
  - Goals scored by team
  - Top 10 scorers
- **League table** with the selected teams.
- **Bonus: live dashboard.** A `dcc.Interval` scrapes the website again every 10 minutes, overwrites the CSV files and refreshes the dashboard automatically. If scraping fails, the dashboard keeps showing the last saved data.

## Project Structure

```
premier-league-dashboard/
├── premier_league_dashboard.ipynb   # scraper + dashboard
├── data/
│   ├── pl_table.csv                 # scraped league table
│   └── pl_scorers.csv               # scraped top scorers
├── requirements.txt
└── README.md
```

## How to Run

1. Clone the repository and open the folder:

   ```bash
   git clone <repository-link>
   cd premier-league-dashboard
   ```

2. (Optional) create a virtual environment, then install the requirements:

   ```bash
   pip install -r requirements.txt
   ```

3. Open the notebook:

   ```bash
   jupyter notebook premier_league_dashboard.ipynb
   ```

4. Run the cells from top to bottom. The scraping cell needs an internet connection and creates / updates the CSV files in `data/`.

5. Run the dashboard cell. It prints a link, open it in the browser: http://127.0.0.1:8053/

6. For the live version, run the last cell (**Bonus: auto refresh**) instead.

Note: both dashboard cells use port `8053`, so run only one of them at a time (or restart the kernel before running the other).
