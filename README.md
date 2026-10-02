# Dragonfly Field Lab

A practice game for undergraduate organic chemistry. Students land at sites on
Titan, record 1H NMR, 13C NMR and IR spectra of a sample, propose a structure
(multiple choice or drawn), and the mass spectrometer confirms or rejects it.
Nothing is graded or sent anywhere; progress is kept only in the student's own
browser.

All three zones are built, fifteen sites in all:

- Zone 1, Shangri-La dunes: five small molecules detected in Titan's atmosphere.
- Zone 2, crater approach: aromatic rings and isomer pairs.
- Zone 3, Selk crater: an amine, a carboxylic acid, an amide and two amino acids.

Each molecule's debrief says whether it has been detected on Titan, is
predicted there, is a mission target, or is a stand-in for practice.

## Files

- `index.html` – the whole game: layout, spectra drawing, and the molecule data.
- `lib/jsme/` – JSME structure editor (drawing mode).
- `lib/rdkit/` – RDKit.js (compares a drawn structure with the answer).
- `lib/craft3d.js` and `media/dragonfly-model.js` – the 3D rotorcraft on the opening screen.
- `media/opening.webp` – the opening screen background.
- `media/site1.webp` … `site15.webp` – the site pictures on the flight plan.
- `media/zone2.mp4` and `media/zone2-poster.webp` – the arrival clip for zone 2.

Keep the folder structure as it is. The game must be served over http(s);
drawing mode will not fully work if `index.html` is opened by double-clicking.

## Putting it online

1. Copy this folder into a GitHub repository that has GitHub Pages turned on.
2. The game is then at `https://<user>.github.io/<repo>/<folder>/`.
3. In D2L, link to that address, or embed it with an iframe, for example:
   `<iframe src="https://<user>.github.io/<repo>/<folder>/" width="100%" height="900" allowfullscreen></iframe>`

## The opening video

The opening screen plays NASA's "Touchdown on Titan" animation
(credit: NASA/Johns Hopkins APL) straight from NASA's server:
https://science.nasa.gov/video-detail/dragonflyattitan2025/
That file is 4K, so it can be slow on a weak connection. If it cannot play,
the game shows a link to NASA's page instead. To change the video, edit
`VIDEO_URL` near the top of the script in `index.html`.

## The 3D rotorcraft on the opening screen

The rotorcraft is "NASA Dragonfly Quadcopter" by Emil van Dam, published on
Sketchfab under the Creative Commons Attribution 4.0 licence:
https://sketchfab.com/3d-models/nasa-dragonfly-quadcopter-fa2d23cbce6642598f5e6e6f65b8b312
It is an artist's model of the craft, not an official NASA file. The licence
allows reuse as long as the author is credited, so the credit line under the
opening scene must stay. The original was converted to glTF with its textures reduced from 4096 to
1024 pixels (about 2 MB instead of 178 MB) and stored as text in
`media/dragonfly-model.js`, so it loads like any other script.
`lib/craft3d.js` draws it (three.js is bundled inside that file).
If a browser cannot show 3D, a flat silhouette of the craft is shown instead.

## The opening picture

`media/opening.webp` is the background of the opening screen, a picture
supplied by the course author. To change it, replace that file with another
picture of the same name (about 1600 pixels wide works well). If it cannot
load, the game draws a simple dune scene instead.

## Arrival clips

A zone can have a short video that plays the first time a student lands at its
first site (zone 2 has one: `media/zone2.mp4`, supplied by the course author).
It plays over the page with a Skip button (Esc also skips), then the game
carries on to the site. Once seen, a "Replay arrival" button appears beside
that zone on the flight plan. If the file is missing or cannot play, the game
goes straight to the site.

To add one for another zone, put the file in `media/` and add
`clip:{src:"media/zone3.mp4", poster:"media/zone3-poster.webp"}` to that zone's
entry in the `ZONES` list in `index.html` (the poster is optional). Use an MP4
with H.264 video; a few megabytes is plenty for ten seconds.

## The site pictures on the flight plan

`media/site1.webp` to `site5.webp` are illustrations painted for this game by a
small terrain-drawing script (dunes, ripples, cobbles, haze and a dim sun).
They are artistic impressions, not photographs or data, and the flight plan
page says so. Each site's picture and one-line description are set by `img`
and `view` in its `SITES` entry in `index.html`.

## Where the spectral data come from

- 1H and 13C shifts for ethane, propane, benzene, acetonitrile and toluene:
  Fulmer et al., Organometallics 2010, 29, 2176 (values in CDCl3).
- NMR shifts for propanenitrile, ethylbenzene, 1,4-dimethylbenzene, butanenitrile,
  2-methylpropanenitrile and all five zone 3 molecules, all IR band positions,
  and all mass-spectrum peak heights were entered from memory of standard
  reference spectra and should be checked against SDBS or the NIST WebBook
  before students rely on them.
- The two amino acids are shown as they would appear in D2O (hydrogens on N and
  O not seen), with IR spectra of the solid, which is made of ions. Their shifts
  depend on pH, so treat them as typical values.
- Hydrogens on N and O in the amine, acid and amide are drawn as broad peaks.
- The aromatic hydrogens of toluene and ethylbenzene are drawn as three
  overlapping first-order patterns, which gives a realistic-looking multiplet
  but not an exact one.
- The 1H spectrum carries an integration line (the green stepped curve), and
  each step is labelled with its number of hydrogens ("3H"). To remove those
  labels, so students must work out the ratio from the step heights, set
  `SHOW_H` to `false` near the top of the script in `index.html`. The signal
  buttons show only the chemical shift; `SHOW_AREAS` set to `true` adds relative
  areas to them. The collapsed "Peak list as text" under each spectrum gives the
  same numbers as the plot, for students who cannot read it.
- Spectra are drawn from these numbers. Multiplets are first-order at 400 MHz;
  IR band shapes are simplified.

## Adding or editing a molecule

Each sample is one entry in the `SITES` list in `index.html`:

- `h1`: `[shift, number of H, number of neighbouring H, J in Hz]` per signal
- `c13`: `[shift, relative height]` per peak
- `ir`: `[wavenumber, depth 0–1, width]` per band
- `ms`: `[m/z, relative %]` per peak
- `options`: the four multiple-choice structures, each with a SMILES string,
  nominal mass, and a `why` sentence shown when that wrong answer is proposed.
  The order in this list does not matter: the game shuffles the four for each
  student, and deals the position of the right answer so that it falls in each
  of the four places about equally often. A site keeps its order until the
  mission is restarted
- `smiles` / `jsme`: the answer (the second is the editor's own canonical form,
  used only if RDKit cannot load)
- 1H signals that share a fifth value (for example `"ar"`) are shown to the
  student as one overlapping multiplet; a sixth value makes a peak broad
  (its width in Hz), for hydrogens on N or O
- `h1note` adds a sentence under the 1H spectrum (used for the D2O samples)
- an option shows either `f` (a typed formula) or, with `draw:1`, a ring drawing
  taken from the `STRUCT` list; `gen_struct.js` in the project sources made those
- `ZONES` lists which sites belong to which zone

## Credits

Not affiliated with NASA or Johns Hopkins APL. See `THIRD-PARTY-NOTICES.md`
for the licences of JSME, RDKit, three.js and the 3D model.
