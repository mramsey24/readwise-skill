# Readwise CLI Skill

## Description 
Use the `readwise` command to access the user's Readwise highlights and Reader documents. Readwise has two products:

- **Readwise** — highlights from books, articles, podcasts, and more. Includes daily review and spaced repetition.
- **Reader** — a read-later app for saving and reading articles, PDFs, EPUBs, RSS feeds, emails, tweets, and videos.

## Setup

If `readwise` is not installed:
```bash
npm install -g @readwise/cli
```

If not authenticated, ask the user for their Readwise access token (they can get one at https://readwise.io/access_token), then run:
```bash
readwise login-with-token <token>
```





 