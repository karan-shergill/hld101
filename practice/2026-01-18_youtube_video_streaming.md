# YouTube Video Streaming

# My Approach
![youtube_video_streaming](images/youtube_video_streaming.png)

## Requirements

### Functional

1. User is able to upload a video
2. Users across globe are able to view/stream the uploaded video

### Non-Functional

1. In CAP, Availability >>> Consistency
2. Users able able to steam video with minimum buffering/latency
3. System need to be at scale

## HLD Steps

1. User wanted to upload a video on youtube
    1. User added metadata like title and other info and a POST API request is made to youtube
    2. The request passes through API gateway
    3. Request reached [video upload service]
2. Video metadata is stored in database and we get a unique video_id
3. Video metadata also made a request to blob storage like s3 for pre-signed upload URL links
4. Pre-signed upload URL are send back to client
5. Using Pre-signed upload URL, client uploads the raw video directly to blob storage s3
    1. On client side of youtube, AWS SDK might been used
    2. so the video is actually 1st chucked into smaller sized and than uploaded in parallel
    3. by this way we can upload in parallel
    4. also if upload is failed only the failed chunk needs to be re-uploaded
6. Once video is fully uploaded by client S3 send a notification to [video processing service]
7. [video processing service] update the s3 links in the metadata table
8. [video processing service] also add the video_id in a queue
9. from this queue [video processing pipeline] pulls the video_id
    1. [video processing pipeline] downloads the original video from S3
10. Inside [video processing pipeline]
    1. the original video gets chunked into smaller parts like 20-30 seconds each
11. Each chunked video get processed for:
    1. video transcoding: keeping diffrent format of same video like 4k, full-HD, hD, 720p etc
    2. check for abusive/adult content and other things
    3. Audio processing
    4. process for subtitle & transcription
    5. A [manifest file] is created
12. video chunks in diffrent format are uploaded to s3 and all the s3 links are update in the [manifest file], and this [manifest file] is also uploaded to s3
13. S3 send notification to [video processing service] regarding all upload
14. [video processing service] update [manifest file] and status in the metadata table
    1. For famous creators, few starting chunks of there video are uploaded to CDN for better streaming experience
15. Viewer request for a video, by clicking in a link
16. [Video streaming service] get its metadata + Manifest file from DB
    1. for viral/trending videos the [manifest file] is already or CDN, the CDN link is in cache
17. [Video streaming service] return [manifest file] & metadata
18. Depending on the bandwidth of the client device / Depending on the client setting on the client device / Depending on the client’ device
    1. Get relevant URL from  [manifest file] and get chunks from CDN
19. As few chunks are been played via CDN, get future chunks from S3 / CDN

<details>
  <summary>ChatCPT Design Review</summary>

Overall, this is a **solid, interview-ready HLD** for a YouTube-like system. Your flow is mostly correct, and you’re thinking in the right direction (pre-signed URLs, chunked upload, CDN, adaptive streaming, async processing).

Below is **structured feedback**: what’s correct, what needs clarification/correction, and what interviewers usually expect you to mention explicitly.

---

## 1. High-Level Verdict

**Rating:** ⭐⭐⭐⭐☆ (4 / 5 for HLD interview)

**Strengths**

* Correct separation of **upload**, **processing**, and **streaming**
* Proper use of **pre-signed URLs** → avoids backend becoming a bottleneck
* Async **video processing pipeline**
* CDN usage for low latency
* Adaptive bitrate streaming using manifest (HLS/DASH conceptually correct)

**Main gaps**

* Some **ownership confusion** (who talks to CDN vs S3)
* Missing **control-plane vs data-plane** clarity
* Processing pipeline scalability details
* A few **incorrect assumptions** (S3 → CDN push, manifest fetching logic)
* Missing reliability & failure handling

---

## 2. Upload Flow Review (Steps 1–6)

### ✅ What you did right

* Metadata first → generate `video_id` ✅
* Pre-signed upload URLs ✅
* Client uploads directly to S3 in chunks ✅
* Parallel + resumable uploads ✅

This is **exactly how real systems do it**.

### 🔧 Improvements / Clarifications

#### 🔹 Chunking location

You wrote:

> the original video gets chunked into smaller parts like 20–30 seconds

👉 **Clarification**:

* **Upload chunking** (5–50 MB parts) ≠ **Streaming chunking** (2–10 sec segments)
* Upload chunking is just multipart upload
* Streaming chunks are created **after transcoding**

Mention this distinction explicitly in interviews.

---

## 3. S3 → Video Processing Trigger (Step 6)

You wrote:

> Once video is fully uploaded by client S3 send a notification to [video processing service]

✅ Correct conceptually.

