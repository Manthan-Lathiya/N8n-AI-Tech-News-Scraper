# AI Tech Article Scraper and Publisher

This repository contains an n8n workflow (`AI_Tech_Article_Scraper_and_Publisher_GitHub.json`) that automates the process of scraping AI and tech articles from various RSS feeds, filtering them, summarizing them using an AI model, checking for duplicates, posting them to a Telegram channel, and storing them in Airtable.

## Overview

The workflow performs the following tasks:
- **Triggers Daily**: Runs at 8 AM daily using a schedule trigger.
- **Fetches a Counter**: Retrieves a counter value from Airtable to determine which website to scrape.
- **Selects a Website**: Rotates through 11 tech websites (e.g., Spectrum, Gizmodo, Engadget) based on the counter.
- **Scrapes RSS Feeds**: Fetches the latest articles from the selected website's RSS feed.
- **Filters Articles**: Selects one article relevant to AI or tech using keyword matching.
- **Summarizes the Article**: Uses OpenAI's `gpt-4o-mini` model to generate a catchy summary for a Telegram audience.
- **Checks for Duplicates**: Ensures the article hasn’t been posted before by checking against an Airtable database.
- **Posts to Telegram**: If the article is new and relevant, posts the summary to a Telegram channel.
- **Stores in Airtable**: Saves the article details (title, summary, link) in Airtable.
- **Updates the Counter**: Increments the counter to rotate to the next website the following day.

## Prerequisites

- **n8n**: You need an n8n instance (self-hosted or cloud). See the [n8n installation guide](https://docs.n8n.io/getting-started/installation/).
- **Airtable Account**: To store the counter and articles.
- **Telegram Bot**: To post articles to a Telegram channel.
- **OpenAI API Key**: To summarize articles using the `gpt-4o-mini` model.

## Setup Instructions

1. **Install n8n**:
   - Follow the [n8n installation guide](https://docs.n8n.io/getting-started/installation/) to set up n8n on your server or local machine.

2. **Import the Workflow**:
   - Download the `AI_Tech_Article_Scraper_and_Publisher_GitHub.json` file from the `workflows/` directory.
   - In n8n, go to the Workflows tab, click the three dots (⋮), and select "Import from File".
   - Upload the `AI_Tech_Article_Scraper_and_Publisher_GitHub.json` file.

3. **Configure Credentials**:
   - **Airtable**:
     - Create an Airtable base with two tables: "CounterTable" (with fields `id`, `CounterValue`) and "ArticlesTable" (with fields `Title`, `Summary`, `Link`).
     - In the JSON file, replace `"<Airtable-Base-Value>"` with your Airtable base ID (found in your Airtable URL, e.g., `appXXXXXXXXXXXXXX`).
     - In the JSON file, replace `"<Table-Value>"` with the appropriate table IDs for "CounterTable" and "ArticlesTable" (e.g., `tblXXXXXXXXXXXXXX`).
     - In the JSON file, replace `"<Record-ID>"` with the record ID of the counter record in the "CounterTable" (e.g., `recXXXXXXXXXXXXXX`).
     - In n8n, go to "Credentials", add a new Airtable credential (using a Personal Access Token), and name it (e.g., "Airtable Personal Access Token").
     - Assign this credential to the following nodes: `Fetch_WebsiteCounter`, `Check_ArticleDuplicate`, `Action_StoreArticleInAirtable`, `Action_UpdateCounterAfterPost`.
   - **Telegram**:
     - Create a Telegram bot using BotFather and add it to your channel.
     - In the JSON file, replace `"@YOUR_TELEGRAM_CHANNEL"` with your Telegram channel ID (e.g., `@YourChannel`).
     - In n8n, go to "Credentials", add a new Telegram credential with your bot token, and name it (e.g., "Telegram Bot Credentials").
     - Assign this credential to the `Action_PostToTelegram` node.
   - **OpenAI**:
     - In n8n, go to "Credentials", add a new OpenAI credential with your API key, and name it (e.g., "OpenAI API Credentials").
     - Assign this credential to the `OpenAI Chat Model` node.

4. **Update Website List**:
   - The `Switch_SelectWebsite` node selects from 11 websites (spectrum, gizmodo, engadget, etc.). Update the list in the node if you want to fetch articles from different sources.

5. **Test the Workflow**:
   - Run the workflow manually to ensure it fetches articles, filters them, checks for duplicates, summarizes them, posts to Telegram, and stores them in Airtable.
   - Verify that the counter increments correctly in Airtable.

## Directory Structure

- `workflows/`: Contains the n8n workflow JSON file (`AI_Tech_Article_Scraper_and_Publisher_GitHub.json`).
- `README.md`: This file, providing an overview and setup instructions.

## Contributing

Feel free to fork this repository, make improvements to the workflow, and submit a pull request. Suggestions for additional features (e.g., more websites, better filtering, or different AI models) are welcome!

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.