+++
title = "Immich Introduction: self-hosted Google Photo"
date = 2025-07-23
description = "Discover Immich, the open-source, self-hosted solution that gives you full control over your photo and video backups, serving as a powerful alternative to Google Photos."

[taxonomies]
categories = ["Technology"]
tags = ["self-hosting", "homelab"]

[extra]
cover.image = "images/cover-immich-introduction.png"
cover.alt = "Immich Introduction"
+++

## What is Immich?

Our photos and videos are more than just files; they are our precious memories. Losing them is simply not an option. That's where a robust backup strategy comes in, and the gold standard is the **3-2-1 backup rule**: keep at least **three** copies of your data, on at least **two** different devices, with **one** copy stored off-site.

While cloud services like Google Photos provide an excellent off-site backup, relying on them alone leaves you vulnerable. What if you lose access to your account or the service changes its terms? This is where Immich comes in.

Immich is a self-hosted, open-source photo and video backup solution that allows you to create that crucial second, local copy of your memories on a server you control. It's the missing piece of the 3-2-1 puzzle for your photo library, giving you a local, instantly accessible copy of every picture and video. Beyond just backup, it's a great alternative to Google Photos, offering full control and privacy over your data.

## Key Features of Immich

Immich is more than just a backup tool; it's a full-featured photo management application. Here are some of the standout features:

-   **Automatic Backup:** Native mobile applications for Android and iOS automatically back up your photos and videos as you take them.
-   **Multi-user Support:** Create separate accounts for family members or friends, each with their own private photo library.
-   **Albums and Sharing:** Organize your photos into albums and share them with others via public links. You can set passwords and expiration dates for shared links.
-   **Metadata and Maps:** Immich reads and displays EXIF data from your photos, including camera settings and location. You can view your photos on an interactive world map.

### AI-Powered Features: The Magic Inside

What truly sets Immich apart are its powerful, locally-run AI features that bring your photo library to life, similar to what you'd expect from Google Photos:

-   **Facial Recognition:** Immich automatically detects faces in your photos and groups them together. You can then name the faces, making it incredibly easy to find all the pictures of a specific person.
-   **Object and Scene Detection:** The AI models can identify thousands of objects and scenes in your photos. This allows you to perform powerful searches like "sunset," "beach," "dogs," or "cars" to instantly find the photos you're looking for without any manual tagging.
-   **Smart Search:** Combine search terms to find exactly what you need. For example, you could search for "photos of [Person's Name] at the beach" to narrow down your results.

![Immich AI Features](/images/demo-immich-ai.png)

## How to install Immich?

The world of self-hosted software moves fast, and installation steps can change. While the guide below provides a solid starting point, it's always best to consult the official documentation for the latest instructions. You can find it on the [Immich homepage](https://immich.app).

The recommended way to install Immich is by using Docker and Docker Compose.

### Prerequisites

-   A server with Docker and Docker Compose installed.
-   A user with permissions to run Docker commands.

### Installation Steps

1.  **Create a directory for Immich:**
    ```bash
    mkdir immich-app
    cd immich-app
    ```

2.  **Download `docker-compose.yml` and `example.env`:**
    ```bash
    wget https://github.com/immich/immich/releases/latest/download/docker-compose.yml
    wget https://github.com/immich/immich/releases/latest/download/example.env
    ```

3.  **Rename `example.env` to `.env`:**
    ```bash
    mv example.env .env
    ```

4.  **Customize your `.env` file:**
    Open the `.env` file and customize the variables. The most important one is `UPLOAD_LOCATION`, which is the path where your photos and videos will be stored. You should also set a strong password for the database.

5.  **Start Immich:**
    ```bash
    docker-compose up -d
    ```

    This command will pull all the necessary images and start the Immich containers in the background.

6.  **Access Immich:**
    You can now access your Immich instance at `http://<your-server-ip>:2283`. The first time you access it, you will be prompted to create an admin account.

## How to import from Google Photos?

The most common way to get your photos out of Google Photos is by using Google Takeout.

1.  **Request a Google Takeout:**
    -   Go to [Google Takeout](https://takeout.google.com/).
    -   Deselect all products and then select only "Google Photos".
    -   Choose the export format and delivery method. It's recommended to split the export into smaller archives (e.g., 50GB) to make it easier to manage.
    -   Wait for the email from Google with the download links.

2.  **Import into Immich:**
    Once you have downloaded and extracted your Google Takeout files, you have a few options to import them into Immich:

    -   **Web UI:** You can drag and drop the files directly into the Immich web interface. This is suitable for a smaller number of files.
    -   **Immich CLI:** For large libraries, the Immich CLI provides a more robust way to upload your files. It can be run directly on the server where your files are located. You can find more information about the CLI in the official Immich documentation.
    -   **[`immich-go`](https://github.com/simulot/immich-go):** This is a third-party command-line tool that is very popular for importing from Google Takeout. It has features like automatic album creation based on the folder structure from Google Takeout.

## How to import from iCloud?

For those embedded in the Apple ecosystem, getting your photos out of iCloud is the first step. The most comprehensive method is to request a full download of your data from Apple.

1.  **Request Your Data from Apple:**
    -   Go to Apple's Data and Privacy portal at [privacy.apple.com](https://privacy.apple.com/).
    -   Log in with your Apple ID.
    -   Find the option to "Get a copy of your data" and click on it.
    -   Select "iCloud Photos" from the list of data categories.
    -   Follow the prompts to confirm your request. Apple will then prepare your data for download and notify you when it's ready.

2.  **Import into Immich:**
    Once you have downloaded and unzipped your iCloud Photos archive, you can use the same import methods as with Google Photos:

    -   **Web UI:** Best for smaller batches of photos.
    -   **Immich CLI or [`immich-go`](https://github.com/simulot/immich-go):** Highly recommended for large libraries to ensure a smooth and reliable import process.

This post provides a basic overview of Immich. In future posts, I will dive deeper into its features and explore more advanced topics.

## A Note on Supporting Immich

Immich is a free and open-source project, driven by a passionate community. You might notice a "Buy me a coffee" or similar donation link within the application. It's important to know that this is purely a way to show your appreciation and support the developers. Donating does not unlock any additional features; all functionalities are available to every user from the start. If you find Immich valuable, consider supporting its continued development.
