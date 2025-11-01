# AI-Powered Video Idea and Generation Workflow 
(n8n + Gemini + LangChain + Sisif API)

## Overview
This project automates the end-to-end creation of short AI-generated videos around creative topics.  
It uses **n8n** as the automation platform to design a complete workflow - from concept ideation to cinematic video generation and result storage.
The goal is to let anyone generate a ready-to-publish short-form video idea, build the AI-based video prompt automatically, produce the video using the **Sisif API**, and save its metadata and download link in a Google Sheet for review or publication.

## Objective
To create a fully automated pipeline that:
1. Generates an original video idea using **Google Gemini**.
2. Expands that idea into a detailed cinematic video script using **LangChain** and the built-in **Think Tool**.
3. Sends the structured prompt to the **Sisif API** to generate a short vertical video.
4. Stores the prompt details and the generated video URL in **Google Sheets**.
This allows creators to get video ideation and production with minimal effort.

## Key Features
- **Automated Idea Generation:** Uses Google Gemini to produce before/after-style transformation video concepts.
- **Structured Prompt Creation:** Uses a LangChain Agent and the Think Tool to generate production-ready, cinematic prompt templates.
- **AI Video Rendering:** Integrates directly with the Sisif API for real video generation from the prompt.
- **Result Logging:** Saves metadata and video download links in Google Sheets automatically.
- **Fully No-Code Execution:** Built entirely inside the n8n workflow builder.

## Architecture
### Tools and Resources
| Component | Type | Purpose |
|------------|------|----------|
| n8n | Workflow Orchestration | Hosts and runs the full automation pipeline |
| Google Gemini API | AI Resource | Generates creative video ideas |
| LangChain (Think Tool) | AI Tool | Expands ideas into detailed structured prompts |
| Sisif API | Video Generation API | Creates short AI-generated videos from prompts |
| Google Sheets | Resource | Saves final video URLs and metadata |

### System Flow
```text
┌────────────────────────┐
│  n8n Trigger (Daily)   │
└────────────┬───────────┘
             │
             ▼
┌────────────────────────┐
│  Google Gemini Agent   │
│  Generates Idea        │
└────────────┬───────────┘
             │
             ▼
┌────────────────────────┐
│  LangChain Agent +     │
│  Think Tool            │
│  Builds Video Prompt   │
└────────────┬───────────┘
             │
             ▼
┌────────────────────────┐
│  Format Code Node      │
│  Cleans JSON Prompt    │
└────────────┬───────────┘
             │
             ▼
┌────────────────────────┐
│  Sisif API HTTP Request│
│  Generates Video       │
└────────────┬───────────┘
             │
             ▼
┌────────────────────────┐
│  Wait Node (Rendering) │
└────────────┬───────────┘
             │
             ▼
┌────────────────────────┐
│  Download / URL Fetch  │
│  + Google Sheets Save  │
└────────────────────────┘
```

## How the Workflow Operates
1. **Trigger:** The n8n schedule trigger starts the workflow daily (or manually).
2. **Idea Generation (Gemini):** Gemini generates a creative before/after transformation video concept including caption, idea, environment, and sound.
3. **Prompt Template Setup:** A Set node defines a master JSON structure (camera, lighting, motion, etc.) which acts as a prompt blueprint.
4. **Prompt Generation (LangChain + Think Tool):** The LangChain Agent uses the Think Tool to expand the Gemini idea into a structured cinematic prompt suitable for the Sisif API.
5. **Prompt Formatting:** A Code node cleans and validates the JSON before sending it.
6. **Video Creation:** The Sisif API generates a short (5-second) 9:16 video based on the AI prompt.
7. **Video Retrieval:** After waiting a few minutes, another HTTP request fetches the generated video's status and URL.
8. **Storage:** The video URL and metadata are automatically written to a Google Sheet.

## Setup Instructions
### 1. Clone the Repository
```bash
git clone https://github.com/JeevanaMukku/ai-idea-and-video-generator.git
cd ai-video-generation-workflow
```
### 2. Import Workflow into n8n
1. Open your n8n instance.
2. Go to **Workflows → Import from File**.
3. Upload the JSON file from this repository.
### 3. Configure API Credentials
In n8n, go to **Credentials** and create:
| Credential Name | Type | Notes |
|-----------------|------|-------|
| Google Gemini (PaLM) API | GooglePalmApi | Needed for idea generation |
| Sisif Auth | HTTP Header Auth | Add your Sisif API key as the value for Authorization |
| Google Sheets OAuth2 | Google Sheets | Connect your Google account for logging results |
### 4. Set Environment Variables
Optional, but you can store your API keys in `.env`:
```ini
SISIF_API_KEY=<your_api_key_here>
GEMINI_API_KEY=<your_api_key_here>
```
### 5. Run the Workflow
1. Click **Execute Workflow** in n8n.
2. The system will automatically generate a video and store it in your connected Google Sheet.

## Future Scope
- **Automated Trend Integration:** Add a step to fetch real-time trending topics from Google Trends or Twitter and feed them into the Gemini idea generator.
- **Social Upload Integration:** Extend the workflow to auto-post videos to YouTube Shorts, or Instagram Reels.
- **Multiple Video Styles:** Allow users to select between different video templates (cinematic, nature, product, futuristic, etc.).
- **Enhanced Analytics:** Store engagement or view metrics alongside each video in Sheets or a database.

## Summary
This n8n-based workflow demonstrates how to chain together AI creativity (Gemini + LangChain) and automated video production (Sisif API) inside a simple, low-code system.
It allows anyone to scale content ideation and production using only visual workflow building and standard web APIs.
