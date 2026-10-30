# image-fit.yazi

Scale images and PDF pages to fit the preview pane by rendering a resized copy
and showing it in Yazi.

## Requirements

- macOS (uses `/usr/bin/sips`)
- `pdftoppm` from Poppler for PDF previews (`brew install poppler`)
- Yazi 25.12.29 or newer

## Compatibility

Maintained against Yazi/Ya `26.8.15`.

## Installation

```sh
ya pkg add stellarjmr/image-fit
```

## Usage

Add this to `yazi.toml`:

```toml
[plugin]
prepend_previewers = [
  { mime = "image/*", run = "image-fit" },
  { mime = "application/pdf", run = "image-fit" },
]
```

The PDF entry is optional. Yazi's built-in PDF previewer renders pages at
150 DPI and never upscales them, so they stop filling large preview panes.
With this entry, pages are rendered at the preview area's pixel size instead.
Use <kbd>J</kbd>/<kbd>K</kbd> to change pages as usual.

To let this plugin fit images to the full preview area, add:

```toml
[preview]
# Let images use the full preview area by removing caps.
max_width = 9999
max_height = 9999
```

## Notes

- The plugin falls back to Yazi's built-in image renderer if `/usr/bin/sips`
  fails or returns malformed status metadata.
- It falls back if image metadata is unavailable, malformed, or inaccessible.
- It falls back if source file metadata is malformed, or if the source image URL
  is missing or cannot be converted for `/usr/bin/sips`.
- It falls back if preview area metadata is missing or malformed.
- It falls back if preview cap metadata is missing or inaccessible.
- Spot fallback errors are ignored so metadata panel rendering does not crash.
- It also falls back if a temporary resized image path cannot be allocated.
- It falls back if the temporary resized image cannot be converted into a Yazi
  preview URL.
- For PDFs, the plugin falls back to Yazi's built-in PDF previewer if
  `pdftoppm` fails (including pages past the end of the document), or if the
  preview area size or a temporary path is unavailable.
- This plugin keeps the original aspect ratio. If you want stretch or crop
  behavior, adjust the plugin implementation.

## License

MIT License - see [LICENSE](LICENSE) file for details.