**Interview-friendly version**

* S3 event → SNS / SQS / EventBridge
* Video Processing Service consumes event
* Marks video as `UPLOADED`

🎯 This shows **decoupling and back-pressure handling**

---

## 4. Video Processing Pipeline (Steps 9–14)

This is your **strongest section**, but it needs more structure.

### ✅ What’s correct

* Async queue-based processing
* Transcoding into multiple resolutions
* Audio, subtitles, abuse detection
* Manifest generation
* Store processed chunks back in S3

Perfect.

---

### 🔧 Important Corrections & Enhancements

#### ❌ “Pipeline downloads the entire video”

You wrote:

> [video processing pipeline] downloads the original video from S3

⚠️ At scale, **workers stream from S3**, not download entire file to disk.

Better phrasing:

> Processing workers stream video from object storage and process in a streaming fashion.

---

#### ❌ “Few starting chunks uploaded to CDN”

You wrote:

> few starting chunks of their video are uploaded to CDN

⚠️ **CDNs pull, not push**.

✅ Correct mental model:

* CDN pulls chunks **on first request**
* Popular videos stay hot in CDN cache
* You *may* pre-warm CDN using background requests, but CDN still pulls

👉 Say this in interviews, it’s a big plus.

---

#### ✅ Manifest responsibility

Good that you mentioned a **manifest file**.

Make it explicit:

* Manifest = `.m3u8` (HLS) or `.mpd` (DASH)
* Contains:

    * Resolution
    * Bitrate
    * Chunk URLs
    * Audio/subtitle tracks

---

## 5. Metadata & Manifest Storage

### ⚠️ Small correction

You wrote:

> Video streaming service get its metadata + Manifest file from DB

Better:

* **DB** → metadata (title, owner, status, duration)
* **Object storage / CDN** → manifest file

Manifest is **too large & mutable** for DB.

---

## 6. Streaming Flow Review (Steps 15–19)

### ✅ What’s correct

* Client requests video
* Streaming service returns metadata + manifest location
* Client selects resolution based on bandwidth/device
* Adaptive bitrate streaming
* CDN → S3 fallback

Excellent.

---

### 🔧 Missing but Expected in Interviews

#### 🔹 Who fetches video chunks?

Important distinction:

* **Backend never streams video data**
* Backend only gives **signed URLs / CDN URLs**
* Client directly fetches chunks from CDN

Say this explicitly.

---

#### 🔹 Adaptive bitrate logic

You can add:

* Player measures:

    * Buffer health
    * Throughput
* Switches resolution dynamically
* Requests next chunk at higher/lower bitrate

---

## 7. CAP & Non-Functional Requirements

### ✅ CAP choice

You said:

> Availability >>> Consistency

Correct.

Make it concrete:

* Eventual consistency between:

    * Metadata DB
    * Processing status
* User may see “Processing…” even after upload

---

### 🔧 Add these NFRs explicitly

Interviewers expect:

* **Idempotency** (upload retries)
* **Fault tolerance**

    * Worker crash mid-transcode
    * Retry with same `video_id`
* **Rate limiting**

    * Upload abuse
* **Cost optimization**

    * Cold storage for old videos
    * Lower resolutions only for unpopular videos

---

## 8. Missing Components (Add These)

These are **small mentions** but score big points:

1. **Authentication & Authorization**

    * Upload permission
    * Private / unlisted videos

2. **DRM / Signed URLs**

    * Time-bound chunk access
    * Prevent hot-linking

3. **Monitoring**

    * Upload failures
    * Processing lag
    * CDN cache hit ratio

4. **Deletion flow**

    * Creator deletes video
    * Async cleanup of chunks + manifests

---

## 9. How to Present This in an Interview (Important)

If you present **exactly this**, structure it like:

1. Clarify requirements (upload, stream, scale)
2. High-level architecture (3 services)
3. Upload flow (pre-signed URLs)
4. Processing pipeline (async, scalable)
5. Streaming (CDN + adaptive bitrate)
6. Trade-offs & failure handling

Avoid step-by-step narration only—**explain intent**.

---

## 10. Final Summary

### What you did exceptionally well

* Pre-signed uploads
* Async processing
* CDN + adaptive streaming
* Clear separation of concerns

### What to fix before interviews

* CDN pulls, not pushes
* Manifest not from DB
* Streaming chunks ≠ upload chunks
* Explicit failure handling

</details>

# Updated Design Post ChatGPT Review

![youtube_video_streaming_2](images/youtube_video_streaming_2.png)

<details>
  <summary>ChatCPT Design 2 Review</summary>

Short answer: **yes, this version is largely correct and interview-ready** 👍
Long answer below with **precise feedback on what is now right, what is still slightly off, and how to explain it confidently in an interview.**

