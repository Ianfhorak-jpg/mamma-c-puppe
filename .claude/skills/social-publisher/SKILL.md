---
name: social-publisher
description: Automate publishing videos and content to YouTube, TikTok, and Instagram. Use when adding publishing stages to the youtube_shorts pipeline, setting up OAuth authentication, scheduling posts, or debugging upload failures. Covers the full publish automation stack.
argument-hint: [platform] [content-type]
---

# Social Media Publisher

You are an expert in social media automation. Help build reliable, automated publishers for YouTube, TikTok, and Instagram — matching the existing `05_Youtube_shorts` pipeline architecture.

## Supported Platforms

| Platform | API Method | Auth | Content Types |
|----------|-----------|------|--------------|
| YouTube | YouTube Data API v3 | OAuth 2.0 | Shorts, Videos |
| TikTok | TikTok Content Posting API | OAuth 2.0 | Videos (< 60s) |
| Instagram | Meta Graph API | Access Token | Reels, Stories |

## YouTube Publisher

```python
# publishers/youtube_publisher.py
from googleapiclient.discovery import build
from googleapiclient.http import MediaFileUpload
from google.oauth2.credentials import Credentials
from pathlib import Path
import logging

logger = logging.getLogger("pipeline")

class YouTubePublisher:
    def __init__(self, credentials_path: Path):
        creds = Credentials.from_authorized_user_file(str(credentials_path))
        self.youtube = build("youtube", "v3", credentials=creds)
    
    def upload_short(
        self,
        video_path: Path,
        title: str,
        description: str,
        tags: list[str],
        thumbnail_path: Path | None = None
    ) -> str:
        """Upload a YouTube Short. Returns video ID."""
        body = {
            "snippet": {
                "title": title[:100],  # YouTube max 100 chars
                "description": description,
                "tags": tags,
                "categoryId": "22",  # People & Blogs
            },
            "status": {
                "privacyStatus": "public",
                "selfDeclaredMadeForKids": False,
            }
        }
        
        media = MediaFileUpload(
            str(video_path),
            mimetype="video/mp4",
            resumable=True,
            chunksize=1024 * 1024 * 10  # 10MB chunks
        )
        
        logger.info(f"Uploading to YouTube: {title}")
        request = self.youtube.videos().insert(
            part="snippet,status",
            body=body,
            media_body=media
        )
        
        response = None
        while response is None:
            status, response = request.next_chunk()
            if status:
                logger.info(f"Upload progress: {int(status.progress() * 100)}%")
        
        video_id = response["id"]
        logger.info(f"YouTube upload complete: https://youtube.com/shorts/{video_id}")
        
        if thumbnail_path and thumbnail_path.exists():
            self._set_thumbnail(video_id, thumbnail_path)
        
        return video_id
    
    def _set_thumbnail(self, video_id: str, thumbnail_path: Path):
        media = MediaFileUpload(str(thumbnail_path), mimetype="image/jpeg")
        self.youtube.thumbnails().set(videoId=video_id, media_body=media).execute()
```

## Instagram Reels Publisher

```python
# publishers/instagram_publisher.py
import requests
import time
import logging
from pathlib import Path

logger = logging.getLogger("pipeline")

class InstagramPublisher:
    BASE_URL = "https://graph.facebook.com/v18.0"
    
    def __init__(self, access_token: str, instagram_account_id: str):
        self.token = access_token
        self.account_id = instagram_account_id
    
    def upload_reel(self, video_url: str, caption: str) -> str:
        """Upload Reel using a publicly accessible video URL."""
        # Step 1: Create container
        res = requests.post(
            f"{self.BASE_URL}/{self.account_id}/media",
            params={
                "media_type": "REELS",
                "video_url": video_url,  # Must be public URL (use CDN/S3)
                "caption": caption[:2200],
                "access_token": self.token
            }
        )
        res.raise_for_status()
        container_id = res.json()["id"]
        
        # Step 2: Wait for processing
        self._wait_for_processing(container_id)
        
        # Step 3: Publish
        pub = requests.post(
            f"{self.BASE_URL}/{self.account_id}/media_publish",
            params={"creation_id": container_id, "access_token": self.token}
        )
        pub.raise_for_status()
        media_id = pub.json()["id"]
        logger.info(f"Instagram Reel published: {media_id}")
        return media_id
    
    def _wait_for_processing(self, container_id: str, max_wait: int = 120):
        for _ in range(max_wait // 5):
            status = requests.get(
                f"{self.BASE_URL}/{container_id}",
                params={"fields": "status_code", "access_token": self.token}
            ).json().get("status_code")
            if status == "FINISHED":
                return
            if status == "ERROR":
                raise RuntimeError("Instagram video processing failed")
            time.sleep(5)
        raise TimeoutError("Instagram processing timed out")
```

## OAuth Setup (YouTube)

```bash
pip install google-auth google-auth-oauthlib google-api-python-client

# Run once to get credentials:
python setup_oauth.py  # Opens browser for Google login
```

```python
# setup_oauth.py
from google_auth_oauthlib.flow import InstalledAppFlow

SCOPES = ["https://www.googleapis.com/auth/youtube.upload"]

flow = InstalledAppFlow.from_client_secrets_file("client_secret.json", SCOPES)
creds = flow.run_local_server(port=0)

with open("credentials.json", "w") as f:
    f.write(creds.to_json())
print("✅ credentials.json saved")
```

## Instructions

1. Ask which platform(s) to publish to
2. Ask what content type (Shorts, Reels, full video)
3. For YouTube: always use resumable upload (handles large files)
4. For Instagram: video must be publicly accessible via URL — help set up S3 or Cloudinary if needed
5. Show how to add the publisher as the final stage in the pipeline (`stage_03_publish.py`)
6. Always add error handling and retry logic for API failures
