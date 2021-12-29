---
layout: default
parent: Upload
title: Configuration
nav_order: 3
---

Environment variables
=====================

- UPLOAD_HOST - host, to set correct download urls in the response (without http://). Required.
- UPLOAD_PORT - exposed port of host, to set correct download urls in the response. Optional. The default value is 80.
- UPLOAD_MAX_FILESIZE - maximum allowed size of file. Optional. The default value is 10M.
- UPLOAD_MAX_DIMENSION - maximum allowed dimension of image. Optional. The default value is 1000. If image is more than this value, uploader will resize the image.
- UPLOAD_VIDEO_MAX_DIMENSION - maximum allowed dimension of video. Optional. The default value is 720. If video is more than this value, uploader will resize the video.
- UPLOAD_DIGEST_PREFIX - after every file is uploaded, a unique identificator, called "digest", is returned in response (for example, "abcdeAce4VKD2Wg"), UPLOAD_DIGEST_PREFIX (in the example "abcde") will be set at the beginning of the digest. If you have multiple upload servers this prefix will help to determine which server preserved a file. Optional.
- UPLOAD_DIGEST_LENGTH - length of the meaningful part of the digest (without UPLOAD_DIGEST_PREFIX). Optional. The default and minimum allowed value is 10.
- UPLOAD_AUTH - File download authentication (see below). The default value is false.
- UPLOAD_AUTH_SALT - File download authentication salt (see below). The default value is false.
- PG_HOST - PostgreSQL connection host. Required.
- PG_REAL_HOST - PostgreSQL database instance host (not PgBouncer). Required.
- PG_PORT - PostgreSQL port. Default value is 5432.
- PG_DATABASE - PostgreSQL database name. Required.
- PG_SCHEMA - PostgreSQL database schema. Default is "public".
- PG_USER - PostgreSQL user name. Required.
- PG_PASSWORD - PostgreSQL user password. Required.
- CLEAR_CACHE_PERIOD - Time in seconds thumbnail's cache lives in seconds. Optional. Default is 5184000 (2 months).
- PHP_MAX_EXECUTION_TIME - max_execution_time option in php.ini. Optional. The default value is 60.
- PHP_MEMORY_LIMIT - memory_limit option in php.ini. Optional. The default value is 512M.
- PHP_PM_MAX_CHILDREN - number of FPM workers. Default value is 10.
- PHP_PM_MAX_REQUESTS - number of FPM max requests. Default value is 500.

Volumes
=======

- /opt/upload/files - directory with all uploaded files.
- /opt/upload/web/cache - directory with cached thumbnails.

If you want to make any additional configuration of container, mount your bash script to /opt/setup.sh. This script will be executed on container setup.
