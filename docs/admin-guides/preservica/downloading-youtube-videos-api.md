# YouTube Video Downloads and Metadata Export Guide

> **Purpose:** This guide outlines the process for downloading videos and exporting metadata from a YouTube Brand Account using the YouTube Data API.

> **Note:** This page is under development. Future additions will include:
> - Prerequisites section
> - API Setup section
> - Download Process section
> - Quality Settings section
> - Error Handling section
> - Best Practices section

## Overview

> **Summary:** Here's a summary of the entire process, from creating the app in **Google Cloud** to running the script to get a list of videos from a **YouTube Brand Account** you manage:

### 1. **Set Up the Google Cloud Project**
   
   1. **Go to Google Developers Console**:
      - Visit the [Google Developers Console](https://console.developers.google.com/).
      - Sign in with your Google account.

   2. **Create a New Project**:
      - Click **Select a project** in the top-right corner.
      - Click **New Project**, give it a name (e.g., "YouTube Data API Project"), and click **Create**.

   3. **Enable YouTube Data API v3**:
      - In your project, click on **Enable APIs and Services**.
      - Search for **YouTube Data API v3**, select it, and click **Enable**.

### 2. **Create OAuth Credentials**
   
   1. **Go to Credentials**:
      - In the left-hand menu, click **Credentials**.
      - Click **Create Credentials**, and select **OAuth 2.0 Client ID**.

   2. **Configure OAuth Consent Screen**:
      - You will be prompted to set up the OAuth consent screen.
      - Select **External** for the user type, fill in basic app information (e.g., app name, support email), and **Save**.
   
   3. **Create OAuth 2.0 Client ID**:
      - For **Application type**, select **Desktop App**.
      - Name it (e.g., "YouTube Data API Desktop Client") and click **Create**.
      - Download the `client_secret.json` file, which will be used in your Python script.

### 3. **Set Up Your Python Environment**

   1. **Install the required Python libraries**:
      Run the following command to install the necessary dependencies:
      ```bash
      pip install google-auth google-auth-oauthlib google-auth-httplib2 google-api-python-client yt-dlp
      ```

### 4. **Run the Python Script**

   Use the following script to authenticate and list videos from the YouTube Brand Account:

   ```python
   import os
   import google_auth_oauthlib.flow
   import googleapiclient.discovery
   import googleapiclient.errors

   # Set up the API scope for YouTube Data API v3
   scopes = ["https://www.googleapis.com/auth/youtube.readonly"]

   # Function to authenticate and get the YouTube service
   def get_authenticated_service():
       api_service_name = "youtube"
       api_version = "v3"
       client_secrets_file = "client_secret.json"  # Path to the client secret file

       # Get credentials and create an API client
       flow = google_auth_oauthlib.flow.InstalledAppFlow.from_client_secrets_file(client_secrets_file, scopes)
       credentials = flow.run_local_server(port=0)
       
       youtube = googleapiclient.discovery.build(api_service_name, api_version, credentials=credentials)
       return youtube

   # Function to list videos from the Brand Account
   def list_brand_account_videos(youtube):
       videos = []
       request = youtube.search().list(
           part="id,snippet",
           forMine=True,  # Fetches videos from the authenticated Brand Account
           maxResults=50,
           type='video'
       )

       while request is not None:
           response = request.execute()

           # Collect video IDs and titles
           for item in response['items']:
               if 'snippet' in item:
                   video_id = item['id']['videoId']
                   title = item['snippet']['title']
                   videos.append((title, f"https://www.youtube.com/watch?v={video_id}"))

           request = youtube.search().list_next(request, response)

       return videos

   if __name__ == "__main__":
       # Get authenticated YouTube service
       youtube_service = get_authenticated_service()

       # List videos from the Brand Account
       video_list = list_brand_account_videos(youtube_service)

       # Print video URLs and titles
       for title, url in video_list:
           print(f"Title: {title}, URL: {url}")
   ```

### 5. **Run the Script**

   1. **Place the `client_secret.json` file** in the same directory as your script.
   
   2. **Run the Python script**:
      ```bash
      python your_script.py
      ```

   3. **Sign in to Google**: 
      - During execution, the script will open a browser window for OAuth authentication.
      - Log in with the Google account that has **manager access** to the Brand Account.
      - After selecting your Google account, ensure you switch to the **Brand Account** when prompted.

   4. **View the List of Videos**:
      - The script will print the titles and URLs of all videos from the Brand Account, including public, private, and unlisted ones.

### 6. (Optional) **Download Videos Using yt-dlp**

   If you want to download the videos instead of just listing them, you can integrate **yt-dlp** into the script to handle the downloads, as outlined in the previous examples.

> **Key Points:**
> - **OAuth Authentication**: This process allows you to access the Brand Account's videos, even if they are private or unlisted, as long as you are authenticated as a manager
> - **Cookies for Downloading**: For downloading private/unlisted videos that require authentication, you may need to use cookies with `yt-dlp` (optional if you're just listing videos)
> - **API Limits**: The YouTube Data API has daily quotas (10,000 units/day) for free use, but listing videos for personal use should fall well within these limits

Let me know if you need further clarification or assistance!

# Downloading YouTube Videos via API

> **Note:** This section is under development. Future additions will include:
> - Prerequisites section
> - API Setup section
> - Download Process section
> - Quality Settings section
> - Error Handling section
> - Best Practices section