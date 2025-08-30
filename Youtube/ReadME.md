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
- Availability >> Consistency for video uploads
- Supportig uploading/ streaming of large videos (256gb)
- Low latency streaming(<500ms) true in low bandwidth
- The system should scale to a high number of videos uploaded and watched per day (~1M videos uploaded per day, 100M videos watched per day).


### Out of Scope (Below the Line)
- The system should protect against bad content in videos.
- The system should protect against bots or fake accounts uploading or consuming videos.
- The system should have monitoring / alerting.

## Scale Requirements

- **Video Uploads:** ~1 million videos uploaded per day  
- **Video Views / Downloads:** ~100 million videos watched per day  
- **Maximum Video Size:** 256 GB
- 
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


```
High-Level Design
Background: Video Streaming

* Video Codec - A video codec compresses and decompresses digital video, making it more efficient for storage and transmission. Codec is an abbreviation for "encoder/decoder." Codecs attempt to reduce the size of the video while preserving quality. Codecs usually trade-off on the following: 1) time required to compress a file, 2) support on different platforms, 3) compression efficiency (a.k.a. how much the original file is reduced in size), and 4) compression quality (lossy or not). Some popular codecs include: H.264, H.265 (HEVC), VP9, AV1, MPEG-2, and MPEG-4.
* Video Container - A video container is a file format that stores video data (frames, audio) and metadata. A container might house information like video transcripts as well. It differs from a codec in the sense that a codec determines how a video is compressed / decompressed, whereas a container dictates file format for how the video is stored. Support for video containers varies by device / OS.
* Bitrate - The bitrate of a video is the number of bits transmitted over a period of time, typically measured in kilobytes per second (kbps) or megabytes per second (mbps). The size and quality of the video affect the bitrate. High resolution videos with higher framerates (measured in FPS) have significantly higher bitrates vs. low resolution videos at lower framerates. This is because there's literally more data that needs to be transferred in order for the video to play. Compression via codecs can also have an effect on bitrate, as more efficient compression can lead to a larger video being compressed to a much smaller size prior to transmission.
* Manifest Files - Manifest files are text-based documents that give details about video streams. There's typically 2 types of manifest files: primary and media files. A primary manifest file lists all the available versions of a video (the different formats). The primary is the "root" file and points to media manifest files, each representing a different version of the video. A video version is typically split into small segments, each a few seconds long. Media manifest files list out the links to these clip files and are used by video players to stream video by serving as an "index" to these segments.

Throughout this write-up, a "video format" will be a shorthand term we use for referring to a container and codec combination. Now, let's jump into the functional requirements!

1) Users can upload videos
One of the main requirements of YouTube is to allow users to upload videos that are eventually viewed by other users. When we upload a video, we need to consider a few fundamental things:
1. Where do we store the video metadata (name, description, etc.)?
2. Where do we store the video data (frames, audio, etc.)?
3. What do we store for video data?


For the video metadata, we are assuming an upload rate of ~1M videos/day. This means, over the course of the year, we'll have ~365M records in the database representing videos. As a result, we should consider storing video metadata in a database that can be horizontally partitioned, such as Cassandra. Cassandra offers high availability and enables us to choose a partition key for our data. We can partition on the videoId, since we aren't really worried about bulk-accessing videos in this design, just querying individual videos.


Scale 

1M uploads
100 M download
Max video size: 256 gb
￼



1. Start from client we will go to api gateway and then video service. The video service takes video/video metadata and store it.
2. Our Api gateway has limit of 10 mb so we will use multipart fomr data with s3 to upload video in chunks
3. S3 will give back predefined urls and client will use these urls to upload chunks. Once it’s uploaded s3 will Give notification to chunker , videomatadata . Video meta data will have now full s3 metatarsal data
4. VideoMeta data will also gave status
4.Video meta data will have meta data about chunks and its resprctive url
5. Chunks will also have transcoding in 240 , 360 etc
6. Now CDN can be used as cache for popluar chunks so ur video is fast


<img width="1469" height="650" alt="Screenshot 2025-08-16 at 3 39 13 PM" src="https://github.com/user-attachments/assets/49bf35a5-b5b5-403c-8a52-9a6bc371a6e7" />


<img width="639" height="618" alt="Screenshot 2025-08-30 at 7 59 08 PM" src="https://github.com/user-attachments/assets/41bb7065-d33e-4971-aa6b-bb240d96a93f" />


