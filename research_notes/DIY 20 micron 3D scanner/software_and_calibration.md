# Software, calibration methods and laptop-side pipeline for a DIY 20 micron wired 3D scanner

Research date: 11 November 2026. Source-access notes that affect confidence:
- GitHub API access was blocked in this session, so repository dates/stars come from the rendered GitHub web pages (where fetched) or from third-party indexes. Where a last-commit date was not retrievable it is listed as a gap.
- Several primary PDFs/sites returned HTTP 503/403 (Brown University mesh.brown.edu calibration page, Purdue ZhangLab PDF, TU Delft repository, docs.opencv.org direct fetch). Those items rely on search-result summaries and are flagged.
- Items under "Inferences" are the researcher's reasoning or general background knowledge and are NOT independently sourced in this session; treat them as lower confidence than "Cited Findings".

---

## 1. What open-source structured-light software exists, and is it maintained (2026)?

### Takeaway
SLStudio (DTU, Jakob Wilm) is the stand-out: it has been modernised and was receiving commits in November 2026, supports phase shifting with heterodyne unwrapping and Gray code, drives a projector as a fullscreen/second display over HDMI, and supports USB3 Vision cameras via Aravis under GPLv3. Most other projects (3DUnderworld, Brown scan3d-capture, structured-light-python, older Gray-code repos) are stale or archived and are best used as reference code; SLMaster (AGPL-3.0, Windows, CUDA-capable) is the other substantial full application.

### Cited Findings

