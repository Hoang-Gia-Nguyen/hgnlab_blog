+++
title = "Automate Subtitle Translation with Lingarr"
date = 2025-09-26
draft = false
description = "Discover Lingarr, a tool that automatically translates subtitles for your media library by integrating with Sonarr, Radarr, and various AI/translation services."
[taxonomies]
categories = ["Self-hosting"]
[extra]
cover.image = "images/lingarr-cover.png"
cover.alt = "Lingarr Introduction"
+++

If you're a media enthusiast with a self-hosted setup, you've probably faced this problem: you get a new movie or TV series, but you can't find subtitles in your native language. For example, I was excited to watch "Secret of Silent Witch," but a quick search on OpenSubtitles only turned up German subtitles. Since I only understand English and Vietnamese, I was out of luck. This is a common headache, especially for newer or less mainstream content.

### Meet Lingarr: Your Personal Subtitle Translator

Enter [Lingarr](https://github.com/lingarr-translate/lingarr), a fantastic tool designed to solve this exact problem. It acts as a bridge between your media managers (like Sonarr and Radarr) and various translation services, including powerful AI models. It scans your library for subtitles that aren't in your preferred language and translates them for you.

### A Quick Setup Guide

Getting started with Lingarr is quite straightforward. The setup process is broken down into a few key areas:

1.  **Integration**: This is where you connect Lingarr to your existing media managers. You'll need to provide the URL and API key for your Sonarr and Radarr instances. (We won't dive into setting up Sonarr or Radarr here, as Lingarr assumes you already have them running).

2.  **Services**: Here, you choose your translation engine. Lingarr supports traditional services (like DeepL, Google Translate) and modern AI options (like OpenAI's GPT models).
    *   **Traditional Translators**: These are straightforward. Lingarr sends the subtitle text, and the service sends back the translation.
    *   **AI Services**: This is where it gets interesting. You can craft a prompt to guide the AI's translation. Lingarr provides a solid default prompt, but you can customize it. You have two main choices: translate line-by-line with a context prompt, or translate multiple lines in a batch. While the official advice is that line-by-line offers superior context and quality, I've found that batch translation often works better for me. Maybe my context-prompting skills just need some work!

3.  **Automation**: Lingarr can be configured to automatically scan your library and translate any missing subtitles. This is an incredibly powerful feature, but it comes with a big warning: **be careful!** Most of these translation and AI services are paid. If you enable full automation on a large library without setting limits, your next bill might just give you a heart attack.

### Personal Experience: English vs. Vietnamese AI Translations

In my own usage, I've noticed a significant difference in quality between languages. AI-powered translations into English are often excellent and feel very natural. However, when translating into Vietnamese, the results can be a bit clunky.

I believe this is largely because most AI models are predominantly trained on English data. Furthermore, Vietnamese has a complex system of pronouns where "I" and "you" change based on the relationship, age, and status of the speakers. English is much simpler in this regard. This is a nuance that AI often struggles with, sometimes leading to awkward or overly formal dialogue.

Despite this, Lingarr is an invaluable addition to my self-hosting toolkit. It fills a critical gap and makes my media library more accessible than ever. Just remember to keep an eye on your API usage!
