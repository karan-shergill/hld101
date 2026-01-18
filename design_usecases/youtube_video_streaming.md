# YouTube Video Streaming

## Source
1. https://www.hellointerview.com/learn/system-design/problem-breakdowns/youtube
2. https://youtu.be/jWRW2xGMqSw
3. https://youtu.be/-JtjQ-OA7XE
4. https://bytebytego.com/courses/system-design-interview/design-youtube
5. https://medium.com/javarevisited/how-does-youtube-stream-millions-of-videos-daily-3d8627d297e5

## Area of focus
1. How can we handle processing a video to support adaptive bitrate streaming?
2. How do we support resumable uploads?
3. How do we scale to a large number of videos uploaded / watched a day?
4. Speeding up uploads, how to improve?
5. Resume streaming where the user left off, how to do this?
6. View counts for video, how to keep that updated?

## New Learning
1. AWS S3 multipart upload / GCS resumable upload
    1. Pre-signed URL upload
    2. Before video is uploaded its chunked into smaller sizes at client side
2. What is manifest file? what importance it holds?
    1. This hold importance for Netflix, Spotify design as well.
3. What is transcoding? Why its important, why its needed?
4. What is video processing pipeline, what is this used for? what are different steps/systems?
5. When to use CDN and when to use blob storage like s3?
