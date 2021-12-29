---
layout: default
parent: Upload
title: API
nav_order: 4
---

API Reference
=============

### Upload a file

POST /file - upload a file, provide a file within form-data body with a key "file".

Response: JSON with 3 fields - status, digest, download.

Response example:

```json
{
    "status": true,
    "digest": "abcdeAce4VKD2Wg",
    "name": "example",
    "extension": "txt",
    "size": 123,
    "mimetype": "text/plain",
    "download": "http://example.com/file/abcdeAce4VKD2Wg"
}
```

### Download a file

GET /file/{digest} - get a file with the digest {digest}.

Example:

```
GET /file/abcdeAce4VKD2Wg
```

### Get file info

GET /file/{digest}/info - get a file info with the digest {digest}.

Example:

```
GET /file/abcdeAce4VKD2Wg/info
```

Response example:

```json
{
    "status": true,
    "digest": "abcdeAce4VKD2Wg",
    "name": "example",
    "extension": "txt",
    "size": 123,
    "mimetype": "text/plain",
    "download": "http://example.com/file/abcdeAce4VKD2Wg"
}
```

### Delete File

GET /file/{digest} - delete a file with the digest {digest}.

Example:

```
DELETE /file/abcdeAce4VKD2Wg
```

### Upload an image

POST /image - upload an image, provide a file within form-data body with a key "file".

Response: JSON with 4 fields - status, digest, download, thumbnail.

Response example:

```json
{
    "status": true,
    "digest": "abcdeAce4VKD2Wg",
    "name": "example",
    "extension": "jpg",
    "mimetype": "image/jpeg",
    "size": 123,
    "download": "http://example.com/file/abcdeAce4VKD2Wg",
    "thumbnail": "http://example.com/image/abcdeAce4VKD2Wg"
}
```

### Download an image thumbnail

GET /image/{digest} - get an image with the digest {digest}.

Parameters:
- w - resize an image to this width;
- h - resize an image to this height;
- m - resize mode: c - crop-resize, r - preserve dimensions (default).

Example:

```
GET /image/abcdeAce4VKD2Wg?w=500&h=500&m=c
```

### Rotate an image

POST /image/{digest}/rotate - rotate an image with the digest {digest}.

Parameters (json):
- d - degrees to rotate with. Clockwise, only divisible by 90 values are allowed.

Request example:

```
POST /image/abcdeAce4VKD2Wg/rotate
```

```json
{
    "d": 180
}
```

Result of the rotation is a new file. Response example:

```json
{
    "status": true,
    "digest": "abcdeT3pNjL7RaD",
    "download": "http://example.com/file/abcdeT3pNjL7RaD",
    "thumbnail": "http://example.com/image/abcdeT3pNjL7RaD"
}
```

### Crop an image

POST /image/{digest}/crop - crop an image with the digest {digest}.

Parameters (json):
- x - x-coordinate to start with;
- y - y-coordinate to start with;
- w - number of pixels to go to right from the x-coordinate;
- h - number of pixels to go to down from the y-coordinate.

Request example:

```
POST /image/abcdeAce4VKD2Wg/crop
```

```json
{
    "x": 90,
    "y": 100,
    "w": 500,
    "h": 400
}
```

Result of the cropping is a new file. Response example:

```json
{
    "status": true,
    "digest": "abcdeT3pNjL7RaD",
    "download": "http://example.com/file/abcdeT3pNjL7RaD",
    "thumbnail": "http://example.com/image/abcdeT3pNjL7RaD"
}
```

### Delete Image

GET /image/{digest} - delete a image with the digest {digest}.

Example:

```
DELETE /image/abcdeAce4VKD2Wg
```

### Upload a video

POST /video - upload a video, provide a file within form-data body with a key "file". Video will be transcoded to mp4.

Response: JSON with 8 fields - status, digest, name, extension, mimetype, size, download, preview.

Response example:

```json
{
    "status": true,
    "digest": "abcdeAce4VKD2Wg",
    "name": "example",
    "extension": "mov",
    "mimetype": "video/quicktime",
    "size": 123,
    "download": "http://example.com/video/abcdeAce4VKD2Wg",
    "preview":  "http://example.com/video/abcdeAce4VKD2Wg/preview"
}
```

### Download a video

GET /video/{digest} - get a video with the digest {digest}.

Example:

```
GET /video/abcdeAce4VKD2Wg
```

### Download a video preview

POST /video/{digest}/preview - get a video preview with the digest {digest}.

Request example:

```
POST /video/abcdeAce4VKD2Wg/preview
```

### Get video info

GET /video/{digest}/info - get a video info with the digest {digest}.

Example:

```
GET /video/abcdeAce4VKD2Wg/info
```

Response example:

```json
{
    "status": true,
    "digest": "abcdeAce4VKD2Wg",
    "name": "example",
    "extension": "mov",
    "size": 123,
    "mimetype": "video/quicktime",
    "download": "http://example.com/video/abcdeAce4VKD2Wg",
    "preview":  "http://example.com/video/abcdeAce4VKD2Wg/preview",
    "duration": "10"
}
```

### Delete Video

GET /video/{digest} - delete a video with the digest {digest}.

Example:

```
DELETE /video/abcdeAce4VKD2Wg
```

### Upload an audio

POST /audio - upload an audio, provide a file within form-data body with a key "file".

Response: JSON with 8 fields - status, digest, name, extension, mimetype, size, download, duration.

Response example:

```json
{
    "status": true,
    "digest": "abcdeAce4VKD2Wg",
    "name": "example",
    "extension": "mp3",
    "mimetype": "audio/mpeg",
    "size": 123,
    "download": "http://example.com/audio/abcdeAce4VKD2Wg",
    "duration": "10"
}
```

### Download an audio

GET /audio/{digest} - get an audio with the digest {digest}.

Example:

```
GET /audio/abcdeAce4VKD2Wg
```

### Get audio info

GET /audio/{digest}/info - get a audio info with the digest {digest}.

Example:

```
GET /audio/abcdeAce4VKD2Wg/info
```

Response example:

```json
{
    "status": true,
    "digest": "abcdeAce4VKD2Wg",
    "name": "example",
    "extension": "mp3",
    "size": 123,
    "mimetype": "audio/mpeg",
    "download": "http://example.com/audio/abcdeAce4VKD2Wg",
    "duration": "10"
}
```

### Delete audio

GET /audio/{digest} - delete a audio with the digest {digest}.

Example:

```
DELETE /audio/abcdeAce4VKD2Wg
```