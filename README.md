# Dragonfly Field Lab

A practice game for undergraduate organic chemistry. Students land at sites on
Titan, record 1H NMR, 13C NMR and IR spectra of a sample, propose a structure
(multiple choice or drawn), and the mass spectrometer confirms or rejects it.
Nothing is graded or sent anywhere; progress is kept only in the student's own
browser.

Zone 1 (five small molecules detected in Titan's atmosphere) is built.
Zones 2 and 3 are placeholders on the site map.

## Files

- `index.html` – the whole game: layout, spectra drawing, and the molecule data.
- `lib/jsme/` – JSME structure editor (drawing mode).
- `lib/rdkit/` – RDKit.js (compares a drawn structure with the answer).
- `lib/craft3d.js` and `media/dragonfly-model.js` – the 3D rotorcraft on the opening screen.

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

## Where the spectral data come from

- 1H and 13C shifts for ethane, propane, benzene and acetonitrile: Fulmer et al.,
  Organometallics 2010, 29, 2176 (values in CDCl3).
- Propanenitrile NMR shifts, all IR band positions, and all mass-spectrum peak
  heights were entered from memory of standard reference spectra and should be
  checked against SDBS or the NIST WebBook before students rely on them.
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

## Credits

Not affiliated with NASA or Johns Hopkins APL. See `THIRD-PARTY-NOTICES.md`
for the licences of JSME, RDKit, three.js and the 3D model.