---

## 1. Overall Verdict (Updated Design)

**Rating now:** ⭐⭐⭐⭐⭐ (4.5 / 5)

You’ve fixed the **big conceptual issues** from the first version:

* CDN pull model is clearer
* Manifest handling is much better
* Upload vs streaming vs processing responsibilities are separated cleanly
* Control-plane vs data-plane is *almost* perfect

This is absolutely fine for **FAANG / product company HLD interviews**.

---

## 2. What Is Now Correct (Very Important)

### ✅ Upload Flow (Bottom-left)

This is **textbook correct** now:

* Client sends metadata → API Gateway → Video Upload Service
* Upload Service:

    * Stores metadata
    * Generates **pre-signed URLs**
* Client uploads directly to **S3**
* Multipart, resumable uploads (25–50 MB chunks)

👉 **No backend data bottleneck**
👉 **Exactly what YouTube/S3-style systems do**

✅ You clearly separated:

* *Upload chunking* (MB-based)
* *Streaming chunking* (seconds-based)

Good improvement.

---

### ✅ Processing Pipeline (Bottom-right)

This part is **strong** now:

* S3 upload notification → Video Processing Service
* Video ID pushed to queue
* Workers pull from queue
* Parser → Transcoder → Abuse check → Subtitles
* Create streaming chunks (20–30s)
* Create **manifest**
* Store:

    * Transcoded chunks
    * Subtitles
    * Manifest
      → back to S3

This is **exactly how interviewers expect it**.

💡 Bonus point:
You explicitly showed **queue-based fan-out**, which implies scalability.

---

### ✅ Streaming Flow (Top)

Major improvement here:

* Client requests video
* API Gateway → Video Streaming Service
* Streaming service:

    * Fetches metadata
    * Fetches manifest (from CDN/S3)
* Client:

    * Uses manifest
    * Fetches chunks **directly from CDN**
    * Switches bitrate adaptively

✅ Backend **never streams video data**
✅ CDN is first-class citizen
✅ Adaptive streaming is client-driven

This is correct.

---

## 3. Minor Corrections / Tweaks (Important but Small)

These won’t fail you, but fixing them makes your answer *senior-level*.

---

### 🔧 1. “Put chunks and manifest in CDN” (wording issue)

In your diagram you wrote:

> Put chunks and manifest file in CDN

⚠️ Technically **CDNs pull, not push**.

#### How to say it instead (important for interviews):

> Chunks and manifests are stored in S3. CDN pulls them on first request and caches them.

You already *understand* this — just adjust wording if asked.

---

### 🔧 2. Streaming service fetching manifest from CDN

Right now it looks like:

> Video Streaming Service → CDN → get manifest

This is **okay**, but in real systems:

* Streaming service usually:

    * Returns **manifest URL**
    * Or returns **signed CDN URL**
* Client fetches manifest directly

#### Interview-perfect phrasing:

> Streaming service returns metadata and a signed CDN URL for the manifest. The client fetches it directly.

Not mandatory, but cleaner.

---

### 🔧 3. CDN → S3 fallback path

You wrote:

> if chunk missing from CDN get from S3

Correct, but clarify **who does this**:

* **Client does not decide**
* CDN automatically falls back to origin (S3)

Say:

> On cache miss, CDN fetches chunk from S3 (origin).

---

## 4. What Interviewers Might Ask Next (Be Ready)

If they push deeper, expect these:

### Q1. What if processing fails midway?

Answer:

* Video remains in `PROCESSING_FAILED`
* Retry via idempotent video_id
* Dead-letter queue for repeated failures

---

### Q2. How do you scale processing?

Answer:

* Stateless workers
* Auto-scaling based on queue depth
* GPU instances for transcoding

---

### Q3. How do you optimize cost?

Answer:

* Transcode fewer resolutions for unpopular videos
* Cold storage for old chunks
* CDN cache TTL tuning

---

### Q4. CAP trade-off?

Answer:

* Availability > Consistency
* Manifest & metadata eventually consistent
* User may see “processing” briefly

---

## 5. One-Sentence Interview Summary (Very Useful)

If you had to summarize this design in **one sentence**:

> “We use pre-signed uploads to object storage, asynchronous video processing pipelines to generate adaptive streaming formats, and CDN-backed HLS/DASH streaming so clients fetch video chunks directly with minimal latency.”

That sentence alone sounds **very strong**.

---

## 6. Final Answer to Your Question

> **“Is this fine now?”**

✅ **Yes. This design is correct, realistic, and interview-ready.**
🔹 Only minor wording/clarity tweaks remain
🔹 Architecturally, it’s solid
</details>