**SLStudio (jakobwilm/slstudio)**
- The most recent commits on `master` are dated 3 and 6 November 2026 (e.g. "build: fix OpenCV 5 calibration headers", "Fix low-risk compiler warnings without changing behavior"; several others have placeholder messages "asdf"). 138 commits total; no release/tag was visible on the commits page — [GitHub commits](https://github.com/jakobwilm/slstudio/commits/master/)
- Licence: "SLStudio is available under GPLv3"; commercial licensing offered separately — [GitHub README](https://github.com/jakobwilm/slstudio/)
- Patterns/codecs: phase shifting, heterodyne unwrapping, spatial unwrapping, motion-compensated and other phase-shift variants, sparse Gray-code reconstruction; CLI reconstruction and PLY/PCD export — [GitHub README](https://github.com/jakobwilm/slstudio/)
- Cameras: Aravis (USB3 Vision / GigE Vision) included in the Linux, Windows and macOS packages; OpenCV webcams in all packages; libdc1394, XIMEA xiAPI and FLIR/Teledyne Spinnaker as optional source-build backends — [GitHub README](https://github.com/jakobwilm/slstudio/)
- Projectors: desktop fullscreen output on all platforms; "exclusive" projector output (included in the Linux package, optional elsewhere); TI LightCrafter 4500 over USB with pattern upload, or patterns streamed over HDMI; LightCrafter 3000 network control — [GitHub README](https://github.com/jakobwilm/slstudio/)
- Calibration: geometric calibration of the camera-projector pair from a range of target positions/angles across the field of view (circle-grid PDF included), plus optional intensity calibration that measures projector response and applies compensation — [GitHub README](https://github.com/jakobwilm/slstudio/)
- Build stack is current: CMake >= 3.21, C++17, OpenCV 4 or 5, Eigen, GLFW/GLEW/OpenGL 3.3, Dear ImGui; packages as Linux AppImage (glibc >= 2.39), macOS DMG, Windows x64 ZIP; optional ROS 2 — [GitHub README](https://github.com/jakobwilm/slstudio/)
- The README explicitly warns hardware validation is incomplete: Windows projector outputs, LightCrafter 3000 projection, Aravis hardware triggering and projector-camera trigger circuits are listed as untested; "Passing CI builds and automated tests does not establish hardware compatibility" — [GitHub README](https://github.com/jakobwilm/slstudio/)
- Real-time (20 Hz+) point clouds require projector-camera hardware synchronisation; standard projectors and webcams work at lower rates — [GitHub README](https://github.com/jakobwilm/slstudio/)
- Original paper: Wilm, Olesen, Larsen, "SLStudio: Open-source framework for real-time structured light", IPTA 2014 — [DTU Orbit](https://orbit.dtu.dk/en/publications/slstudio-open-source-framework-for-real-time-structured-light/)
- The original design is a single camera + single projector system — [GitHub README](https://github.com/jakobwilm/slstudio/)

**SLMaster (Practice3DVision/SLMaster)**
- Described as "a relatively complete open-source Structured Light 3D camera software" covering calibration, stripe encoding, 3D reconstruction and point cloud post-processing; features include single/dual/trinocular reconstruction, real-time reconstruction, stripe generation, offline camera calibration with error-distribution display, online projector calibration, offline scanning from bundled sample data — [GitHub](https://github.com/Practice3DVision/SLMaster)
- Licence AGPL-3.0; 423 stars, 77 forks, 31 open issues, 67 commits; not archived — [GitHub](https://github.com/Practice3DVision/SLMaster)
- GPU: OpenCV built with `WITH_CUDA` is recommended; without an NVIDIA GPU it runs on CPU; the prebuilt .exe installer does not support GPU acceleration — [GitHub](https://github.com/Practice3DVision/SLMaster)
- Windows/MSVC focused; dependencies Qt 5.15.14, OpenCV 4.8.0 + contrib, VTK 9.2.0, PCL 1.12.1, and the MVViewer 2.3.5 camera SDK (Irayple/Huaray industrial cameras) — [GitHub](https://github.com/Practice3DVision/SLMaster)
- The repository page does not enumerate which fringe encodings are supported — [GitHub](https://github.com/Practice3DVision/SLMaster)

**SLutils (jhacsonmeza/SLutils)**
- C++/CUDA utilities: N-step and three-step phase shifting (with modulation map output), unwrapping by centre-line (spatial), phase-shift + Gray code, and multi-frequency phase shifting; CUDA build is on by default if a CUDA compiler is found, CPU fallback available; optional Python bindings via nanobind (NumPy arrays on CPU, `torch.Tensor` on CUDA); Apache-2.0; 7 stars, 49 commits — [GitHub](https://github.com/jhacsonmeza/SLutils)

**structured-light-python (helleb0re)**
- Python FPP framework: sinusoidal phase shifting with arbitrary number of shifts and periods, hierarchical phase unwrapping, automatic ROI detection, simple projector gamma correction; cameras via OpenCV webcams or Baumer NeoAPI; dependencies opencv-contrib-python, numpy, scipy, matplotlib — [GitHub](https://github.com/helleb0re/structured-light-python)
- The authors report about 60 micron accuracy on a granite slab using a "phasogrammetric" approach — [GitHub](https://github.com/helleb0re/structured-light-python)
- The repository was archived (read-only) by its owner on 4 January 2025; 26 stars — [GitHub](https://github.com/helleb0re/structured-light-python)

**elerac/structuredlight**
- Python library (pip-installable, MIT) that generates and decodes structured light patterns including Gray code and phase shifting; it is a coding/decoding library, not a full scanner — [ecosyste.ms index](https://awesome.ecosyste.ms/projects/github.com%2Felerac%2Fstructuredlight)

**OpenCV `structured_light` module (opencv_contrib)**
- Provides `GrayCodePattern` (a time-multiplexed Gray code approach following the 3DUNDERWORLD stereo method) and `SinusoidalPattern` (Fourier transform profilometry FTP, phase-shifting profilometry PSP, and Fourier-assisted phase shifting FAPS, with phase unwrapping) — [docs.rs mirror of OpenCV module docs](https://docs.rs/opencv/0.81.1/opencv/structured_light/index.html)
- Official tutorials exist for capturing a Gray-code dataset, decoding Gray code into a point cloud, and capturing sinusoidal patterns/computing phase maps; the tutorials are carried in the 3.4, 4.x and 5.x documentation trees — [OpenCV 4.x tutorial index](https://docs.opencv.org/4.x/d3/d81/tutorial_structured_light.html); [OpenCV 5.x tutorial index](https://docs.opencv.org/5.x/d3/d81/tutorial_structured_light.html)

**3DUNDERWORLD-SLS**
- Open-source Gray-code stereo structured-light scanner with CPU and GPU (CUDA, enabled by default if detected) implementations; main repository theICTlab/3DUNDERWORLD-SLS-GPU_CPU — [arXiv 1406.6595](https://arxiv.org/pdf/1406.6595); [Zenodo record](https://zenodo.org/records/1048960)
- The latest archived release is "v4 (GPU support)", published 14 November 2017 — [Zenodo record](https://zenodo.org/records/1048960)

**Brown University (Moreno & Taubin) projector-camera calibration + scanning software**
- The method ("Simple, Accurate, and Robust Projector-Camera Calibration", 3DIMPVT 2012, pp. 464-471, DOI 10.1109/3dimpvt.2012.77) uses local homographies around each checkerboard corner to transfer corner positions from camera to projector image, with subpixel precision and a full radial+tangential distortion model for the projector — [paper record](https://synapsesocial.com/papers/W2171988673)
- The projector parameters are estimated without relying on the camera calibration to find correspondences, avoiding propagation of small camera calibration errors — [patent US20140078260A1](https://patentimages.storage.googleapis.com/97/76/74/74dc2306ea21d3/US20140078260A1.pdf)

**Other projects surfaced**
- pranavkantgaur/3dscan: C++/OpenCV Gray-code + phase-shift scanner, MIT, last pushed 2018 — [search index summary of GitHub](https://github.com/mfkiwl/3dscan)
- DTU SeeMaLab scanner: reference implementation of binary Gray coding, phase shifting and line shifting for a two-camera + projector + turntable rig, distributed via SVN — [DTU eco3d page](https://eco3d.compute.dtu.dk/pages/3d_scanner)
- ETH Zurich `sl_sensor` reuses reconstruction components from SLStudio — [ecosyste.ms index](https://awesome.ecosyste.ms/projects/github.com%2Fethz-asl%2Fsl_sensor)
- jhdewitt/sltk (structured light toolkit) and nikolaseu/neuvision forks exist — [sltk wiki](https://github.com/jhdewitt/sltk/wiki); [neuvision fork](https://github.com/J-C-Haley/neuvision)

### Inferences
- For a "wired to a laptop, lowest cost, maximum accuracy" build, the realistic choices are (a) SLStudio as-is (single camera + projector, GPLv3, actively touched in Nov 2026) or (b) writing a small Python/C++ pipeline of one's own using SLutils / structured-light-python / OpenCV as reference code. The burst of commits with placeholder messages in SLStudio suggests active refactoring; pin a known-good commit or packaged build rather than tracking `master`.
- SLStudio's own README says its hardware paths are not fully validated after the modernisation, so budget time for debugging camera/projector I/O.
- No open-source package found advertises a turnkey two-camera + multi-frequency phase-shift + turntable workflow comparable to commercial FlexScan3D; the DTU SeeMaLab code is the nearest academic equivalent, and SLMaster the nearest GUI application (but tied to specific Chinese industrial camera SDKs and Windows).
- AGPL/GPL licences only matter if the user redistributes modified software; for personal use they impose no cost.

### Gaps
- Last-commit dates for SLMaster, SLutils, elerac/structuredlight, 3DUNDERWORLD (GitHub, post-2017), sltk, neuvision and the OpenCV structured_light module could not be retrieved (GitHub API blocked; pages do not show dates).
- Could not reach mesh.brown.edu (HTTP 503) to confirm the current download, licence and last update of the Brown "Projector-Camera Calibration / 3D Scanning Software" (scan3d-capture); believed to be unmaintained Qt/OpenCV 2-era code but this was not verified.
- No evidence was found for any open "BYU" fringe-projection code base; nothing surfaced under that name.
- The specific encodings SLMaster implements (e.g. multi-frequency heterodyne, complementary Gray code) are not listed on its README; requires reading its wiki/source.
- FlexScan3D itself (Polyga) is commercial; no pricing/free-tier information was researched.

---

## 2. Laser-line software (Horus/Ciclop, FabScan)

### Takeaway
Horus (BQ Ciclop) is abandoned upstream and its forks are stale; FabScanPi is the only laser-line project with commits in the 2020s (last seen April 2024) but it is Raspberry Pi-centred, not laptop-wired. Neither is a credible route to 20 microns.

### Cited Findings
- FabScanPi-Server's five most recent commits are dated 27 March 2024 and 8 April 2024 — [GitHub commits](https://github.com/mariolukas/FabScanPi-Server/commits/master/)
- FabScanPi documents a conversion of the Ciclop to FabScan software by replacing the webcam with a Raspberry Pi camera and running the server on the Pi — [FabScanPi docs](https://fabscanpi-server.readthedocs.io/en/latest/steps_ciclop.html)
- Horus forks show 2015-era releases (e.g. javivi001 fork 0.1.2.4 targeting Ubuntu 14.04/14.10; Swindon Makers firmware fork last updated 2015) — [fork index](https://gh.r-mj.com/javivi001/horus); [horus-fw mirror](https://github.imc.re/LibreScanner/horus-fw)
- Older laser-line turntable systems report multi-view registration error of about 70 microns within an 80 mm rotation radius on a platform of 50 micron accuracy — [Chinese Journal of Lasers 2005](https://m.researching.cn/articles/OJ145ba5afdbb493b4)

### Inferences
- Horus was written for Python 2 / wxPython-era dependencies, so running it on a 2026 laptop OS typically needs a community fork or legacy environment (not verified in this session).
- Hobby laser-line scanners with webcams are generally in the 0.1-0.5 mm class; a laser-line approach to 20 microns would need a high-quality line laser, industrial camera, and custom sub-pixel peak-detection code rather than these packages.

### Gaps
- No authoritative statement of Horus's archival status or a maintained Python 3 fork was retrieved.
- No measured accuracy figures for Ciclop/FabScan were retrieved in this session.

---

## 3. Photogrammetry, Gaussian splatting/NeRF, and macro focus-stacked photogrammetry

### Takeaway
Free photogrammetry is well served in 2026 (Meshroom 2025.1, COLMAP 3.12/3.13, RealityScan 2.x free under US$1M revenue), and peer-reviewed macro focus-stacked SfM has demonstrated single-digit-micron error on small rough surfaces — but only on highly textured samples, with scale and validation supplied externally. Gaussian splatting/NeRF methods are evaluated at the millimetre level and are not metrology tools.

### Cited Findings
- RealityScan (rebranded RealityCapture, 2.0 released June 2025) is free for individuals and businesses with under US$1 million revenue over the past 12 months, plus students/educators, with all features; above the threshold a paid seat is required — [RealityScan licence page](https://www.realityscan.com/en-US/license); [CG Channel](https://www.cgchannel.com/?p=167577)
- Paid seat price is stated as US$1,250 per seat per year on the licence page as fetched; a search summary of the same site reported US$1,850 — conflicting, check the live page — [RealityScan licence page](https://www.realityscan.com/en-US/license); conflicting figure via [realityscan.com/license](https://realityscan.com/license)
- Moving to RealityScan 2.0 requires accepting a new EULA; the legacy RealityCapture licensing server was announced to close on 3 August — [RealityScan news](https://www.realityscan.com/news/the-realitycapture-legacy-licensing-server-will-close-down-on-august-3); [RealityScan 2.0 news](https://www.realityscan.com/news/realityscan-20-new-release-brings-powerful-new-features-to-a-rebranded-realitycapture)
- Meshroom 2025.1.0 was released 18 August 2025 — [GitHub release](https://github.com/alicevision/Meshroom/releases/tag/v2025.1.0)
- Meshroom's depth-map (MVS) stage requires an NVIDIA GPU with CUDA compute capability >= 2.0; depth-map generation is the most computation-intensive part of the pipeline — [Meshroom manual](https://meshroom-manual.readthedocs.io/en/latest/faq/needs-cuda/needs-cuda.html); [Meshroom tutorial](https://meshroom-manual.readthedocs.io/en/latest/tutorials/sketchfab/sketchfab.html)
- AliceVision build docs list CUDA >= 11.0 for feature extraction and depth map computation — [AliceVision INSTALL](https://alicevision.readthedocs.io/en/stable/md__home_docs_checkouts_readthedocs_8org_user_builds_alicevision_checkouts_stable_INSTALL.html)
- A mailing-list thread reports SYCL (non-CUDA GPU) support being merged into AliceVision, relevant to AMD/Intel GPUs; not confirmed in official binaries — [AliceVision group](https://groups.google.com/g/alicevision/c/z-ICK-THNZI/m/afybV0QtAgAJ)
- COLMAP 3.12 and 3.13 releases exist with prebuilt Windows CUDA binaries; dense `patch_match_stereo` requires a CUDA build — [COLMAP 3.12.6 mirror](https://www.sourceforge.net/projects/colmap.mirror/files/3.12.6); [COLMAP 3.13.0 mirror](https://sourceforge.net/projects/colmap.mirror/files/3.13.0); [COLMAP install docs](https://colmap.github.io/install.html)
- 3DF Zephyr Free is not time-limited, is for personal use, and is capped at 50 images; Lite allows 500 — [3Dflow Free edition page](https://www.3dflow.net/?p=7885); [3Dflow forum](https://3dflow.net/forums/forum/3df-zephyr-forum-english/7551-number-of-photos-limit)
- Macro SfM + focus stacking with consumer equipment and free/affordable software, validated against white-light interferometry, reported a standard deviation of error of about 6.5 microns on sub-millimetre rock-fracture roughness (errors associated with features of wavelength < 150 microns and amplitude < 10 microns). The figure comes from a garbled text extraction and should be confirmed in the paper — [TU Delft: Focus stacking photogrammetry for micro-scale roughness reconstruction (2019)](https://research.tudelft.nl/en/publications/focus-stacking-photogrammetry-for-micro-scale-roughness-reconstru)
- Depth of field is the binding constraint in macro work: e.g. roughly 40 microns DOF at full magnification on a Canon MP-E 65 mm — [focus-stacking notes](https://download.osmand.net/wiki/User:Franz_van_Duns/Focus_Stacking)
- Macro lenses with focus stacks violate the standard pinhole model; GEOMAR's "MaCal" work proposes a dedicated focus-stack camera model/calibration — [GEOMAR MaCal](https://www.geomar.de/fileadmin/content/forschen/fb2/mg/omv/macrocalibration.pdf)
- Gaussian splatting / NeRF geometry: a 2026 comparative evaluation (19 scenes) found 2DGS best with a Chamfer distance of 3.15 mm, with precision 0.46 and recall 0.28 at a 2 mm threshold — [arXiv 2604.18205](https://arxiv.org/pdf/2604.18205)
- On the DTU object benchmark, original 2DGS averages about 0.80 mm Chamfer distance — [arXiv 2503.06587](https://arxiv.org/pdf/2503.06587)
- 2DGS/3DGS can out-perform conventional photogrammetry on transparent glass where SfM/MVS fails, per a metric error assessment on a glass artefact — [Sensors 2025, PMC12300396](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC12300396/)

### Inferences
- Gaussian splatting/NeRF errors are reported in tenths of millimetres to millimetres, 1-2 orders of magnitude away from 20 microns; they are view-synthesis tools, not a route to the target.
- Photogrammetry produces a scale-free reconstruction; scale must be injected by measured distances (scale bars / coded targets with calibrated spacing, or a calibrated object in scene). Any relative error in the scale reference transfers directly: 20 microns over a 100 mm part is 0.02 percent, so scale bars need to be known to a few microns (e.g. gauge-block or CMM-calibrated spacing) — printed rulers are not adequate. (Reasoning; not sourced.)
- Macro focus-stacked photogrammetry is the cheapest path if the user already owns a camera and macro lens, but it only works on matte, richly textured surfaces, needs hundreds to thousands of source frames, and a focus-stacking step that can itself introduce geometric artefacts. It is a plausible complement or cross-check, not obviously a replacement for fringe projection on smooth machined parts.
- RealityScan's free tier is the most capable free option for the user (personal use is clearly under the revenue cap) but is closed-source; Meshroom and COLMAP (+OpenMVS for dense/mesh) are the open alternatives.

### Gaps
- Agisoft Metashape pricing/licensing in 2026 was not retrieved.
- OpenMVS maintenance status was not retrieved.
- No VRAM/RAM requirement figures from primary docs were found for Meshroom 2025.1 or COLMAP 3.12+ dense stages.
- The TU Delft record returned HTTP 503; the 6.5 micron figure and the method of scale-setting in that study were not confirmed from the full text.
- No peer-reviewed accuracy evaluation of small-object photogrammetry using scale bars against a CMM was retrieved in this session.

---

## 4. How the laptop is used: GPU, memory, HDMI projection and sync, USB3 cameras, triggering

### Takeaway
For static high-accuracy fringe scanning the laptop is not the bottleneck: phase decoding and triangulation are simple per-pixel operations, hardware synchronisation is only needed for real-time rates, and HDMI-as-second-display works if the software waits for each pattern to be genuinely displayed. The GPU matters mainly for photogrammetry dense matching (CUDA-only in Meshroom/COLMAP) and optionally for accelerating phase decoding.

### Cited Findings
- Over HDMI the frame timing is not deterministic: "an uncertain number of irrelevant frames could be inserted before the expected video frame is displayed", which makes reliable capture of pattern sequences hard without a handshake — [Vision Systems Design: low-cost structured light scanner](https://vision-systems.com/home/article/14035114/how-to-develop-a-low-cost-3d-scanner-using-structured-light)
- Hardware sync approach: use the projector's vertical sync pulse to trigger the camera; TI notes HDMI VSYNC is commonly used to trigger an external camera, and its Pico kit provides a sync pulse with adjustable delay — [TI E2E forum](https://e2e.ti.com/support/dlp-products-group/dlp/f/dlp-products-forum/153106/synchronization-kit-dlp-projector); [3DBody.tech 2016 paper (Petkovic et al.)](https://proc.3dbody.tech/papers/2016/16286petkovic.pdf)
- Software-only synchronisation with an off-the-shelf projector, camera and PC has been demonstrated at 30 FPS acquisition — [3DBody.tech 2016 paper (Petkovic et al.)](https://proc.3dbody.tech/papers/2016/16286petkovic.pdf)
- A low-cost trick is a photodiode/LDR watching a white corner square in the projected frame to trigger the camera or confirm display — [TI E2E forum](https://e2e.ti.com/support/dlp-products-group/dlp/f/dlp-products-forum/153106/synchronization-kit-dlp-projector)
- SLStudio renders patterns via OpenGL to a fullscreen desktop output or an "exclusive" projector output; real-time scanning needs projector-camera synchronisation, but ordinary projectors and webcams work at lower rates — [SLStudio README](https://github.com/jakobwilm/slstudio/)
- In the DTU accuracy study the cameras used 66.66 ms exposure with a consumer Full-HD LED DLP projector (LG PF80G), i.e. slow, static acquisition of 64 patterns per phase-shift scan — [Eiríksson et al., ISPRS 2016](https://isprs-archives.copernicus.org/articles/XL-5-W8/7/2016/isprs-archives-XL-5-W8-7-2016.pdf)
- GenICam cameras expose `TriggerSource` (software or a hardware line) and `TriggerDelay`; switching between software and hardware trigger is a parameter change — [MVTec acquisition modes](https://www.mvtec.com/services-support/developers-corner/article/review-of-acquisition-modes); [IDS trigger tech tip](https://en.ids-imaging.com/kb-article/techtip-gev-trigger-ctl.html)
- GPU use in open structured-light code: SLutils (CUDA phase shifting/unwrapping with torch tensors), SLMaster (OpenCV CUDA), 3DUNDERWORLD (CUDA Gray-code reconstruction) — [SLutils](https://github.com/jhacsonmeza/SLutils); [SLMaster](https://github.com/Practice3DVision/SLMaster); [3DUNDERWORLD Zenodo](https://zenodo.org/records/1048960)
- GPU use in photogrammetry: Meshroom depth maps need NVIDIA CUDA; COLMAP patch-match stereo needs a CUDA build — [Meshroom manual](https://meshroom-manual.readthedocs.io/en/latest/faq/needs-cuda/needs-cuda.html); [COLMAP install docs](https://colmap.github.io/install.html)
- Data volume indicator: the DTU phase-shift scan yields about 450k points on a flat and about 20k on a 25 mm sphere per view from 9 MP cameras — [Eiríksson et al., ISPRS 2016](https://isprs-archives.copernicus.org/articles/XL-5-W8/7/2016/isprs-archives-XL-5-W8-7-2016.pdf)

### Inferences
- Recommended capture loop for a consumer HDMI projector (reasoning from the non-determinism finding above): display pattern fullscreen with vsync on -> wait a fixed settle time of several projector frames (e.g. 100-200 ms; consumer projectors add processing latency) -> software-trigger the camera with an exposure that is an integer multiple of the projector frame period (1/60 s) so DLP colour-wheel/PWM dithering averages out -> optionally average several exposures -> next pattern. At roughly 0.3 s per pattern, a 40-60 pattern sequence takes 15-20 s per view, acceptable for static objects on a turntable.
- Projector "image enhancement" features (keystone, sharpening, dynamic contrast, overscan/scaling) must be disabled and the laptop output set to the projector's native resolution at 1:1 pixel mapping with no OS scaling or night-light/colour management; otherwise the fringe pattern is resampled. (Background knowledge; not sourced.)
- USB3 bandwidth: USB 3.x Gen 1 is nominally 5 Gbit/s, and USB3 Vision cameras commonly deliver on the order of 350-400 MB/s in practice, so a 12 MP 8-bit mono camera can stream ~30 fps and a 20 MP camera ~15-20 fps — far more than a static scanner needs. Two cameras should be on separate root ports/controllers if streaming simultaneously. (Background knowledge; no source retrieved this session.)
- Memory budget (estimate): one 12 MP 16-bit frame is 24 MB; 60 patterns x 2 cameras is about 2.9 GB of raw images per view if all held in RAM as uint16 (half that for 8-bit). Decoding to float32 phase maps adds a few hundred MB. 16 GB RAM is comfortable; 32 GB+ is useful for merging many multi-million-point views and for Poisson meshing at high depth. Phase decoding on GPU needs well under 8 GB VRAM for 12-20 MP if processed per frequency; photogrammetry dense matching is the VRAM-hungry task.
- N-step phase computation is a per-pixel weighted sum + atan2 and takes well under a second per view with vectorised NumPy; the GPU is a convenience, not a requirement, for offline scanning. Hardware triggering is only worth the complexity for dynamic/handheld scanning or to shorten capture time.
- Non-NVIDIA alternatives: CPU builds of SLutils/OpenCV for fringe decoding; for photogrammetry, RealityScan and Metashape historically support non-CUDA paths less well (not verified), and AliceVision's SYCL work may eventually help AMD/Intel GPUs.

### Gaps
- No authoritative numbers were retrieved for consumer-projector HDMI latency/settle times, or for USB3 Vision practical throughput.
- No primary-source VRAM/RAM requirement tables retrieved for any package.
- No documentation was found on achieving frame-accurate HDMI output from Python specifically (e.g. via GLFW/pyglet/PsychoPy swap-buffer timing); the common DIY approach of `cv2.imshow` fullscreen plus a delay was not source-verified.

---

## 5. Calibration to reach about 20 microns

### Takeaway
The best-documented low-cost recipe is the DTU one: two calibrated industrial cameras with the projector used only as an uncalibrated texture/phase source, Zhang calibration on a checkerboard bonded to float glass with a deliberately small parameter set (fixed principal point and aspect ratio, k1 and k2 only), about 11 well-spread poses, and many-step high-frequency phase shifting. That reached precision comparable to (and in form error better than) a GOM ATOS III over a 300 mm field; 20 microns then requires scaling the field of view down and controlling target accuracy, projector nonlinearity and thermal drift.

### Cited Findings

**Reference DIY-class system and what calibration choices mattered (Eiríksson, Wilm, Pedersen, Aanæs 2016)**
- Hardware: two Point Grey GS3-U3-91S6C-C cameras (Sony ICX814 1", 3376 x 2704), 16 mm lenses at f/5.6, LG PF80G Full-HD LED DLP projector, 450 mm camera baseline, about 750 mm object distance, stereo FOV 300 x 300 x 230 mm, on a rigid aluminium beam with a rotation stage, enclosable against ambient light — [ISPRS Archives XL-5/W8, 2016](https://isprs-archives.copernicus.org/articles/XL-5-W8/7/2016/isprs-archives-XL-5-W8-7-2016.pdf)
- Calibration target: 400 x 280 x 12 mm unhardened float glass with a high-resolution printed checkerboard glued to the flat surface — [ISPRS 2016](https://isprs-archives.copernicus.org/articles/XL-5-W8/7/2016/isprs-archives-XL-5-W8-7-2016.pdf)
- Camera model: Zhang's method, 4-parameter pinhole plus up to five distortion parameters; eight parameter configurations were compared using VDI/VDE 2634 metrics — [ISPRS 2016](https://isprs-archives.copernicus.org/articles/XL-5-W8/7/2016/isprs-archives-XL-5-W8-7-2016.pdf)
- Result: enabling the first two radial coefficients gives significant improvement (especially in sphere distance); estimating free aspect ratio and principal point degrades performance — "omitting the principle point estimation makes calibration significantly more stable"; "only the k1 and k2 distortion coefficients are required" for that setup — [ISPRS 2016](https://isprs-archives.copernicus.org/articles/XL-5-W8/7/2016/isprs-archives-XL-5-W8-7-2016.pdf)
- Poses: 11 board observations evenly sampled across a +/-40 degree range gave results similar to 81 observations; a large angular range is "slightly beneficial", shallow angles raise localisation uncertainty and non-planarity effects — [ISPRS 2016](https://isprs-archives.copernicus.org/articles/XL-5-W8/7/2016/isprs-archives-XL-5-W8-7-2016.pdf)
- Encoding: phase shifting with 32 steps at the highest frequency (period 19.2 projector px) plus two lower-frequency sets for unwrapping, 64 patterns total; Gray code used 20 patterns; line shifting 28 patterns — [ISPRS 2016](https://isprs-archives.copernicus.org/articles/XL-5-W8/7/2016/isprs-archives-XL-5-W8-7-2016.pdf)
- Phase shifting gave the best sphere-form (PF) and flatness (F) in all positions, about 3x the point density of Gray code, and excellent off-focus robustness; but phase shift and Gray code showed a systematic positive bias in sphere diameter (residuals toward the lateral edge in the encoding direction) while line shifting appeared unbiased — [ISPRS 2016](https://isprs-archives.copernicus.org/articles/XL-5-W8/7/2016/isprs-archives-XL-5-W8-7-2016.pdf)
- Versus GOM ATOS III Triple Scan (FOV 240 x 320 x 240 mm): the DIY system was more precise in PF with lower variance, while the commercial system showed better sphere-spacing accuracy (SD) and flatness — [ISPRS 2016](https://isprs-archives.copernicus.org/articles/XL-5-W8/7/2016/isprs-archives-XL-5-W8-7-2016.pdf)
- Phase-shift encoding requires a close-to-linear projector-camera intensity response — [ISPRS 2016](https://isprs-archives.copernicus.org/articles/XL-5-W8/7/2016/isprs-archives-XL-5-W8-7-2016.pdf)

**Projector as inverse camera**
- Zhang & Huang (2006) treat the projector as an image-capturing device so that projector calibration reduces to standard camera calibration — [Optical Engineering 45(8) 083601](https://www.spiedigitallibrary.org/journals/optical-engineering/volume-45/issue-8/083601/Novel-method-for-structured-light-system-calibration/10.1117/1.2336196.full)
- Li, Karpinsky & Zhang (2014) extended this to out-of-focus projectors and reported about 73 microns RMS over a 150 x 250 x 200 mm volume — [Applied Optics 2014 (Purdue)](https://engineering.purdue.edu/ZhangLab/publications/papers/2014-ao-calibration.pdf)
- Moreno & Taubin (2012): local homographies per checkerboard corner give sub-pixel projector coordinates for each corner and allow a full radial + tangential distortion model for the projector, independent of the camera calibration — [paper record](https://synapsesocial.com/papers/W2171988673); [patent text](https://patentimages.storage.googleapis.com/97/76/74/74dc2306ea21d3/US20140078260A1.pdf)
- Projector distortion is frequently under-modelled because the projector principal point can lie outside the chip (lens offset), so standard distortion functions fit poorly — [arXiv 1404.7298](https://arxiv.org/pdf/1404.7298)

**Stereo cameras + uncalibrated projector**
- With camera-camera triangulation, "the calibration parameters of the projection unit are not necessary for the measurement process" — [arXiv 1404.7298](https://arxiv.org/pdf/1404.7298)
- A profilometer built from two calibrated cameras and one uncalibrated projector using stereo phase rectification reports high metric accuracy — [Applied Optics record](https://bvbvf1.bib-bvb.de/vufind/PrimoRecord/cdi_proquest_miscellaneous_2725650336)

**Phase-shift error sources and gamma (Zuo et al. 2018 review)**
- Phase noise for standard N-step PSP: sigma_phi = sqrt(2/N) * sigma_n / B (sigma_n = intensity noise, B = modulation); after unwrapping with F fringe periods across the field, the effective error is further divided by F. Increasing F reduces error more efficiently than increasing N or B — [Zuo et al., Optics and Lasers in Engineering 109 (OSTI copy)](https://www.osti.gov/pages/servlets/purl/1469785)
- Home-theatre projectors default to a nonlinear gamma tuned to human vision, which distorts sinusoids and creates periodic phase error — [Zuo et al. 2018](https://www.osti.gov/pages/servlets/purl/1469785)
- Harmonic sensitivity: 3-step is sensitive to all higher-order (>= 2) errors; 4-step is insensitive to the 2nd; 5-step insensitive to the 2nd and 3rd; in general N-step is sensitive only to harmonics (p+1)N +/- 1 (e.g. 6-step is sensitive only to harmonics 5, 7, 11, 13...). A large number of steps can theoretically remove nonlinearity effects entirely, at the cost of sequence length — [Zuo et al. 2018](https://www.osti.gov/pages/servlets/purl/1469785)
- Compensation options: active (pre-distort patterns with a calibrated gamma/polynomial/LUT), defocusing binary or gamma-distorted fringes (simple, but reduces contrast/SNR), or passive LUT phase-error compensation built from fringe images of a flat board — [Zuo et al. 2018](https://www.osti.gov/pages/servlets/purl/1469785)
- Defocus does not introduce phase error for sinusoidal patterns in theory; it only reduces fringe contrast — [Zuo et al. 2018](https://www.osti.gov/pages/servlets/purl/1469785)
- Binary (0/1) patterns eliminate projector/camera nonlinearity by construction — [Measurement Science Review 2020](https://www.measurement.sk/2020/msr-2020-0006.pdf)

**Target accuracy and bundle adjustment**
- Printed boards are rarely perfectly planar (manufacturing, glue), and an imperfect target can significantly degrade Zhang's method; a bundle adjustment that jointly refines camera, projector and target-point positions mitigates this — [Stony Brook ISMAR 2018](https://www.cs.stonybrook.edu/~hling/publication/calib-18ismar.pdf)
- Reprojection error is not always a reliable indicator of calibration quality when the target has manufacturing errors; bundle adjustment with free target points loses absolute scale, so the target must be rescaled to a known dimension — [SPIE Optical Engineering 56(7) 074101](https://spiedigitallibrary.org/journals/Optical-Engineering/volume-56/issue-7/074101/Method-for-calibration-accuracy-improvement-of-projector-camera-based-structured/10.1117/1.OE.56.7.074101.pdf)
- Target-aware bundle adjustment that also solves the shape of the calibration target is used in recent (2025) multi-view phase-measuring work — [SciTePress 2025](https://www.scitepress.org/Papers/2025/131454/131454.pdf)
- Camera alignment drifts with thermal and mechanical stress, degrading a stored calibration and motivating periodic field recalibration — [US patent 11652975](https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/11652975)

### Inferences
- Most forgiving approach for a cheap projector: two calibrated cameras + projector as an uncalibrated phase source. Projector lens distortion, lens-shift offset, focus breathing, thermal drift of the projector optics and (to first order) its gamma no longer enter the triangulation geometry, because both cameras observe the same projected phase and correspondences are made camera-to-camera. Residual gamma error still adds phase ripple, but it is largely common-mode between the two cameras and is suppressed further by using many phase steps. The price is a second camera and loss of data where only one camera sees the surface. The cited evidence (arXiv 1404.7298; DTU study) supports the geometric part; the "common-mode gamma" argument is reasoning, not a cited result.
- With a single camera + projector (SLStudio, Moreno-Taubin style), the projector must be calibrated as an inverse camera and its intrinsics must stay stable — hard with consumer projectors whose optics heat up and whose focus/zoom rings have play. Reported accuracies for this configuration in the cited literature are tens of microns RMS at best over about 150-250 mm volumes (73 microns in Li et al. 2014), i.e. short of 20 microns unless the field of view is much smaller.
- Scaling argument: the DTU rig used 3376 px across about 300 mm (about 90 microns/px) and achieved form errors in the tens of microns (values are in figures and were not extracted numerically). Since triangulation error scales roughly with pixel footprint, a 20 micron target suggests a field of view of roughly 60-100 mm with a 9-20 MP camera (about 20-30 microns/px) so that the needed sub-pixel/phase precision is of order 1/5 to 1/10 px after systematic errors. This is an estimate, not a sourced figure.
- Practical gamma strategy for a cheap projector: (1) set the projector to its most linear picture mode and disable dynamic contrast; (2) measure the response by projecting a grey ramp of ~32-64 levels and recording mean camera intensity (with a linear, gamma-off machine-vision camera), then pre-distort patterns with the inverse LUT; (3) use N >= 8-16 steps at the finest frequency so residual harmonics up to order N-2 cancel; (4) keep the pattern within roughly 10-90 percent of the drive range to avoid clipping. Steps 1 and 4 are background knowledge; 2-3 follow from the Zuo review.
- Phase-to-height (reference-plane) calibration is an alternative to full stereo calibration but needs a precise translation stage to move a reference plane through the volume; for a DIY builder without such a stage the stereo-vision (Zhang-type) calibration is more accessible. (Background knowledge; not sourced this session.)
- Target accuracy: for a 20 micron goal the target's feature positions and flatness should be known several times better than the goal (single-digit microns). A laser-printed sheet glued to float glass (as DTU used at 300 mm scale) is probably marginal at a 60-100 mm field; better options are a photolithographic chrome-on-glass or ceramic dot/checker target, or else a bundle adjustment that frees target point coordinates and then fixes scale from one accurately measured length. The specific tolerance is the researcher's estimate.
- Warm-up: let projector and cameras reach thermal equilibrium before both calibrating and scanning (commonly 30-60 minutes for consumer projectors), calibrate in the same thermal state as scanning, and re-verify with a quick artefact scan each session. Supported qualitatively by the drift patent; the duration is background knowledge.
- Use few, well-conditioned parameters: the DTU result that fixing the principal point and using only k1, k2 was more accurate than a full model is a strong, counter-intuitive, citable recommendation — do not enable every OpenCV flag (`CALIB_RATIONAL_MODEL`, tangential, k3) by default; test each configuration against a physical artefact, not against reprojection error.
- Circle grids or ChArUco boards with sub-pixel refinement are commonly preferred to plain checkerboards under defocus, and SLStudio ships a circle-grid target; relative accuracy of target types was not sourced.

### Gaps
- Numeric VDI/VDE values (PF, PS, SD, F in microns) from the DTU paper are in bar charts and could not be extracted from the text layer; the report writer should describe them qualitatively or consult the figures (axes run to roughly 150 microns for PF and 300 microns for SD across the tested configurations).
- The original Moreno-Taubin paper's reported reprojection errors and recommended pose count were not retrieved (Brown site 503).
- Song Zhang's temporal-unwrapping review (Purdue PDF, 503) was not readable, so guidance on multi-frequency (hierarchical) vs multi-wavelength (heterodyne) vs Gray-code unwrapping noise tolerance is unsourced. Background understanding: hierarchical multi-frequency unwrapping (e.g. periods 1 -> ~8 -> ~64 across the field) is the most robust when pattern count is not constrained, and heterodyne with close high frequencies amplifies noise by the synthetic-wavelength ratio.
- No source was found that quantifies thermal warm-up drift of consumer projectors in microns or gives a specific warm-up time.
- No source retrieved gives a quantitative flatness/feature-position tolerance for calibration targets as a function of desired accuracy.
- No DIY build was found with independently verified accuracy at or below 20 microns; the nearest documented open Python project reported about 60 microns on a granite slab ([structured-light-python](https://github.com/helleb0re/structured-light-python)), and a 2024 low-cost stereo SL thesis reported sub-millimetre to millimetre accuracy ([Würzburg MSc thesis](https://robotik.informatik.uni-wuerzburg.de/telematics/download/MSc_Christoph_Liebender.pdf)).

---

## 6. Multi-view registration and meshing

### Takeaway
Multi-view alignment is likely to be the largest single consumer of a 20 micron budget: the only turntable figures found are about 70 microns registration error, and no source demonstrated turntable-plus-ICP registration at the 10 micron level. Open tools (CloudCompare, MeshLab, Open3D) cover ICP, global registration and Poisson meshing.

### Cited Findings
- Turntable-based scanners require calibration of the rotation axis, and recalibration whenever the scanner-turntable geometry changes — [Lau et al., IEEE CG&A 2014](https://www.cs.cityu.edu.hk/~rynson/papers/cga14.pdf)
- Reported turntable registration errors: about 70 microns within an 80 mm rotation radius on a platform of 50 micron accuracy; axis correction using calibration balls raised registration accuracy to 0.07 mm — [Chinese Journal of Lasers 2005](https://m.researching.cn/articles/OJ145ba5afdbb493b4); [BUAA journal 2005](https://bhxb.buaa.edu.cn/CN/Y2005/V31/I08/899)
- A common pipeline estimates the rotation axis from a calibration plane/target and uses it as the initial transform, then refines with ICP, reporting improved accuracy; plain ICP is hard to apply directly — [SPIE Optical Engineering 63(3) 033101](https://medicalimaging.spiedigitallibrary.org/journals/optical-engineering/volume-63/issue-03/033101/Calibration-method-of-a-three-dimensional-scanner-based-on-a/10.1117/1.OE.63.3.033101.full)
- Automatic calibration methods exist for two-axis rotary tables, each axis calibrated consecutively — [Sensors 20(24) 7107](https://mdpi.com/1424-8220/20/24/7107)
- VDI/VDE 2634 Part 3 is the guideline part that covers multi-view systems (Part 2 covers single-view area scanning) — [TUM thesis 2017](https://mediatum.ub.tum.de/doc/1619962/e2odjmbd0ri6wtjhdym1axrcr.2017_MELO_Thesis.pdf)
- The DTU reference rig included a rotation stage, also used to automate calibration board rotation — [ISPRS 2016](https://isprs-archives.copernicus.org/articles/XL-5-W8/7/2016/isprs-archives-XL-5-W8-7-2016.pdf)
- SLMaster includes point cloud post-processing built on PCL/VTK — [SLMaster](https://github.com/Practice3DVision/SLMaster)

### Inferences
- Error budget (root-sum-square reasoning): if single-view noise/form error is ~10 microns, calibration systematic (scale/distortion) ~10 microns and registration ~10 microns, the combined figure is ~17 microns; a 70 micron registration error (the only literature number found) would by itself blow the budget. Therefore: (a) state accuracy claims separately for single-view and for merged models; (b) minimise the number of views; (c) treat the turntable axis as an initial guess only and refine with point-to-plane ICP followed by a global (multiway/pose-graph) optimisation so loop-closure error is distributed.
- A cheap stepper turntable has wobble/runout and step error well above 20 microns at the object radius, so pure "rotate by known angle about calibrated axis" registration is insufficient; it needs geometric refinement, or reference spheres/markers fixed to the turntable platter alongside the object to solve each view's pose directly.
- ICP on smooth, symmetric parts (cylinders, planes, spheres) is ill-conditioned along sliding directions; fixture features or marker spheres are needed there.
- Meshing: screened Poisson reconstruction (MeshLab, Open3D, CloudCompare plugin) smooths and can bias sharp edges and thin features by tens of microns depending on octree depth/point weight; for metrology, do measurements (sphere fits, plane fits, cloud-to-CAD distances) on the registered point clouds, and mesh only for visualisation/printing. (Background knowledge; not sourced this session.)
- Tool roles (background knowledge): CloudCompare — fine registration (ICP), primitive fitting, cloud-to-cloud/cloud-to-mesh distance maps; MeshLab — global alignment, Poisson, cleaning; Open3D (Python) — scripted point-to-plane/colored ICP, pose-graph multiway registration, Poisson, TSDF.

### Gaps
- No recent (2020+) source was found reporting post-registration residuals in microns for small-volume structured-light turntable systems.
- Maintenance/version status of MeshLab, CloudCompare and Open3D in 2026 was not retrieved (all were actively maintained as of the researcher's prior knowledge; unverified here).
- No sourced quantification of Poisson-reconstruction bias at micron scales.

---

## 7. Verifying real accuracy at home

### Takeaway
The accepted way is a cut-down VDI/VDE 2634 Part 2 test: scan a precision sphere at ~10 positions (probing error form and size), a two-sphere ball-bar at ~7 positions (sphere-spacing error) and a flat at ~6 positions (flatness), rejecting at most 0.3 percent of points. Reference artefacts must be several times better than the 20 micron target; the DTU artefacts were known to 1-3 microns.

### Cited Findings
- VDI/VDE 2634 Part 2 quality parameters as applied by DTU: probing error form PF (radial range of residuals to a least-squares sphere), probing error size PS (signed difference between fitted and nominal diameter), sphere distance error SD (signed difference between measured and calibrated centre distance), flatness F (range of residuals to a least-squares plane); up to 0.3 percent of worst points may be rejected — [Eiríksson et al., ISPRS 2016](https://isprs-archives.copernicus.org/articles/XL-5-W8/7/2016/isprs-archives-XL-5-W8-7-2016.pdf)
- Positions: PF and PS with one sphere at 10 positions in the measurement volume; SD with the ball-bar at 7 positions; F with the flat at 6 positions — [ISPRS 2016](https://isprs-archives.copernicus.org/articles/XL-5-W8/7/2016/isprs-archives-XL-5-W8-7-2016.pdf)
- Artefact quality used: ceramic-sphere dumbbell with centre distance 198.9612 mm (uncertainty 0.001 mm), sphere diameters 24.9989 and 24.9969 mm with form deviations within about -1.3/+2.0 microns; painted and lapped aluminium flat with deviations -3.0/+1.2 microns; all CMM-measured with 1.8 micron deviation uncertainty — [ISPRS 2016](https://isprs-archives.copernicus.org/articles/XL-5-W8/7/2016/isprs-archives-XL-5-W8-7-2016.pdf)
- Typical VDI artefacts are a painted/lapped aluminium flat and alumina-zirconia ceramic spheres on a carbon-fibre rod — [UTK MTRC structured light scanning artefact](https://mtrc.utk.edu/wp-content/uploads/sites/45/2023/10/Structured-light-scanning-artifact.pdf)
- The standard is well suited to relative measures such as acceptance testing and benchmarking — [ISPRS 2016](https://isprs-archives.copernicus.org/articles/XL-5-W8/7/2016/isprs-archives-XL-5-W8-7-2016.pdf)
- Steel gauge blocks have been scanned (including with microfine talc coating) to study scanner uncertainty and repeatability — [NSF PAR 10490036](https://par.nsf.gov/servlets/purl/10490036)
- CloudCompare has been used for coating-thickness evaluation in a VDI/VDE 2634 Part 3 thesis — [TUM thesis 2017](https://mediatum.ub.tum.de/doc/1619962/e2odjmbd0ri6wtjhdym1axrcr.2017_MELO_Thesis.pdf)
- Sphere-diameter bias is a known signature of phase-shift/Gray-code systems (positive residuals toward lateral edges), so PS reveals systematic errors that PF does not — [ISPRS 2016](https://isprs-archives.copernicus.org/articles/XL-5-W8/7/2016/isprs-archives-XL-5-W8-7-2016.pdf)

### Inferences
- Home artefact kit (background knowledge; prices/grades not sourced): (1) bearing-grade ceramic (Si3N4/ZrO2, matte) or steel balls of ISO 3290 grade G5-G25, sphericity well under 1 micron, diameters to ~1-2 microns — use for PF/PS; (2) a DIY ball-bar from two such balls bonded to a carbon-fibre or Invar rod, with centre distance established by gauge blocks/micrometer or a paid CMM measurement — use for SD; (3) grade 0-2 steel or ceramic gauge blocks wrung to a flat for step-height/length tests; (4) an optical flat, a lapped granite surface plate offcut, or float glass (locally flat to a few microns over ~100 mm but not certified) for F.
- Shiny steel and translucent ceramic both scan poorly; a matting coating is usually needed and adds its own thickness (sublimating scanning sprays are marketed with single-digit-micron layers; talc/developer sprays can be tens of microns and uneven). At 20 microns the coating is a first-order error term and should be reported.
- A sphere-spacing (SD) test is the only one of these that catches scale/calibration error; PF and F mostly measure noise and local distortion. A scanner can show 10 micron flatness and still have 50 micron length error — so do not claim "20 micron accuracy" without a length test spanning the measurement volume.
- CloudCompare workflow: segment the artefact points -> Tools > Fit > Sphere / Plane (reports radius and RMS) -> export scalar field of residuals for range after rejecting the worst 0.3 percent -> compute centre-to-centre distances; or script the same with Open3D/NumPy least-squares fits for repeatability. (CloudCompare menu path from background knowledge; not sourced this session.)
- Repeat the tests cold vs warmed-up and on different days to expose thermal and mechanical drift; and separately test merged multi-view scans (Part 3 style) because registration error is not captured by single-view tests.

### Gaps
- No step-by-step CloudCompare tutorial for VDI-style sphere/plane evaluation was found.
- No sourced price/availability/grade data for precision spheres, gauge blocks or optical flats was retrieved.
- No sourced measurement of coating-spray thickness was retrieved.
- The VDI/VDE 2634 guideline text itself (paywalled) was not consulted; procedure details are as reported by the DTU paper.

---

## 8. Programming routes (Python + OpenCV + camera SDKs) and reference implementations

### Takeaway
A self-written pipeline is very feasible: GenICam-standard Python stacks (Harvesters, Aravis, vendor SDKs) handle the camera, OpenCV handles calibration and triangulation, and several small open repositories show N-step phase shifting with multi-frequency unwrapping. The theory needed is fully covered by the Zuo et al. 2018 review.

### Cited Findings
- Harvesters is an image-acquisition library for Python maintained by the official GenICam committee, working with GenTL producers for USB3 Vision/GigE Vision devices — [PyPI harvesters](https://pypi.org/project/harvesters/0.post0.dev586/)
- Aravis is a glib/gobject-based open library for GenICam cameras; SLStudio ships it as its USB3 Vision/GigE backend on Linux, Windows and macOS — [SLStudio README](https://github.com/jakobwilm/slstudio/)
- `viva-genicam` is a newer pure-Rust GenICam stack with Python wheels for discovering, controlling and streaming GigE/USB3 Vision cameras without Aravis or a C toolchain — [PyPI viva-genicam](https://pypi.org/project/viva-genicam/)
- GenICam `TriggerSource`/`TriggerDelay` features give software or hardware-line triggering through the same API — [MVTec](https://www.mvtec.com/services-support/developers-corner/article/review-of-acquisition-modes); [IDS](https://en.ids-imaging.com/kb-article/techtip-gev-trigger-ctl.html)
- OpenCV contrib `structured_light` supplies Gray-code generation/decoding and sinusoidal (FTP/PSP/FAPS) phase computation with tutorials — [OpenCV tutorials](https://docs.opencv.org/4.x/d3/d81/tutorial_structured_light.html)
- Reference code to read: structured-light-python (pure Python FPP: arbitrary N and periods, hierarchical unwrapping, gamma correction, config-driven; archived Jan 2025) — [GitHub](https://github.com/helleb0re/structured-light-python); SLutils (N-step, multi-frequency and Gray-code unwrapping in C++/CUDA with Python bindings, Apache-2.0) — [GitHub](https://github.com/jhacsonmeza/SLutils); elerac/structuredlight (pattern generation/decoding, MIT) — [ecosyste.ms](https://awesome.ecosyste.ms/projects/github.com%2Felerac%2Fstructuredlight); SLStudio codecs (C++, GPLv3) — [GitHub](https://github.com/jakobwilm/slstudio/)
- Theory: the standard N-step algorithm and its error analysis (noise formula, harmonic sensitivity, gamma compensation, defocus) are laid out in Zuo et al., "Phase shifting algorithms for fringe projection profilometry: A review" (2018) — [OSTI open copy](https://www.osti.gov/pages/servlets/purl/1469785)
- Calibration background referenced by open projects: Feng et al., calibration of fringe projection profilometry review (Optics and Lasers in Engineering, 2021) is cited by structured-light-python as the calibration reference — [GitHub README](https://github.com/helleb0re/structured-light-python)
- DTU's working recipe to imitate in code: 32-step phase shift at period 19.2 px + two lower frequencies for unwrapping, phase matched between two calibrated cameras, then triangulated — [ISPRS 2016](https://isprs-archives.copernicus.org/articles/XL-5-W8/7/2016/isprs-archives-XL-5-W8-7-2016.pdf)

### Inferences
- Minimal self-built pipeline (stereo cameras + uncalibrated projector), using only free components — a sketch from background knowledge, consistent with the cited DTU recipe:
  1. Pattern generation (NumPy): for each of 3 frequencies (e.g. 1, 8, 64-100 periods across the projector width) and N steps (N >= 8 at the top frequency), I_n = 0.5 + 0.45*cos(2*pi*f*x/W - 2*pi*n/N), passed through the inverse-gamma LUT; add horizontal set too if doing projector calibration (not needed for stereo-only).
  2. Display + capture: fullscreen borderless window on the HDMI output (OpenCV `imshow`/GLFW/pyglet), settle delay, software trigger both cameras, exposure = multiple of 1/60 s, save 12-bit raw mono.
  3. Wrapped phase: phi = atan2(-sum I_n sin(2*pi*n/N), sum I_n cos(2*pi*n/N)); modulation B = (2/N)*sqrt(S^2 + C^2) used as a mask.
  4. Temporal unwrapping, coarse to fine: k = round((ratio*Phi_low - phi_high)/(2*pi)); Phi_high = phi_high + 2*pi*k.
  5. Stereo matching: rectify both cameras with OpenCV (`stereoRectify`, `remap`), then for each left pixel find the sub-pixel position on the same row in the right camera with equal absolute phase (linear interpolation of the monotonic phase); with only vertical fringes the epipolar constraint supplies the second coordinate.
  6. Triangulate (`cv2.triangulatePoints` or disparity-to-depth via Q), filter by modulation and left-right consistency, export PLY.
  7. Calibration: `cv2.calibrateCamera` + `cv2.stereoCalibrate` with a restricted model (fix principal point/aspect ratio, k1, k2 only as a starting point per DTU), ~11+ poses over +/-40 degrees, validated on an artefact.
- Camera SDK choice (background knowledge; versions not verified): Basler `pypylon`, FLIR/Teledyne Spinnaker PySpin, The Imaging Source `IC4`, Daheng `gxipy`, Hikrobot MVS Python, and Arducam's USB3 SDK / UVC all have Python paths; UVC webcam-class devices via `cv2.VideoCapture` typically lack true raw output, exposure determinism and trigger control, which matter at 20 microns. `libcamera`/Picamera2 is the Raspberry Pi route and is not directly relevant to a laptop-wired camera except through UVC bridges.
- Prefer monochrome global-shutter sensors with gamma off, fixed gain and raw 10/12-bit output; a Bayer colour sensor loses resolution/adds demosaic artefacts to the phase.

### Gaps
- pypylon, PySpin, Arducam SDK and libcamera documentation were not fetched; their 2026 status and laptop (Windows/Linux x86) support are unverified in this session.
- No single tutorial was found that walks end-to-end through N-step + multi-frequency unwrapping + stereo triangulation in Python with verified accuracy; the user would be combining the repositories and review above.
- Song Zhang's temporal phase unwrapping review ([Purdue PDF](https://engineering.purdue.edu/ZhangLab/publications/papers/2018-ole-unwrapreview.pdf)) is the natural companion reference for step 4 but could not be read (HTTP 503).
