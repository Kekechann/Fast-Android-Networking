## 2024-05-24 - Okio vs Manual FileOutputStream in Android
**Learning:** When downloading files in Android apps that use OkHttp, reading the response stream into a `FileOutputStream` with a manual byte array buffer (e.g., 2KB) is significantly slower than using Okio. OkHttp already exposes an Okio `BufferedSource`. By wrapping the destination `File` in an Okio `BufferedSink` and calling `sink.writeAll(source)`, the app leverages Okio's segment pooling and native I/O optimizations, avoiding manual buffer allocation and extra array copying.
**Action:** Whenever handling `Response.body().byteStream()` or `source()` from OkHttp, always prefer Okio's `writeAll` over manual `InputStream` loops for file operations.

## 2024-05-18 - Cached TypeToken/TypeReference reflection overhead
**Learning:** Re-instantiating anonymous inner classes like `new TypeToken<...>() {}` or `new TypeReference<...>() {}` incurs significant reflection overhead as they resolve generic types at runtime.
**Action:** Always cache these generic type descriptors as `static final` constants to avoid repetitive runtime reflection penalties.
