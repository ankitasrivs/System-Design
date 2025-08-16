## Functional Requirements

### Core Requirements
- Users can upload videos.
- Users can watch (stream) videos.

### Out of Scope (Below the Line)
- Users can view information about a video, such as view counts.
- Users can search for videos.
- Users can comment on videos.
- Users can see recommended videos.
- Users can make a channel and manage their channel.
- Users can subscribe to channels.

## Non-Functional Requirements

### Core Requirements
- The system should be highly available (prioritizing availability over consistency).
- The system should support uploading and streaming large videos (10s of GBs).
- The system should allow for low latency streaming of videos, even in low bandwidth environments.
- The system should scale to a high number of videos uploaded and watched per day (~1M videos uploaded per day, 100M videos watched per day).
- The system should support resumable uploads.

### Out of Scope (Below the Line)
- The system should protect against bad content in videos.
- The system should protect against bots or fake accounts uploading or consuming videos.
- The system should have monitoring / alerting.

## Core Entities

- **User**: A user of the system, either an uploader or viewer.

- **Video**: A video that is uploaded and watched by users.

- **VideoMetadata**: Metadata associated with a video, such as:
  - The uploading user
  - URL reference to a transcript
 
## The API

The API is the primary interface that users will interact with. Defining it early helps guide the high-level system design. Below are endpoints corresponding to our core functional requirements.

### Upload a Video
- **Endpoint:** `POST /upload`
- **Request Body:**
```json
{
  "video": "<video_file>",
  "videoMetadata": {
    "title": "string",
    "description": "string",
    "uploadedBy": "userId",
    "transcriptUrl": "string",
    "tags": ["string"]
  }
}
Description: Uploads a new video along with its metadata.

Stream a Video
Endpoint: GET /videos/{videoId}

Response:

json

{
  "video": "<video_file_or_stream_url>",
  "videoMetadata": {
    "title": "string",
    "description": "string",
    "uploadedBy": "userId",
    "transcriptUrl": "string",
    "tags": ["string"]
  }
}
Description: Retrieves a video for streaming along with its metadata.



