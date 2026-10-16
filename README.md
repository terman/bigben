# bigben

index.html is a webpage that will display the clock face of Big Ben (using the current
local time) and, optionally, play ticks and/or the appropriate chimes.
You can access the webpage at https://heffalump.mit.edu/bigben or clone the repo and
access `file://...repodir.../index.html`.

I like to view this page in a background tab of my always-open browser, setting the volumes
so as to simulate having a grandfather clock in a neighboring room.

Acknowledgements:

* clock face: https://bigbentime.com
* sounds: https://sound-effects.bbcrewind.co.uk/search?q=Big%20Ben

## Background-tab audio

This version uses the Web Audio API and schedules chimes ahead on the browser's audio
clock. This avoids relying on JavaScript `setTimeout()` at the instant of a quarter-hour,
which is unreliable when browsers throttle or freeze background tabs.

On each page load, click **Start audio & close** (or **Enable / recover audio**) once.
Modern browsers require a user gesture before a page may start audible playback.
After that click, the page preloads the local MP3 files and schedules roughly the next
26 hours of quarter-hour/hour chimes. It extends the schedule periodically and rebuilds
it when the tab becomes visible again.

Ticks are generated as a looping Web Audio buffer instead of repeated JavaScript timers.
The 12 o'clock strike count is also corrected to 12.

For best reliability, serve this directory over HTTP/HTTPS. Opening `index.html` through
`file://` may prevent `fetch()` from loading the MP3 files in some browsers; in that case
the page falls back to ordinary `<audio>` playback, which is less reliable in a background
tab.

No ordinary web page can guarantee background audio in every browser/device state.
Mobile browsers may suspend or interrupt audio when the browser is backgrounded or the
screen is locked. The page attempts to recover the AudioContext when it becomes active
again.
