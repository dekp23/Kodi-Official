# Fix: Replace Broken HTML Scraping with Metadata API for Playback

## Problem

The `play()` method was scraping archive.org's HTML page for a `js-play8-playlist` element to resolve stream URLs. Internet Archive removed this element when they retired their old Play8 player, causing all playback to fail with "unplayable item" in Kodi's playlist player.

## Changes

- Removed the `re.search` regex scrape of `js-play8-playlist` from the HTML page
- Removed the HTML request entirely from `play()`
- All playback now goes through the stable `archive.org/metadata/<item_id>` JSON API
- **Video:** fetches files with `height` field, sorts by resolution/size, prompts user to choose source
- **Audio:** fetches files by extension (`.mp3`, `.ogg`, `.flac`, `.opus`, `.wav`, `.aac`, `.m4a`), builds playlist for multi-file items
- Added proper `setResolvedUrl(False)` calls on failure instead of silent returns

## Tested On

| Component | Version |
|-----------|---------|
| LibreELEC | 12.2.1 |
| Kodi | 21.3 (Omega) |
| Hardware | Raspberry Pi 3B |
