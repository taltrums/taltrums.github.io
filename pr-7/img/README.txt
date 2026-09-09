================================================================
  MEDIA DROP FOLDER
================================================================

PHOTOS — three stills. Exact filenames matter:

  setup.jpg   desk / rig / battlestation
  lift.jpg    gym / training
  life.jpg    lifestyle / candid

  Portrait, ~1200 x 1500 (4:5), under ~400KB each.
  Greyscale at rest, full color on hover.

----------------------------------------------------------------

VIDEO — the muscle-up reel (vertical, 10s, silent):

  lift.mp4          required
  lift.webm         optional, smaller, browser tries it first
  lift-poster.jpg   first frame, shown before playback

  Nothing gets cropped - the frame adapts to your file.
  Autoplays muted, loops, and only while scrolled into view.

  Compress it (audio stripped, it plays silent anyway):

    ffmpeg -i raw.mov -an -vf "scale=-2:1280" \
      -c:v libx264 -crf 28 -preset slow \
      -movflags +faststart -pix_fmt yuv420p img/lift.mp4

    ffmpeg -i img/lift.mp4 -an -c:v libvpx-vp9 -crf 34 -b:v 0 img/lift.webm

    ffmpeg -ss 1 -i img/lift.mp4 -vframes 1 -q:v 3 img/lift-poster.jpg

  Target under 2MB for the mp4. Check with:  du -h img/lift.mp4

----------------------------------------------------------------
No HTML edits needed for any of this. Paths are already wired.
