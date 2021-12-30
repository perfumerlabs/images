---
layout: default
title: Upload
nav_order: 120
has_children: true
---

What is it
===========

This is ready-to-use docker microservice for file and image uploading.
Upload takes all care about preserving and distributing files and images.

Lets take deeper look, how it works.

Files
-----

Consider, you have to upload some files and then show download links at some place.

Uploading can be done via next HTTP request:

```
POST https://upload.example.com/file
```

This can be done even from client-side code.
After successful uploading server returns `json` with special `digest` parameter (among other parameters such as file name, extension, size, etc):

```json
{
    ...
    "digest": "qwertyuiop",
    ...
}
```

This `digest` is actually an internal identificator of uploaded file.
If you need to place somewhere a link for file downloading, you can write html link as following:

```html
<a href="https://upload.example.com/file/qwertyuiop" target="_blank">Download my file</a>
```

And that's it.

Images
------

Uploading images is as simple as file.

```
POST https://upload.example.com/image
```

Upload server already returns all needed [CORS-headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS), so this request can be done right from javascript-code.

Again, in response there will be `digest`, which you can then send in AJAX request to your server and save to the database for respective resource.

If you want to show an image somewhere, you can place html "img" tag as following:

```html
<img src="https://upload.example.com/image/qwertyuiop?w=400&h=300&m=c">
```

Notice special parameters we placed to the end of the link: `w`, `h`, `m`.
They are `width`, `height` and `resize mode` parameters respectively.
"w=400&h=300&m=c" query string means, that upload server will resize image to "400x300" dimension with `crop`-typed mode.
`Crop` means that image will be returned exactly as "400x300" (extra dimensions will be cut off).
Another option is "m=r" (which is default behaviour).
It means that image will be resized till it can be placed entirely to rectangle "400x300".

So, when you call "https://upload.example.com/image/qwertyuiop?w=400&h=300&m=c" uploader resizes image on-the-fly and returns smaller image to reduce bandwidth traffic.
Resize is done just once, and then resized image is placed to cache.
On repetitive requests this image will be returned right by bundled nginx web-server making this operation fast.
Original image is not deleted after resize, and is preserved all the time.
Resized image is a smaller copy of that image.

Audio and videos
----------------

Uploader also supports video and audio upload, this is made just like as a file upload.
The only difference is that Uploader converts videos to smaller format after upload to reduce hard drive space consumption.