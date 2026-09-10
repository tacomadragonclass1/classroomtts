STEAMPUNK ROBOT TTS ASSET PACK
===============================

Included files
--------------
base_head.png
eyes_open.png
eyes_closed_blink.png
mouth_closed.png
mouth_small_open.png
mouth_medium_open.png
mouth_wide_open.png
mouth_o.png

Purpose
-------
This asset pack is meant for a simple talking-head web page.
The easiest setup is to stack all PNGs on top of each other in the same-size container.
All PNGs were generated on the same 1254 x 1254 canvas, so they should align if rendered at the same size and positioned at top: 0; left: 0.

Recommended layer order
-----------------------
1. base_head.png
2. eyes_open.png OR eyes_closed_blink.png
3. one mouth image at a time

Important notes
---------------
- Only show ONE eye state at once.
- Only show ONE mouth state at once.
- Keep all layers absolutely positioned and sized identically.
- Render all image layers at 100% width and 100% height of the same square wrapper.
- Since these are transparent PNG overlays, do not crop them.

Suggested HTML structure
------------------------
<div class="avatar" id="avatar">
  <img src="base_head.png" class="layer base" alt="Steampunk robot head">
  <img src="eyes_open.png" class="layer eyes" id="eyesOpen" alt="Open eyes">
  <img src="eyes_closed_blink.png" class="layer eyes hidden" id="eyesClosed" alt="Closed eyes">

  <img src="mouth_closed.png" class="layer mouth" id="mouthClosed" alt="Closed mouth">
  <img src="mouth_small_open.png" class="layer mouth hidden" id="mouthSmall" alt="Small open mouth">
  <img src="mouth_medium_open.png" class="layer mouth hidden" id="mouthMedium" alt="Medium open mouth">
  <img src="mouth_wide_open.png" class="layer mouth hidden" id="mouthWide" alt="Wide open mouth">
  <img src="mouth_o.png" class="layer mouth hidden" id="mouthO" alt="O mouth">
</div>

Suggested CSS
-------------
.avatar {
  position: relative;
  width: 420px;
  aspect-ratio: 1 / 1;
}

.layer {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: contain;
  pointer-events: none;
}

.hidden {
  display: none;
}

Basic blinking logic
--------------------
- Keep eyes_open visible by default.
- Every 3 to 6 seconds, briefly swap to eyes_closed_blink.png for about 120 to 180 ms.
- Then switch back to eyes_open.

Pseudo-logic:
1. hide eyes_open
2. show eyes_closed_blink
3. wait ~150 ms
4. hide eyes_closed_blink
5. show eyes_open
6. repeat after a randomized delay

Basic talking logic
-------------------
If using TTS audio playback:
- mouth_closed.png = idle / silence
- mouth_small_open.png = low volume
- mouth_medium_open.png = medium volume
- mouth_wide_open.png = louder speech
- mouth_o.png = optional occasional variation for rounded vowel look

Simplest version:
- While speech is playing, swap mouth states every 80 to 140 ms.
- Randomize between small / medium / wide / O, but bias toward small and medium.
- When audio stops, return to mouth_closed.

Better version:
- Use Web Audio API AnalyserNode.
- Measure amplitude/volume.
- Map volume thresholds to mouth states.

Very simple mouth mapping example
---------------------------------
volume < 0.02  -> mouth_closed
volume < 0.06  -> mouth_small_open
volume < 0.12  -> mouth_medium_open
volume < 0.20  -> mouth_wide_open
else           -> mouth_o or mouth_wide_open

Implementation tips for CC
--------------------------
- Build a helper like showMouth('closed' | 'small' | 'medium' | 'wide' | 'o').
- Build a helper like setBlink(isBlinking).
- On TTS start: start mouth animation loop.
- On TTS end: stop mouth animation loop and show mouth_closed.
- Keep blinking independent from mouth animation.
- Optional polish: add a tiny idle bob with CSS transform translateY() or scale().

Minimal JS helpers (concept only)
---------------------------------
Store references to mouth image elements in an object, then hide all mouth layers and reveal only the desired one.
Do the same for the eye layers.

Recommended file assumptions
----------------------------
All files are in the same folder as index.html, OR update paths to match your assets folder.
If placing in /assets/robot/, update img src values accordingly.

Short summary for CC
--------------------
Use a square relative container. Stack all PNGs as absolute layers. Keep base_head always visible. Swap between eyes_open and eyes_closed_blink for blinking. Swap mouth_closed / mouth_small_open / mouth_medium_open / mouth_wide_open / mouth_o during playback.
