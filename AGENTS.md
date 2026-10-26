# VideoKit for coding agents

VideoKit makes video small for the web: MOV, MP4, M4V, MKV, WebM, AVI and GIF in; H.264 MP4, VP9 WebM, AV1 MP4 or HEVC MP4 out. It runs on the user's Mac; nothing is uploaded. It is an MCP server and a command line tool. Prefer MCP when your client supports it.

## Use it

If the `videokit` MCP tools are not available, ask the user to install VideoKit (the DMG from https://github.com/tretten/videokit/releases) and run:

```sh
claude mcp add videokit -- /Applications/VideoKit.app/Contents/Helpers/videokit mcp
```

Other MCP clients: command `/Applications/VideoKit.app/Contents/Helpers/videokit`, args `["mcp"]`, stdio transport. Without MCP, run the same binary as `videokit` (if it is on PATH) or by that full path.

## MCP tools

- `inspect {paths}`: codec, size in pixels, frame rate, duration, bytes, sound, HDR, whether it is ready for the web already. Encodes nothing.
- `compress {paths, level?, quality?, size?, crf?, browsers?, codecs?, max_side?, fps?, audio?, poster?, verify?, suffix?, output_dir?, replace?, timeout?, confirm_batch?}`: per file and codec `source`, `output`, `state` (done, unchanged, failed), `before`, `after`, `saved`, `savedPercent`, `note`, `backup`.
- `undo {items}`: pass the `source`, `output`, `backup` of a compress result.
- `capabilities {}`: every option with default and range.

Typical flow: `inspect`, then `compress` (default `level: "balanced"`), then `undo` to revert. Use `size: "2MB"` when there is a file size budget, `level: "careful"` when the picture matters, `"smaller"` for throwaways. For a page that plays the video by itself, ask for `browsers: "modern"` and `poster: true`; the `<video>` markup comes back in the report.

## Rules

- Results go beside the original with `-min` added. Originals are untouched unless `replace: true` (then the original goes to `.originals` beside it). Do not set `replace` unless the user asked. `replace` takes one codec.
- Compressing takes minutes for long or noisy video, because it tries several encodes. Do not run two batches at once: "busy" means retry after the running one ends.
- A folder or more than 50 files needs `confirm_batch: true`. Tell the user the count before you set it.
- `/`, the home folder, `~/Library` and `.app` insides are refused.
- A file VideoKit already made is skipped when it comes from a folder, and reported as unchanged in replace mode. `inspect` shows `stampedByVideoKit`.
- `state: "unchanged"` with a `note` is a normal outcome (nothing smaller was found), not an error.
- Tool errors have `isError: true` and a message.

## Command line

```sh
videokit hero.mov                        # hero-min.mp4 beside it
videokit --json --level careful clips/   # NDJSON: one object per file and codec, then {"summary":...}
videokit --dry-run folder/               # what would happen, writes nothing
videokit --inspect --json hero.mov
videokit --capabilities                  # all options as JSON
videokit --help
```

Exit codes: 0 all done or unchanged, 1 something failed, 2 bad flags or paths, 130 interrupted. Unknown flags are errors on stderr.
