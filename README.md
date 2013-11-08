# [tunes](../../)
## Native `<audio>` and `<video>` Playlists
#### <b>tunes</b> is an opensource [jQuery](http://jquery.com) plugin that uses [JSON](http://en.wikipedia.org/wiki/JSON) and [data attributes](http://dev.opera.com/articles/view/an-introduction-to-datasets/) to add playlist capabilities to native HTML5 audio and video.

### Goals

1. Provide semantic storage and performant access to playlist data.
2. Provide succinct semantic controls that can be styled in [CSS](tunes.css).
3. Be minimal, but extensible.

### Types

Filetypes dictate compatibility. The more types you provide, the better. View the [compatibility grid](https://developer.mozilla.org/en-US/docs/Media_formats_supported_by_the_audio_and_video_elements#Browser_compatibility) to see possible types. To cover all modern browsers you need at least 2 types: 

- **audio** - use `.mp3` and `.ogg`  - converters: [media.io](http://media.io)
- **video** - use `.mp4` and `.webm` - converters: [ffmpeg](http://ffmpeg.org) | [Miro](http://mirovideoconverter.com) ([issues](http://stackoverflow.com/a/13449719/770127))

<b>tunes</b> does not deal with Flash fallbacks for pre-HTML5 browsers. However fallbacks and graceful degradation are possible through smart use of `[data-tunes-insert]` and `[data-tunes-attr]`. A [vanilla diet](http://coding.smashingmagazine.com/2012/11/13/the-vanilla-web-diet/) approach is recommended.

### URIs

- To simplify the examples here, most of the file URIs shown are relative. In production you probably want to use full URIs.
- AJAX-loaded .json files must be on the same domain due to cross-domain restrictions.

### [data-tunes]

`[data-tunes]` is the data attribute in which the JSON playlist is stored. It is designed to be placed on a container element that holds the media element and related informational elements such as credits or captions. It can contain inline JSON **or** the filename of a .json file to load via AJAX. Inline JSON is more performant and more stable than loading AJAX requests. 

```html
<div data-tunes="playlist.json">
    <video controls>
        <source src="default.mp4" type="video/mp4">
        <source src="default.webm" type="video/webm">
    </video>
</div>
```

### [data-tunes-insert]

`[data-tunes-insert]` makes it possible to insert values from the properties in your media object into your HTML.

```html
<figure data-tunes="playlist.json">
    <video controls>
        <source src="default.mp4">
        <source src="default.webm">
    </video>
    <figcaption data-tunes-insert="caption">
        Caption for the default video. The value of the "caption"
        property gets inserted here when the video changes.
    </figcaption>
</figure>
```

### [data-tunes-attr]

`[data-tunes-attr]` makes it possible to update arbitrary HTML attributes based on the properties in your media object. It takes a JSON object that maps attribute names to the property names from the media object that should fill them.

```html
<figure data-tunes="playlist.json">
    <video controls>
        <source src="default.mp4">
        <source src="default.webm">
        <p>
            To watch this video please <a href="http://browsehappy.com">updgrade your browser</a>
            or <a href="default.mp4" data-tunes-attr='{"href": "mp4"}'>download the .mp4</a>
        </p>
    </video>
</figure>
```

### JSON

The format for the JSON playlist data is an array of "media objects" containing data about each media file. Please [validate your JSON](http://jsonlint.com) to prevent syntax errors. The media objects provide several capabilities. A simple `<video>` example would look something like this:

```json
[{
    "mp4": "identity.mp4"
  , "webm": "identity.webm"
 },{
    "mp4": "supremacy.mp4"
  , "webm": "supremacy.webm"
 },{
    "mp4": "ultimatum.mp4"
  , "webm": "ultimatum.webm"
}]
```

**Alternate syntax:** You can achieve the same as above by setting the `src` property to an array of URIs. If you mix the 2 syntaxes, the named extension props take precedence over the `src` prop. In either case **tunes** will choose the most appropriate file based on the feature detection.

```json
[{
    "src": ["identity.mp4", "identity.webm"]
 },{
    "src": ["supremacy.mp4", "supremacy.webm"]
 },{
    "src": ["ultimatum.mp4", "ultimatum.webm"]
}]
```

In your media objects, you can include whatever extra properties you want for use with `[data-tunes-insert]` and/or `[data-tunes-attr]`. The purpose of these attributes is to enable you to include relavent credits, captions, or links.

```json
[{
    "mp4": "identity.mp4"
  , "webm": "identity.webm"
  , "title": "The Bourne Identity"
  , "imbd": "http://www.imdb.com/title/tt0258463/"
 },{
    "mp4": "supremacy.mp4"
  , "webm": "supremacy.webm"
  , "title": "The Bourne Supremacy"
  , "imdb": "http://www.imdb.com/title/tt0372183/"
 },{
    "mp4": "ultimatum.mp4"
  , "webm": "ultimatum.webm"
  , "title": "The Bourne Ultimatum"
  , "imdb": "http://www.imdb.com/title/tt0440963/"
}]
```

## MIME types

In order for media files to play your media server must be configured to serve the correct MIME types as described by [html5doctor.com](http://html5doctor.com/html5-audio-the-state-of-play/). The easiest way to do this is to use the [H5BP](https://github.com/h5bp/html5-boilerplate/)'s [.htaccess](https://github.com/h5bp/html5-boilerplate/blob/master/.htaccess). The needed rules in `.htaccess` are:

```
# MIME types for audio and video files (via h5bp.com)
AddType audio/mp4                      m4a f4a f4b
AddType audio/ogg                      oga ogg
AddType video/mp4                      mp4 m4v f4v f4p
AddType video/ogg                      ogv
AddType video/webm                     webm
AddType video/x-flv                    flv
```

## Troubleshooting

1. Does your JSON validate? Use: [jsonlint.com](http://jsonlint.com)
2. Does your HTML validate? Use: [html5.validator.nu](http://html5.validator.nu)
3. Did jQuery load? Is it version 1.7 or higher? jQuery must run *before* tunes.
4. Are there any JavaScript errors in the console?
5. Is your server configured to serve the correct MIME types? See section above.
6. Are your URIs correct? AJAX-loaded playlists must be on the same server.
7. Ask [@ryanve](http://twitter.com/ryanve) or [submit an issue](https://github.com/ryanve/tunes/issues).

## Dependencies

<b>tunes</b> requires [jQuery](http://jquery.com/) 1.7+ or an [ender](http://ender.jit.su/) build that implements compatible versions of:

- `$()`
- `$.ajax()` *needed only for AJAX playlists
- `$.contains()`
- `$.get()`  *needed only for AJAX playlists
- `$.fn.on()`
- `$.fn.addClass()`
- `$.fn.attr()`
- `$.fn.children()`
- `$.fn.empty()`
- `$.fn.find()`
- `$.fn.html()`
- `$.fn.insertAfter()`
- `$.fn.ready()`
- `$.fn.removeAttr()`
- `$.fn.removeClass()`

## Resources

- [html5rocks.com: HTML5 Video](http://www.html5rocks.com/en/tutorials/video/basics/)
- [dev.opera.com: HTML5 Video and Audio](http://dev.opera.com/articles/view/everything-you-need-to-know-about-html5-video-and-audio/)
- [MDN: Media Formats](https://developer.mozilla.org/en-US/docs/Media_formats_supported_by_the_audio_and_video_elements#Browser_compatibility)
- [MDN: Media Events](https://developer.mozilla.org/en-US/docs/DOM/Media_events)
- [MDN: HTMLMediaElement](https://developer.mozilla.org/en-US/docs/DOM/HTMLMediaElement)

## License: [MIT](http://opensource.org/licenses/MIT)

Copyright (C) 2013 by [Ryan Van Etten](https://github.com/ryanve)