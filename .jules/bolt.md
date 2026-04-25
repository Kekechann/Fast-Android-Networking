## 2024-05-24 - Okio vs Manual FileOutputStream in Android
**Learning:** When downloading files in Android apps that use OkHttp, reading the response stream into a `FileOutputStream` with a manual byte array buffer (e.g., 2KB) is significantly slower than using Okio. OkHttp already exposes an Okio `BufferedSource`. By wrapping the destination `File` in an Okio `BufferedSink` and calling `sink.writeAll(source)`, the app leverages Okio's segment pooling and native I/O optimizations, avoiding manual buffer allocation and extra array copying.
**Action:** Whenever handling `Response.body().byteStream()` or `source()` from OkHttp, always prefer Okio's `writeAll` over manual `InputStream` loops for file operations.

## 2024-05-15 - Redundant URL building in ANRequest
**Learning:** `ANRequest.getUrl()` parses and builds the URL every time it is called. The URL doesn't change after the request is built, so this is a wasted operation, especially since `getUrl()` is called multiple times during the lifecycle of a request.
**Action:** Cache the formatted URL string in a `formattedUrl` variable in `ANRequest` the first time it is built, and return the cached string on subsequent calls.
