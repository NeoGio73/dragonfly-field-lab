# Dragonfly Field Lab

A practice game for undergraduate organic chemistry. Students land at sites on
Titan, record 1H NMR, 13C NMR and IR spectra of a sample, propose a structure
(multiple choice or drawn), and the mass spectrometer confirms or rejects it.
Nothing is graded or sent anywhere; progress is kept only in the student's own
browser.

Zone 1 (five small molecules detected in Titan's atmosphere) and zone 2 (five
larger molecules: aromatic rings and isomer pairs) are built. Zone 3 is a
placeholder on the site map. Each molecule's debrief says whether it has been
detected on Titan, is predicted there, or is a stand-in for practice.

## Files

- `index.html` – the whole game: layout, spectra drawing, and the molecule data.
- `lib/jsme/` – JSME structure editor (drawing mode).
- `lib/rdkit/` – RDKit.js (compares a drawn structure with the answer).
- `lib/craft3d.js` and `media/dragonfly-model.js` – the 3D rotorcraft on the opening screen.
- `media/site1.webp` … `site5.webp` – the site pictures on the flight plan.

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

## The site pictures on the flight plan

`media/site1.webp` to `site5.webp` are illustrations painted for this game by a
small terrain-drawing script (dunes, ripples, cobbles, haze and a dim sun).
They are artistic impressions, not photographs or data, and the flight plan
page says so. Each site's picture and one-line description are set by `img`
and `view` in its `SITES` entry in `index.html`.

## Where the spectral data come from

- 1H and 13C shifts for ethane, propane, benzene, acetonitrile and toluene:
  Fulmer et al., Organometallics 2010, 29, 2176 (values in CDCl3).
- NMR shifts for propanenitrile, ethylbenzene, 1,4-dimethylbenzene, butanenitrile
  and 2-methylpropanenitrile, all IR band positions, and all mass-spectrum peak
  heights were entered from memory of standard reference spectra and should be
  checked against SDBS or the NIST WebBook before students rely on them.
- The aromatic hydrogens of toluene and ethylbenzene are drawn as three
  overlapping first-order patterns, which gives a realistic-looking multiplet
  but not an exact one.
- The 1H spectrum carries an integration line (the green stepped curve), and the
  signal buttons show only the chemical shift, so students read the relative
  areas from the steps. To show "area" numbers on the buttons instead, set
  `SHOW_AREAS` to `true` near the top of the script in `index.html`. The
  collapsed "Peak list as text" under each spectrum always gives the areas, for
  students who cannot read the plot.
- Spectra are drawn from these numbers. Multiplets are first-order at 400 MHz;
  IR band shapes are simplified.

## Adding or editing a molecule

Each sample is one entry in the `SITES` list in `index.html`:

- `h1`: `[shift, number of H, number of neighbouring H, J in Hz]` per signal
- `c13`: `[shift, relative height]` per peak
- `ir`: `[wavenumber, depth 0–1, width]` per band
- `ms`: `[m/z, relative %]` per peak
- `options`: the four multiple-choice structures, each with a SMILES string,
  nominal mass, and a `why` sentence shown when that wrong answer is proposed
- `smiles` / `jsme`: the answer (the second is the editor's own canonical form,
  used only if RDKit cannot load)
- 1H signals that share a fifth value (for example `"ar"`) are shown to the
  student as one overlapping multiplet
- an option shows either `f` (a typed formula) or, with `draw:1`, a ring drawing
  taken from the `STRUCT` list; `gen_struct.js` in the project sources made those
- `ZONES` lists which sites belong to which zone

## Credits

Not affiliated with NASA or Johns Hopkins APL. See `THIRD-PARTY-NOTICES.md`
for the licences of JSME, RDKit, three.js and the 3D model.
