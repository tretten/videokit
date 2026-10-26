<h1 align="center">VideoKit</h1>

<p align="center">
  <img src="logo.png" width="128" alt="VideoKit icon">
</p>

<p align="center">
A Mac app, command line tool, and MCP server that make video small for the web. Choose a quality or a file size, and VideoKit finds the smallest file that keeps it, in the formats the browsers you care about play.
</p>

<p align="center">
  <a href="https://github.com/tretten/videokit/releases/latest/download/VideoKit.dmg">
    <img src="download-macos.svg" width="260" alt="Download for macOS">
  </a>
</p>

## Features

- Takes MOV, MP4, M4V, MKV, WebM, AVI, and GIF, and writes H.264 MP4 by default, or VP9 WebM, AV1 MP4, and HEVC MP4 (for Safari).
- Quality, not guesses: pick Smaller, Balanced, or Careful, and VideoKit tries encodes until it finds the smallest file that still looks the same as the original. Or give a file size, such as 2 MB, and it finds the best picture that fits, making the picture smaller only when nothing else fits.
- Browsers: name the ones you care about (Chrome, Firefox, Safari, Edge, with a minimum version if you like) and VideoKit writes the most efficient format each one plays, plus H.264 for everything else. Modern means recent versions of all four: AV1, VP9, and H.264.
- Made for video that plays by itself: sound is removed unless you keep it, a poster image of the first frame is optional, and the `<video>` markup for the results is one click away.
- Limits the longest side and the frame rate (30 by default). Removes camera and location data.
- Safe by default: the result is saved beside the original with a suffix (`-min`), or replaces it while the original goes to the Trash or an `.originals` folder. A result that is not smaller than the original is not saved. Undo puts a batch back.
- Before and after preview of each finished file, with playback of the result.
- Ways in: drag onto the window or the Dock icon, Choose…, the Finder right-click menu (Quick Actions › Compress with VideoKit), a global shortcut that compresses what is selected in Finder (⌃⌥⌘K by default, configurable), and the menu bar icon.
- Command line tool and MCP server for scripts and coding agents, in the same app (see below).
- Checks for updates with Sparkle.

## Privacy

Runs entirely on your Mac. No analytics, no uploads. The only network connection is Sparkle's update check. The shortcut asks macOS once for permission to read Finder's selection. The right-click menu needs no permission.

## Install

Download the latest DMG from [Releases](https://github.com/tretten/videokit/releases), drag VideoKit to Applications, and open it. Builds are signed and notarized. Requires macOS 14 (Sonoma) or later on a Mac with Apple silicon. Nothing else needs to be installed: the app brings its own encoders (x264, x265, libvpx, SVT-AV1, Opus).

To get the right-click menu, open System Settings › Extensions › Finder (or Quick Actions) and turn on **Compress with VideoKit**.

## Command line

The command line tool is part of the app: `VideoKit.app/Contents/Helpers/videokit`. Put it on your PATH with a link, or call it by its full path.

```sh
videokit hero.mov                          # hero-min.mp4 beside the original
videokit hero.mov --size 2MB               # the best picture that fits 2 MB
videokit hero.mov --browsers modern        # AV1 + VP9 + H.264 fallback
videokit hero.mov --level careful --poster --snippet
videokit --replace hero.mov                # replace it; the original goes to .originals
videokit --dry-run folder/                 # say what would happen, write nothing
videokit --inspect hero.mov                # codec, size, frame rate, ready for the web?
videokit --json hero.mov                   # machine-readable
videokit --capabilities                    # JSON: every option with default and range
videokit --help
```

## Agents

VideoKit is an MCP server for coding agents. See [AGENTS.md](AGENTS.md).

## License

Free software under the GNU General Public License, version 3 or later. The app bundles ffmpeg with x264, x265, libvpx, SVT-AV1, and Opus.
