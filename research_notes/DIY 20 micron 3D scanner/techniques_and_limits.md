# 3D scanning techniques that can realistically reach 0.02 mm in a DIY low-budget build, and the physics/metrology limits

Researcher note on evidence quality: searches were run 2026-11-11. Formulas quoted from papers are cited. All worked numeric examples are my own arithmetic from those formulas and are placed under "Inferences". Several things the brief asked for could not be sourced in this session (listed under "Gaps"); nothing there should be treated as verified.

## 1. Resolution vs point spacing vs accuracy vs precision vs repeatability; what standards and commercial scanners quote; what "0.02 mm" scanners deliver and cost

### Takeaway
"0.02 mm" on consumer spec sheets is almost always single-frame precision (noise/repeatability) or point spacing, not accuracy; the one brochure read in full (Revopoint MINI 2) states 0.02 mm precision but 0.05 mm accuracy from a 2 MP depth camera. Verified 0.02 mm-class accuracy (VDI/VDE 2634 / ISO 12836 style) starts at roughly USD 20k+ (Artec Micro II: 5 µm, USD 22.5k-33k); no independent artifact-based test of any sub-USD-2k scanner at 0.02 mm was found.

### Cited Findings
- Accuracy = closeness to true value; precision = repeatability of repeated scans; vendors quoting "resolution" generally mean mesh resolution, i.e. distance between points in the point cloud, which does not by itself make measurements more accurate — [Artec learning center](https://www.artec3d.com/learning-center/accuracy-precision-resolution); [FacFox summary](https://facfox.com/docs/kb/3d-scanner-accuracy-and-resolution-the-basics)
- Single-scan accuracy describes one capture; volumetric accuracy covers multiple aligned captures and degrades with part size. Spec format "0.02 mm + 0.04 mm/m" = fixed term plus length-dependent term — [Scantech](https://www.3d-scantech.com/how-accurate-is-a-handheld-3d-scanner-/); [Shining 3D guide (vendor)](https://www.shining3d.com/the-ultimate-guide-for-measurement-accuracy?hsLang=it)
- VDI/VDE 2634 has Part 1 (point-by-point probing), Part 2 (area-scanning optical systems), Part 3 (multiple-view area scanning) — [VDI 2634-3 notes](https://www.cnblogs.com/gshang/articles/18353350)
- Probing error (form) = range of residuals from a least-squares sphere fit to all points on a calibrated sphere (ISO 10360 analogue definition); sphere-spacing error = measured minus calibrated distance between two sphere centres, and is how VDI/VDE 2634-2 defines accuracy — [AT Sensors on ISO 10360-10](https://www.at-sensors.com/measurement/geometric-product-specification-din-en-iso-10360-10/); [wenglor accuracy page](https://www.wenglor.com/en/Accuracy/l/cxmCID198205)
- A 2022 study found ISO 10360-13 length tests are more sensitive to camera-model errors than the VDI/VDE 2634-2 tests (i.e. passing sphere tests does not guarantee low length-scale error) — [Precision Engineering 2022](https://www.sciencedirect.com/science/article/abs/pii/S0141635922002318)
- Revopoint MINI 2 brochure (Dec 2024 revision): dual-camera blue structured light; "single-frame precision up to 0.02 mm"; "single-frame accuracy up to 0.05 mm"; fused point distance up to 0.02 mm; working distance 120-250 mm; single capture area 52 x 64 mm at 120 mm and 168 x 132 mm at 250 mm; min scan volume 10x10x10 mm; depth camera 2 MP; footnote: precision and accuracy "acquired in a controlled lab environment. Actual results might vary" — [Revopoint MINI 2 brochure PDF](https://dl.revopoint3d.com/brochure/mini2/mini2-brochure-en.pdf)
- No independent calibrated-artifact test of the MINI 2 at 0.02 mm was found; reviews found were sample-supplied and report no metrology figure — [The Gadgeteer review](https://the-gadgeteer.com/2024/03/13/revopoint-mini-2-3d-scanner-review-copy-and-paste-irl/); [3DSourced review](https://www.3dsourced.com/3d-scanner-reviews/revopoint-mini-2-review/)
- Creality CR-Scan Otter: rated 0.02 mm; one single-author review says it tested near that; same review lists price inconsistently as USD 469 and USD 719 — [3DTechValley Otter review](https://www.3dtechvalley.com/creality-cr-scan-otter-review/)
- Creality CR-Scan Raptor: blue laser mode rated 0.02 mm, NIR mode 0.05 mm; one reviewer measured a test part within 0.03-0.04 mm on most surfaces (single reviewer, method not metrological) — [3DTechValley Raptor review](https://www.3dtechvalley.com/creality-cr-scan-raptor-review/); whole-object accuracy relies on stick-on markers — [3dprinting.com guide](https://3dprinting.com/scanners/best-3d-scanners-for-large-objects/)
- Creality Raptor Pro listed at USD 1,699 with "0.02 mm accuracy" claim (deal listing, not a test) — [Slickdeals listing](https://slickdeals.net/f/18138073-creality-raptor-pro-3d-scanner-22-cross-laser-lines-and-infrared-structured-light-scanning-0-02mm-accuracy-0-2-157-object-size-range-1699)
- Einstar (Shining 3D): sources conflict, 0.1 mm in one guide vs "up to 0.05 mm" in a review; Einstar Vega titled 0.05 mm with whole-mesh accuracy dependent on alignment/markers — [3dprinting.com](https://3dprinting.com/scanners/best-3d-scanners-for-medium-sized-items/); [3DTechValley Vega review](https://www.3dtechvalley.com/einstar-vega-3d-scanner-review/)
- Artec Micro II: 3D point accuracy 5 µm, repeatability 2 µm, ISO 12836 accuracy certification; reseller prices USD 22,500, 29,300 and 32,940 (bundle) — [MatterHackers](https://www.matterhackers.com/store/l/artec-micro-ii-3d-scanner); [Ultimate 3D Printing Store](https://ultimate3dprintingstore.com/a/s/products/artec-micro-ii-3d-scanner); [Artec prices page](https://www.artec3d.com/prices)
- Older reference point: NextEngine HD desktop laser scanner (approx. USD 3,000) was the subject of a peer-reviewed uncertainty/repeatability analysis — [Sensors 2012, PMC3444090](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3444090/)
- A handheld structured-light scanner with manufacturer-certified VDI/VDE 2634 volumetric accuracy of 0.03 mm was used in a multi-site study (figure is the manufacturer's) — [Sensors 2024](https://www.mdpi.com/1424-8220/24/7/2350)

### Inferences
- MINI 2 native sampling: a 2 MP depth camera (assume ~1600 x 1200) over the 52 x 64 mm near field is ~40 µm per pixel. The quoted "0.02 mm fused point distance" is therefore finer than the native pixel footprint, i.e. it is an interpolated/fused mesh spacing, not optical resolution. The 0.02 mm headline is the noise figure; the stated accuracy is 2.5x worse.
- Pattern across the consumer class: 0.02 mm = precision or best-case single-frame; accuracy claims that are stated separately are 0.05 mm; the only hands-on numbers found (0.03-0.04 mm on a Raptor) are single-reviewer, not sphere/length-artifact tests. Treat 0.02 mm at this price tier as marketing-grade until tested with an artifact.
- The price gap between "claims 0.02 mm" (USD ~500-1,700) and "certified 5 µm" (USD 22k+) is about 15-60x.
- For a DIY builder the honest target to state is: repeatability (noise) of ~5-20 µm is easy to demonstrate; accuracy of 20 µm needs a calibrated artifact (gauge blocks, grade-rated spheres, ball bar) and a length-scale check, not just a sphere fit.

### Gaps
- Could not retrieve official VDI/VDE 2634 text; definitions above come from secondary summaries.
- No source found in-session for current pricing/specs of Revopoint MINI 2 (price), Shining 3D desktop units (e.g. EinScan-SP/Transcan), GOM/ZEISS ATOS Q, or dental lab scanners (typ. quoted single-digit µm under ISO 12836). These searches returned nothing usable; do not quote figures for them from these notes.
- No peer-reviewed or lab test applying VDI/VDE 2634 sphere tests to Revopoint, Creality or Einstar units was found.

## 2. Camera-based triangulation limits: pixel footprint, baseline/angle, sub-pixel phase, distortion, depth of field, diffraction (formulas and worked examples)

### Takeaway
Random depth noise is not the binding limit: published phase noise for N-step fringe projection is of order 1/750 of a fringe period, so single-digit-micron noise is routine. What limits a 0.02 mm build is (a) lateral sampling (20 µm features need roughly <=10 µm/pixel, so FOV shrinks to about 40-55 mm wide for 12-20 MP), (b) the diffraction/depth-of-field trade (about 0.6-2.4 mm of sharp depth at true 20 µm optical resolution), and (c) systematic calibration, gamma and thermal errors.

### Cited Findings
- Fringe-projection phase noise (N-step, camera noise var = Cn + K*I): sigma_phi^2 = 2(Cn + K*Ac)/(N*Bc^2); near-saturated fringes: sigma_phi^2 ~ 4/(N * mu_e,sat) (full-well electrons). General depth noise: sigma_z = (T/2pi) * |dz/du_p| * sigma_phi (T = fringe period in projector pixels) — [Modeling the measurement precision of FPP, PMC10613632](https://pmc.ncbi.nlm.nih.gov/articles/PMC10613632/)
- Same paper, simplified epipolar case (as reconstructed by the fetch tool; exponents should be checked against the PDF): sigma_z ~ (T/2pi) * z^2/(f_p * b) * sigma_phi, i.e. depth noise grows with distance squared and falls with baseline b and projector focal length — [PMC10613632](https://pmc.ncbi.nlm.nih.gov/articles/PMC10613632/)
- Measured values in that paper: 5 MP Sony IMX250 mono camera (2448 x 2048), full well ~10.3 ke-, 9-step phase shift, 21-px period: phase variance 6.9e-5 rad^2; 2x2 binning cut variance ~3.5x; projector needed a 30-minute warm-up; optimum fringe period ~5 px focused, ~20 px defocused; depth precision estimated ~70 µm at 900 mm falling to ~5 µm at ~240 mm working distance, with a small 7-9 degree camera-projector angle (DLP6500/DLP4500) — [PMC10613632](https://pmc.ncbi.nlm.nih.gov/articles/PMC10613632/)
- Phase-to-depth sensitivity depends on system geometry and fringe direction; best when fringes are perpendicular (90 degrees) to the epipolar line — [Meza et al. 2020](https://jhacsonmeza.github.io/papers/triangulation_methods_2020.pdf)
- Fundamental triangulation limit on rough surfaces: sigma_z cannot beat SC*lambda/(2pi sin^2 u) (SC speckle contrast, sin u aperture); the term lambda/sin^2 u is the Rayleigh depth of field, so precision and depth of field are tied through the aperture. Example: NA 0.15, diffraction-limited: ~2 µm on rough surfaces, ~200 nm on smooth surfaces at SNR ~100 — [Häusler group, arXiv:1504.04556](https://arxiv.org/pdf/1504.04556)
- Same paper: classic fringe projection "lacks depth of field for high lateral resolution requirements" and generally cannot measure specular surfaces — [arXiv:1504.04556](https://arxiv.org/pdf/1504.04556)
- Temperature: business and dev-kit projectors do not compensate thermal drift of the displayed image, which directly affects structured-light scanner stability — [Sensors 2020, PMC7600602](https://pmc.ncbi.nlm.nih.gov/articles/PMC7600602)
- Low-cost DLP vs laser (MEMS) pico projector for fringe projection with a Raspberry Pi camera: laser projector had longer depth of field and needed less gamma correction (no accuracy figure given) — [Univ. Nottingham poster, 3DMC 2018](https://www.nottingham.ac.uk/research/groups/advanced-manufacturing-technology-research-group/documents/manufacturing-metrology-team/mmt-posters/3dmc-2018-hamburg.pdf)
- Microscopic fringe projection (small FOV, telecentric or microscope optics) is an established field with accuracy/FOV/speed trade-offs reviewed by Hu, Chen, Feng, Zuo (2020); telecentric lenses need special calibration methods and targets — [review record](https://ouci.dntb.gov.ua/works/42jwyD84); [Pistellato et al. 2022](https://www.scitepress.org/Papers/2022/108417/108417.pdf)

### Inferences
Worked examples (my arithmetic; standard optics relations, not taken from a single cited source):

- Lateral sampling vs FOV. FOV = pixels x object-side pixel size.
  | Camera | 20 µm/px (point spacing = 20 µm) | 10 µm/px (Nyquist for 20 µm detail) | 6.7 µm/px (3 px per 20 µm) |
  |---|---|---|---|
  | 12 MP, 4000 x 3000 | 80 x 60 mm | 40 x 30 mm | 27 x 20 mm |
  | 20 MP, 5472 x 3648 | 109 x 73 mm | 55 x 36 mm | 36 x 24 mm |
  | 2 MP, 1600 x 1200 | 32 x 24 mm | 16 x 12 mm | 11 x 8 mm |
  A Bayer colour sensor loses roughly a further 1.4-2x in real resolution versus mono; structured-light decoding windows and phase filtering also smooth laterally. So "20 µm point spacing" is reachable over ~80-110 mm, but resolving 20 µm features needs ~30-55 mm FOV.
- Depth noise from phase noise. For a camera-projector pair with triangulation angle theta and fringe period p on the object: sigma_z ~ p * sigma_phi / (2pi * tan theta). Using the measured sigma_phi = sqrt(6.9e-5) = 0.0083 rad (= period/756): p = 1 mm, theta = 20 deg gives sigma_z ~ 3.6 µm; p = 0.5 mm, theta = 30 deg gives ~1.1 µm. Even a 10x worse phase noise (cheap 8-bit rolling-shutter camera, 3-4 steps) gives ~10-35 µm. Conclusion: noise-wise 0.02 mm depth is achievable with cheap parts at FOV <= ~100 mm; noise is not where the budget goes.
- Depth noise scales as z^2/(f*b): halving working distance at fixed baseline and lens quarters the depth noise, which is why every 0.02 mm-class device is a short-range, small-FOV device (consistent with the 70 µm at 900 mm vs 5 µm at 240 mm figures above).
- Diffraction vs depth of field (object side, NA = m/(2*N*(1+m)) for magnification m and f-number N): Airy radius r = 0.61*lambda/NA; Rayleigh depth of field ~ lambda/NA^2 = r^2/(0.372*lambda).
  - For r = 20 µm: blue 450 nm needs NA 0.0137 and gives DoF ~2.4 mm; red 650 nm needs NA 0.0198 and gives DoF ~1.65 mm.
  - For Airy diameter = 20 µm (stricter): blue needs NA 0.027, DoF ~0.6 mm.
  - Example lens setting: 2.4 µm sensor pixels imaging at 12 µm/px (m = 0.2) at f/8 gives effective f/9.6, NA 0.0104, object-side Airy diameter ~53 µm (about 4.4 pixels): the lens, not the sensor, limits resolution. Opening to f/4 gives ~26 µm Airy diameter but only ~1 mm of depth of field.
  - So at genuine 20 µm lateral resolution, the in-focus slab is only ~0.6-2.4 mm thick. Objects deeper than that need focus stacking, Scheimpflug tilt, z-stepping with multiple scans, or acceptance that 20 µm applies to depth noise only and lateral resolution is ~40-60 µm.
- Blue vs red (incoherent LED/DLP): at equal lateral resolution blue gives ~1.44x more depth of field (ratio 650/450); at equal aperture it gives 1.44x finer diffraction spot. Blue also penetrates less into many plastics/organics (see section 5), which is the main reason commercial units use it. This penetration statement is general knowledge and was not sourced in this session.
- Lens distortion/calibration: a residual of 0.1 px at 10 µm/px is 1 µm laterally, but projected into depth it is multiplied by 1/tan(theta) and by any scale error over the volume. A calibration target must itself be known to a few µm (a laser-printed checkerboard is not; printers have ~0.1-0.5 % scale error, i.e. 50-250 µm over 50 mm). This is the dominant accuracy (not precision) term for DIY builds and is unsourced reasoning.
- Gamma/nonlinearity of consumer projectors produces periodic phase error; mitigations are more phase steps, defocused binary patterns, or a pre-calibrated LUT (the Nottingham poster above confirms gamma differs by projector type).

### Gaps
- Did not retrieve a Song Zhang paper directly; the precision model above (PMC10613632) is from a different group. The specific closed-form "sigma_z vs phase noise and angle" expression I use in the worked examples is the standard small-angle geometry, not quoted verbatim from a source.
- No sourced numbers found for lens distortion residuals of cheap M12/C-mount lenses or for achievable sub-pixel accuracy with rolling-shutter Raspberry Pi cameras.
- The numeric accuracy/FOV tables in the Hu et al. microscopic FPP review were not accessible.

## 3. Laser speckle limits: laser line triangulation vs LED/DLP projection; blue vs red

### Takeaway
Laser triangulation has a hard physical noise floor from speckle, delta_z = C*lambda/(2pi * sin(u_obs) * sin(theta)), which for the small apertures needed for depth of field lands at roughly 7-20 µm per point; incoherent LED/DLP fringe projection largely escapes this (low speckle contrast, photon-noise limited). Blue lasers lower the floor by about the wavelength ratio (~30 %).

### Cited Findings
- Dorsch, Häusler, Herrmann (1994): speckle-limited uncertainty of laser triangulation delta_z = C*lambda / (2pi * sin u_obs * sin theta), with C speckle contrast (C = 1 for a laser on a rough surface), u_obs observation aperture, theta triangulation angle — [Hand-guided light sectioning paper, arXiv:1401.1946](https://arxiv.org/pdf/1401.1946); original abstract at [FAU CRIS](https://cris.fau.de/publications/110424204)
- The uncertainty is independent of projected spot size (if spot > speckle); it is "a couple of micrometers to more than 10 µm" for short-range macroscopic sensors; remedies: low temporal coherence, low spatial coherence, large observation aperture (which costs depth of focus) — [FAU CRIS abstract](https://cris.fau.de/publications/110424204)
- For structured illumination on rough surfaces the same generic term SC*lambda/sin^2 u appears; measured speckle contrast in their incoherent setup was ~0.6 and speckle between phase-shifted images is highly correlated (so much of it cancels in the phase calculation) — [arXiv:1504.04556](https://arxiv.org/pdf/1504.04556)
- Microscopic laser triangulation on technical surfaces has been studied specifically for speckle-driven uncertainty — [Microscopy and Microanalysis](https://www.cambridge.org/core/journals/microscopy-and-microanalysis/article/measurement-uncertainty-of-microscopic-laser-triangulation-on-technical-surfaces/B072D794AE8D23889A571D6E9DAC33A3)
- A custom laser line scanner checked against a CMM on a 1000 µm step: CMM 1002.7 +/- 0.1 µm, scanner 1066.0 +/- 0.4 µm, i.e. sub-micron repeatability but 63 µm (6 %) systematic error — [euspen ICE16 P1.65](https://www.euspen.eu/knowledge-base/ICE16-P1.65.pdf)
- For low-cost webcam plus handheld line-laser builds, accuracy depends on sub-pixel extraction of the laser line centre; line width affects accuracy — [monocular scanner paper summary](https://www.bohrium.com/paper-details/building-a-3d-scanner-system-based-on-monocular-vision/813321724564078593-4259)

### Inferences
- Worked speckle floor (my arithmetic from the Dorsch formula, C = 1, theta = 30 deg):
  | Object-side NA (example) | Red 650 nm | Blue 450 nm | Blue 405 nm |
  |---|---|---|---|
  | 0.0104 (f/8 at m = 0.2) | 19.9 µm | 13.8 µm | 12.4 µm |
  | 0.021 (f/4 at m = 0.2) | 9.9 µm | 6.8 µm | 6.1 µm |
  | 0.05 (macro, f/2.8 near 1:2.5) | 4.1 µm | 2.9 µm | 2.6 µm |
  These are 1-sigma per-point noise before any averaging. A cheap red line laser with a stopped-down lens is therefore right at or above 20 µm noise per point; reaching well below it needs blue, a wide aperture (thin depth of field), a larger triangulation angle (more occlusion), or speckle averaging (moving the line/part slightly and averaging, since independent speckle patterns average as 1/sqrt(n)).
- Blue vs red laser: floor scales linearly with lambda, so 450 nm is 0.69x and 405 nm is 0.62x of 650 nm. This matches the marketing switch to "blue laser" in the Raptor-class devices, but the physical gain is ~30-40 %, not an order of magnitude.
- LED/DLP projection: spatially and temporally incoherent source, so speckle contrast is low and the common-mode remainder cancels in phase shifting; the limit becomes photon noise (section 2), which is 5-10x lower than laser speckle at the same aperture. On physics alone, LED/DLP fringe projection is the better route to 0.02 mm for a static, tethered DIY rig; laser line wins only on ambient-light immunity, dark/shiny surfaces and simplicity.
- The euspen result is a clean illustration of the resolution/precision vs accuracy distinction: 0.4 µm repeatability with 63 µm bias. A DIY laser scanner can look "micron-precise" and still be 3x outside a 0.02 mm accuracy target because of calibration/scale error.
- Laser-MEMS pico projectors (as in the Nottingham comparison) are coherent sources and would be expected to carry speckle noise; this was not quantified in the poster.

### Gaps
- No sourced measurement of speckle contrast for cheap DLP/LED pico projectors or for hobby line-laser modules.
- No sourced quantitative comparison of blue vs red for subsurface scattering in plastics/teeth in this session.
- The full text of Dorsch et al. 1994 was not retrieved; formula is from papers that quote it.

## 4. Published DIY / low-cost results at 10-50 µm and how they were verified

### Takeaway
The best-documented hobbyist result is OpenScan Mini photogrammetry: ~20 +/- 0.03 mm on a 20 mm steel sphere with surface deviation 2-sigma ~0.03 mm, self-tested and self-scaled, with the author retracting an earlier "0.008 mm" claim. I found no DIY structured-light or laser build with artifact-verified 20 µm accuracy; low-cost academic builds found report 0.5 mm-class or mm-class accuracy, and micron-class results come from focus-stacked macro photogrammetry on mm-to-cm fields checked against interferometry.

### Cited Findings
- OpenScan Mini (Raspberry Pi photogrammetry turntable, cross-polarised ring light): two chrome steel spheres (20 mm, 25 mm; vendor tolerance +/-0.0025 mm, not lab-certified), coated with AESUB Orange; 150 photos per set, 8 sets, ~11 min capture each, 20 +/- 2 C; scaled using the 25 mm sphere as reference; sphere fits in GOM Inspect on 60-80 % of the surface. Results: radius 9.985 mm vs 10.000 expected; reference sphere 12.493 vs 12.500; surface deviation 2-sigma = 0.024 and 0.028 mm. Author: all values read low so scaling is slightly off; "not yet confident enough to make a valid accuracy statement"; earlier 0.008 mm claim called "click-baity" — [OpenScan blog, Chasing Microns part 2](https://blog.openscan.eu/posts/chasing-microns-openscan-mini-accuracy-test-part-2/)
- Secondhand report of the developer's figures: sub-50 µm with Raspberry Pi Camera v2.1 and ~10 µm with the Pi HQ camera — [Hackster.io](https://www.hackster.io/news/thomas-megel-s-openscan-offers-10-micron-3d-scanning-on-a-raspberry-pi-and-hq-camera-module-308a2540c5b8)
- Photogrammetry output has no inherent scale; a reference measurement is always needed — [OpenScan docs](https://openscan-org.github.io/OpenScan-Doc/photogrammetry/quality)
- Focus-stacking + structure-from-motion with consumer camera gear, validated against white-light interferometry on a rock fracture surface; error standard deviation in the micrometre range, with errors concentrated in features of wavelength < ~150 µm and amplitude < ~10 µm (exact figure garbled in search snippet; check paper) — [Olkowicz et al., TU Delft record](https://research.tudelft.nl/en/publications/focus-stacking-photogrammetry-for-micro-scale-roughness-reconstru)
- Focus-stacked macro photogrammetry for 1-3 cm inscriptions — [Ravanelli et al. 2022](https://cris.unibo.it/bitstream/11585/855790/4/Ravanelli_Lastilla_Ferrara_High_resolution.pdf); validated against a triangulation laser scanner for small artefacts — [Clini et al., ISPRS 2016](https://isprs-archives.copernicus.org/articles/XLI-B5/229/2016/); blur-masking alternative with similar accuracy to stacking and faster, vs metrology laser reference — [ISPRS Annals 2025](https://isprs-annals.copernicus.org/articles/X-M-2-2025/89/2025/isprs-annals-X-M-2-2025-89-2025.pdf); macro-lens calibration and a focus-stack camera model — [GEOMAR MaCal](https://www.geomar.de/fileadmin/content/forschen/fb2/mg/omv/macrocalibration.pdf)
- Low-cost DLP + two-camera stereo prototype: RMS accuracy below 0.5 mm — [UPC CD6](https://www.cd6.upc.edu/content/cd6_upload/contribucions/2014011611251750619746952d7b38de39c5.pdf)
- Open-source ETH "SL Sensor" structured-light unit: mm-level accuracy, validated against a GOM ATOS Core 300 — [arXiv:2201.09025](https://arxiv.org/pdf/2201.09025); [GitHub](https://github.com/ethz-asl/sl_sensor)
- DIY-oriented open resources without reported accuracy: Brown "Build Your Own 3D Scanner" (MATLAB/C++/OpenCV code) — [mesh.brown.edu](http://mesh.brown.edu/byo3d/source.html); Raspberry Pi + DLP LightCrafter 4500 fringe projection project — [GitHub](https://github.com/samvrocks/3D-dlp-scanner-system)
- Verification practice: a study of Go!SCAN 20/50 structured-light scanners used Grade 1 steel gauge blocks dulled with microfine talc in a trace of lanolin — [arXiv:1910.13199](https://arxiv.org/pdf/1910.13199)
- Photometric stereo, low-cost DSLR + 4 LED accessory: typical height error < 3 mm on a 50 mm object — [Scientific Reports, PMC4460880](https://pmc.ncbi.nlm.nih.gov/articles/PMC4460880)
- Photometric stereo is capable of recovering surface shape at micrometre scale (relief/texture), per a mobile near-light framework paper (no DIY accuracy figure) — [Northwestern Comp Photo Lab](https://compphotolab.northwestern.edu/wordpress/wp-content/uploads/2017/09/mobile_sfs_shape_compress_18.5MB.pdf)
- Low-cost 3D-printed motorised positioner for combined multifocus (shape-from-focus) and photometric stereo of mm-scale objects under a low-power microscope (no accuracy figure in excerpt) — [SciTePress 2022](https://www.scitepress.org/Papers/2022/109135/109135.pdf)
- Shape-from-focus accuracy depends on how well the focus measure can be estimated (needs surface texture) — [camera reconstruction notes](https://bec.ac.in/pdf3/7.%20camera%20reconstruction.pdf)
- Confocal microscopy and white-light interferometry meet ~1 µm precision / ~5 µm lateral / ~1 mm depth specs but need many z-steps (e.g. 50 steps x 4 phases for 500 µm depth at NA 0.15 in structured-illumination microscopy) — [arXiv:1504.04556](https://arxiv.org/pdf/1504.04556)

### Inferences
Technique comparison for a hobbyist at 0.02 mm (synthesis of this document; cost bands are my estimates, not sourced):

| Technique | Depth noise achievable | Accuracy evidence found | Limiter | DIY cost band | Fit for 0.02 mm |
|---|---|---|---|---|---|
| Fringe projection / phase shift (DLP or LED projector + 1-2 cameras) | ~1-10 µm at <=100 mm FOV (noise model, sec. 2) | None verified for DIY; lab systems ~5 µm at 240 mm | Calibration target quality, projector gamma and thermal drift, depth of field | USD ~150-500 (pico projector, 1-2 machine-vision or Pi HQ cameras, lenses) | Best physics; accuracy depends entirely on calibration discipline |
| Gray code only | ~1 projector pixel, no sub-pixel phase | none found | projector pixel footprint | same hardware | Use only for unwrapping, combined with phase shift |
| Laser line triangulation | ~7-20 µm speckle floor per point (sec. 3) | 0.4 µm repeatability but 63 µm bias in one custom build | speckle, line-centre extraction, motion-stage accuracy | USD ~50-200 plus a linear/rotary stage | Marginal; stage error and speckle both land at the target |
| Photogrammetry on turntable (OpenScan class) | surface 2-sigma ~0.03 mm at 20 mm object | Self-tested: ~0.03 mm on 20 mm sphere | texture needed, scale bar accuracy, depth of field | USD ~100-300 | Demonstrated at 0.03 mm class for ~20-50 mm objects; cheapest proven route |
| Macro photogrammetry + focus stacking | micrometre-range SD vs interferometry | Verified vs WLI (rock surface, cm-scale) | capture time (hundreds-thousands of frames), calibration of macro lens | USD ~200-600 incl. used macro lens and rail | Reaches and beats 0.02 mm on cm-scale parts; slow |
| Photometric stereo | micron-scale relief sensitivity | <3 mm absolute error over 50 mm | integrates normals so low-frequency shape drifts | USD ~30-100 | Not a metrology method alone; good as a detail layer fused onto triangulation data |
| Shape from focus | of order depth of field/10 (general rule, unsourced) | no DIY figure found | needs texture and high NA, hence mm-scale FOV | USD ~100-300 (microscope objective + z-stage) | Only for mm-scale parts |
| Confocal / chromatic confocal / WLI | sub-µm to ~1 µm | industrial | point or small-field, slow, expensive optics | not low-budget (no DIY evidence found) | Out of scope for a few hundred USD |

- The verified hobbyist evidence clusters at ~0.03 mm on ~20 mm objects (photogrammetry), not 0.02 mm. A fringe-projection build should beat that on noise but nobody was found publishing an artifact-verified DIY result, so 0.02 mm accuracy for a home-built structured-light scanner is plausible on physics but unproven in the public record I could find.
- Verification recipe implied by the sources: (1) flat reference (plate/gauge block face) for noise and flatness; (2) grade-rated spheres for probing error (sphere-fit residual range); (3) two spheres or a gauge-block step at known distance for length/sphere-spacing error; (4) repeat over days and after warm-up for repeatability; dull shiny references with a thin coating and account for its thickness. Self-scaling from the same artifact used for testing (as OpenScan did) hides scale error.

### Gaps
- No Hackaday / r/3DScanning / YouTube build with a calibrated-artifact accuracy test was surfaced by the searches; this is absence of search hits, not proof none exist.
- OpenScan camera model and total hardware cost for the sphere test were not stated in the post.
- Exact error figures of the Olkowicz et al. focus-stacking paper not retrieved.
- No DIY chromatic confocal or low-cost confocal result found; no sourced accuracy for DIY shape-from-focus.

## 5. Surface properties (shiny, translucent, dark) and scanning spray thickness

### Takeaway
Fringe projection and laser triangulation assume a diffuse opaque surface; shiny, translucent and dark parts need matting, and the coating itself (AESUB Blue 8-15 µm, Orange 2-6 µm, Yellow <1 µm, Diamond ~2 µm per the manufacturer) consumes 10-75 % of a 20 µm error budget per surface, doubled on a diameter.

### Cited Findings
- Fringe projection "cannot, generally, measure specular surfaces" — [arXiv:1504.04556](https://arxiv.org/pdf/1504.04556)
- Polished steel gauge blocks had to be dulled (talc + trace lanolin) to be scanned by structured-light scanners — [arXiv:1910.13199](https://arxiv.org/pdf/1910.13199)
- OpenScan sphere test used AESUB Orange plus cross-polarised lighting on chrome steel spheres — [OpenScan blog](https://blog.openscan.eu/posts/chasing-microns-openscan-mini-accuracy-test-part-2/)
- AESUB Blue (sublimating): layer thickness 8-15 µm depending on application (2019 datasheet said 5-15 µm) — [AESUB Blue TDS](https://www.123-3D.nl/pdf/DSN00007_TDS_EN.pdf); [older datasheet](https://test.3dmart.com.tw/en/upload/2020AESUBBlue/Technical_Datasheet_AESUB_blue.pdf)
- Other AESUB grades (manufacturer/reseller figures): Orange 2-6 µm (sublimates in 12-24 h); Violet 4-8 µm; Green ~8-15 µm in one brochure vs ~10-20 µm in another (conflict); Yellow < 1 µm; Transparent 15-20 µm (dulling, not white); Diamond ~2 µm — [AESUB comparison Nov 2024](https://3dverkstan.se/wp-content/uploads/2025/01/AESUB-Comparison-2024-November_web.pdf); [GoMeasure3D brochure](https://cdn.shopify.com/s/files/1/0306/1181/files/gomeasure3d-aesub-product-brochure-11-02-2023.pdf); independent hands-on (no thickness measurement) at [Aniwaa](https://www.aniwaa.com/review/3d-scanners/reviewing-aesub-3d-scanning-spray/)
- On rough surfaces coherent/partially coherent illumination gives speckle; on smooth (specular) surfaces precision can be far better but only with methods that capture the specular return (e.g. structured-illumination microscopy), not classic triangulation — [arXiv:1504.04556](https://arxiv.org/pdf/1504.04556)

### Inferences
- Budget impact: a coating of thickness t adds t to each surface, so 2t to an outside diameter and -2t to a bore. AESUB Blue at 8-15 µm is 16-30 µm on a diameter, i.e. it alone can exceed a 0.02 mm target; thinner grades (Yellow, Diamond, Orange) or a correction offset are needed. The thickness also varies with operator technique (the datasheet says so), so it is an uncertainty, not just a correctable offset.
- Translucent materials (many resins, nylon, wax, teeth, marble) let light scatter below the surface, shifting the apparent surface inward and blurring fringes; this bias can be tens to hundreds of µm and is worse at longer wavelengths. Dark surfaces cut signal and so raise phase noise (sigma_phi scales as 1/modulation per section 2). Shiny surfaces produce saturation and inter-reflection. These are standard qualitative statements; I did not find quantitative sourced figures this session.
- Low-cost mitigations in order of error added: cross-polarised illumination (zero thickness), HDR/multi-exposure fringe capture (zero), blue illumination for translucent parts (zero), thin sublimating spray (1-6 µm grades), developer spray/talc (thicker, unknown).

### Gaps
- No independent measurement of sprayed layer thickness (all figures are manufacturer or reseller).
- No sourced quantitative subsurface-scattering bias figures for specific plastics.
- No thickness data found for hobbyist substitutes (developer spray, talc, dry-shampoo, airbrushed TiO2, cyclododecane).

## 6. Realistic scan volumes at 0.02 mm for a low-cost build

### Takeaway
At 0.02 mm, a low-cost single-view volume is on the order of 30-100 mm across and about 1-10 mm of usable depth per focus setting; larger objects must be built up by stitching/turntable rotation, where alignment error adds on top. The commercial comparison points agree: MINI 2 covers 52 x 64 mm at its nearest distance and Artec Micro-class certified units are small-object desktop scanners.

### Cited Findings
- Revopoint MINI 2: single capture area 52 x 64 mm at 120 mm, 168 x 132 mm at 250 mm; min scan volume 10 x 10 x 10 mm, max 0.5 m cube via stitching — [Revopoint brochure](https://dl.revopoint3d.com/brochure/mini2/mini2-brochure-en.pdf)
- Fringe-projection depth precision in a lab system: ~5 µm at ~240 mm working distance vs ~70 µm at 900 mm — [PMC10613632](https://pmc.ncbi.nlm.nih.gov/articles/PMC10613632/)
- Global (volumetric) accuracy degrades as part size grows; handheld laser units rely on stick-on markers for whole-object accuracy — [Scantech](https://www.3d-scantech.com/how-accurate-is-a-handheld-3d-scanner-/); [3dprinting.com](https://3dprinting.com/scanners/best-3d-scanners-for-large-objects/)
- OpenScan Mini evidence is on 20-25 mm spheres — [OpenScan blog](https://blog.openscan.eu/posts/chasing-microns-openscan-mini-accuracy-test-part-2/)
- High lateral resolution and depth of field are mutually exclusive in fringe projection — [arXiv:1504.04556](https://arxiv.org/pdf/1504.04556)

### Inferences
- From section 2 arithmetic, realistic single-view envelopes for a cheap build:
  | Goal | Camera | FOV | Usable depth per view |
  |---|---|---|---|
  | 20 µm depth noise, ~40-50 µm lateral detail | 5-12 MP | ~80-110 mm wide | ~5-10 mm (f/8-f/11, diffraction-limited ~50 µm) |
  | 20 µm point spacing, ~25 µm optical resolution | 12-20 MP | ~55-80 mm | ~1-2.5 mm |
  | true 20 µm lateral detail with margin | 12-20 MP | ~30-55 mm | ~0.6-2.4 mm |
- A relative accuracy of 20 µm over 100 mm is 1 part in 5,000; over 50 mm it is 1 in 2,500. Calibration targets, structural stiffness and thermal expansion all have to hold that: a 100 mm aluminium baseline grows ~2.3 µm per kelvin (standard CTE 23 ppm/K; unsourced here), 3D-printed PLA/PETG frames several times more, plus the 30-minute projector warm-up drift noted in section 2.
- Sensible DIY target statement: "0.02 mm noise/precision and roughly 0.02-0.05 mm accuracy on objects up to ~50 mm, verified with spheres and gauge blocks", using a fixed camera-projector head, short working distance (~100-250 mm), blue or white LED/DLP projection, and a turntable for multi-view. Claiming 0.02 mm over >100-150 mm objects in a few-hundred-USD build is not supported by anything found.
- Multi-view stitching error: each registration adds its own error; with ICP on low-feature parts this can exceed single-view error. A mechanically calibrated turntable axis (calibrated once with a sphere or plate) is the low-cost way to keep it bounded. Unsourced reasoning.

### Gaps
- No published low-cost build reporting accuracy as a function of scan volume was found.
- No sourced data on turntable-axis calibration error or ICP registration error at this scale.
- Projector-side limits (pico DLP native pixel pitch at short throw, minimum focus distance, need for close-up lenses) were not researched in this session.
