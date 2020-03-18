🐾 Important MIME types for Web developers

🕘 2019.10.31 由 hoanfirst 编辑

A media type, known as a MIME types or Multipurpose Internet Mail Extensions. 多用途因特网邮件扩展

a media type is a standard that indicates the nature and format of a document, file, or assortment of bytes.

tips：

**`Browsers` use the MIME type(not the file extension) to determine `how to process a URL`**, so it's important that web servers send the correct MIME type in the response's `Content-Type` header. If this is not correctly configured, browsers are likely to misinterpret the contents of files and sites will not work correctly, and downloaded files may be mishandled.


---

the simplest MIME type's structure: `type/subtype`.

type: represents the general category. such as `video`, `text`

subtype: identifies the exact kind of data of the specified type. such as `plain`, `html`

---

the complex MIME type's structure: `type/subtype;parameter=value`.

For example, for any MIME type whose type is `text`, the optional charset parameter can be used to specify the `character set` used for the characters in the data. If no charset is specified, the default is `ASCII (US-ASCII)`. To specify a `UTF-8` text file, the MIME type `text/plain;charset=UTF-8` is used.

---

type分为两类：discrete type(单独类型)和multipart type。

1. discrete type

represents a single file or medium, **such as a single text or music file, or a single video**.

2. multipart type

represents a document that's comprised of multiple component parts, each of which may have its own MIME type, or a multipart type may encapsulate multiple files being sent together in one transaction. **For example, multipart MIME types are used when attaching multiple files to an email**.

there are only two multipart type, `multipart/form-data`, used in the post method of html froms; `multipart/byteranges`, used with 206(http status code) - Partial Content to send part of document，即支持一次请求多个部分内容，request设置`Range: bytes=0-50, 100-150`，要求返回多个部分bytes，再通过response的`Accept-Rages: bytes`判断是否支持，如果支持就会返回：

```

HTTP/1.1 206 Partial Content
Content-Type: multipart/byteranges; boundary=3d6b6a416f9b5
...

```


discrete type|multipart type|
--|--|
font, application, video, audio, , image, text... |multipart/form-data, multipart/byteranges|

text	表明是普通文本文件。Text-only data including any human-readable content, source code, or textual data. `text/plain`, `text/html`

image	表明是某种图像。图像或图形数据，包括位图和矢量静态图像，以及静态图像格式的动画版本，如GIF。`image/jpeg`, `image/png`

audio	表明是某种音频文件。`audio/mpeg`

video	表明是某种视频文件。`video/mp4`

application	表明是某种二进制数据。`application/octet-stream`, `application/pdf`, `application/zip`。`application/x-www-form-urlencoded` 用于表单数据编码为key/value格式发起请求。`application/octet-stream` 用于二进制流数据

font 表明是某种文字。`font/woff`, `font/tff`, `font/otf`

`multipart/form-data`，用于在表单中上传文件时

...
