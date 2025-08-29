# Automated Video Content Creation & Posting with n8n

Automate topic expansion, video generation, and social posting—end to end. This project expands topics from Google Sheets using Gemini AI, generates videos via the Jogg API, then posts to X (Twitter), updating your Sheet with video URLs and status for full traceability.

![Automated Video Content Creation and Posting Cover](./images/Automated%20Video%20Content%20Creation%20and%20Posting%20Cover.png)

---

## Table of Contents

- [Overview](#overview)
- [How It Works](#how-it-works)
  - [Workflow Steps](#workflow-steps)
- [Benefits](#benefits)
- [Requirements](#requirements)
- [Setup Instructions](#setup-instructions)
  - [1. Clone or Import Workflow](#1-clone-or-import-workflow)
  - [2. Connect Google Sheets](#2-connect-google-sheets)
  - [3. Connect Gemini AI](#3-connect-gemini-ai)
  - [4. Connect Jogg API](#4-connect-jogg-api)
  - [5. Connect X (Twitter) Posting](#5-connect-x-twitter-posting)
  - [6. Schedule the Trigger](#6-schedule-the-trigger)
  - [7. Test the Workflow](#7-test-the-workflow)
- [Customization](#customization)
  - [Google Sheets Structure](#google-sheets-structure)
  - [AI Prompt Tweaks](#ai-prompt-tweaks)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

---

## Overview

**Automated Video Content Creation & Posting** is an n8n workflow for creators and teams who want to scale short-form content production. It:

- Expands titles/topics from Google Sheets using Gemini AI
- Generates videos via the Jogg API using an avatar and voice
- Waits for processing, retrieves the final video URL
- Posts to X (Twitter) with title and caption
- Updates the Google Sheet with the video URL and posting status

---

## How It Works

![Automated Video Content Creation & Posting workflow](./images/Automated%20Video%20Content%20Creation%20%26%20Posting%20workflow.png)

### Workflow Steps

1. **Scheduled Trigger**
   - Starts daily at a set time (e.g., 6 AM) to run automatically

2. **Fetch titles from Sheets**
   - Reads the next row where `Status` is "Not Posted"

3. **Expand topics with AI**
   - Uses Gemini to produce a concise `title` (<60 chars) and `content` (<400 chars) in structured JSON

4. **Generate videos via Jogg API**
   - Sends the `content` to Jogg’s create endpoint with avatar/voice settings

5. **Wait and retrieve videos**
   - Waits for processing, then polls Jogg for `video_url` and `status`

6. **Update Sheets with Video URL**
   - Writes the `video_url` back to the same row

7. **Upload to X (Twitter), then check success condition**
   - Posts the video with the generated title and caption; captures whether posting succeeded

8. **Update status**
   - Updates `Status` to `Posted` on success or `Error` if posting failed

---

## Benefits

- **Automates content creation/posting** end-to-end
- **Uses AI and video generation** to scale output
- **Centralizes tracking in Sheets** with URLs and statuses
- **Simplifies social media posting** on X (Twitter)

---

## Requirements

- [n8n](https://n8n.io/) installation or cloud account
- Google account with access to Google Sheets
- Google/Gemini API credentials (for AI topic expansion)
- Jogg API key (HTTP Header Auth) for video generation
- X (Twitter) posting credentials supported by your `Upload Post` node
- A Google Sheet with content titles and status tracking

---

## Setup Instructions

### 1. Clone or Import Workflow

- Import the attached `Automated Video Content Creation & Posting.json` into your n8n instance.

### 2. Connect Google Sheets

- Add your Google Sheets OAuth credentials in the Google Sheets nodes (`Get row(s) in sheet`, `Update Video URL`, `Update Status`).
- Set the `documentId` and `sheetName` to your sheet.

### 3. Connect Gemini AI

- Add your Gemini credentials to the `Google Gemini Chat Model` node.
- Ensure the `Expand on Topic` chain uses the Gemini model via the AI connection.

### 4. Connect Jogg API

- In the `Generate Video` and `Get Video` HTTP Request nodes, configure HTTP Header Auth with your Jogg API key.
- Verify the create endpoint and polling endpoint are reachable from your n8n instance.

### 5. Connect X (Twitter) Posting

- Configure the `Upload Post` node with your posting integration (X/Twitter) credentials.
- Ensure the node is set to upload video and accepts `title`, `caption`, and `video` URL inputs.

### 6. Schedule the Trigger

- Adjust the `Schedule Trigger` node (e.g., run daily at 06:00) to match your posting cadence.

### 7. Test the Workflow

- Run once manually to validate connections.
- Confirm a new video is generated, `Video URL` is written to the sheet, and the post is published to X.
- Verify `Status` updates to `Posted` when successful.

---

## Customization

### Google Sheets Structure

Your sheet should include at least these columns (see `data/Contents.xlsx` for reference):

![Contents Data](./images/Contents%20Data.png)

- **Title**: The topic to expand
- **Video URL**: Filled after the Jogg video is ready
- **Status**: `Not Posted`, `Posted`, or `Error`

### AI Prompt Tweaks

- Adjust the instruction in the `Expand on Topic` node to fit your brand voice, tone, and length constraints.
- Modify maximum title/content lengths if your posting platform requires different limits.

---

## Troubleshooting

- **Google Sheets errors**: Re-check OAuth credentials, `documentId`, and `sheetName`.
- **Gemini AI failures**: Validate credentials, model availability, and rate limits.
- **Jogg API issues**: Ensure API key is set; verify create/polling endpoints and payload.
- **Post to X fails**: Confirm `Upload Post` credentials and that the `video_url` is publicly accessible.
- **No status update**: Ensure the `If` node checks the correct success field and subsequent update nodes run.
- **Workflow not triggering**: Validate schedule settings and that n8n is running.

---

## Contributing

Contributions and suggestions are welcome.
Fork the repo, submit issues, or open pull requests to improve the workflow.

---

## License

This project is licensed under the MIT License. See `LICENSE` for details.

---

## Contact

- **Email:** nebeyoumusie@gmail.com
- **LinkedIn:** [LinkedIn](https://www.linkedin.com/in/nebeyou-musie)
- **X (Twitter):** [X](https://x.com/NebeyouMusie)
- **Upwork:** [Upwork](https://www.upwork.com/freelancers/~017ff01729e3cd26e0?mp_source=share)
- **My Agency:** [Fuzion Tech Website](https://fuzion-tech.com/) or [Fuzion Tech on Upwork](https://www.upwork.com/agencies/1948388369189366041/)

---

**Skills & Technologies:**  
`n8n`, `Automation`, `AI Video Generation`, `Social Media Content Creation`, `Google Sheets Automation`

---

> _For any issues, please contact the project maintainer or open an issue on your workflow repository.

![Content Posted](./images/Content%20Posted.png)


