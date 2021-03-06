# gatsby-remark-videos

The intent of this plugin is to aid in the embedding of looping 'html5 gifv'
like videos from markdown.

NOTE: This isn't actually published yet.

## Install

`npm install --save gatsby-remark-videos`

## Usage

The order of the pipelines will influence the final order in the `<video />`
tag.

### Configuration

```javascript
// In your gatsby-config.js
plugins: [
  ...
  {
    resolve: `gatsby-remark-videos`,
    options: {
      pipelines: [
        {
          name: 'vp9',
          transcode: chain =>
            chain
              .videoCodec('libvpx-vp9')
              .noAudio()
              .outputOptions(['-crf 20', '-b:v 0']),
          maxHeight: 480,
          maxWidth: 900,
          fileExtension: 'webm',
        },
        {
          name: 'h264',
          transcode: chain =>
            chain
              .videoCodec('libx264')
              .noAudio()
              .videoBitrate('1000k')
          maxHeight: 480,
          maxWidth: 900,
          fileExtension: 'mp4',
        },
      ],
    }
  },
  ...
]
```

### Markdown Syntax

Markdown image syntax is used:

```
Video one:
![](video.avi)
```

Creates roughly this:

```html
<video autoplay loop>
  <source src="/static/video-hash-optshash.webm" type="video/webm" />
  <source src="/static/video-hash-optshash.mp4" type="video/mp4" />
</video>
```
