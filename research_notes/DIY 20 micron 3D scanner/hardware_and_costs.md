# Hardware and costs for a DIY 20 micron 3D scanner (laptop-tethered, small objects)

Research date: 2026-11-11. Prices are USD unless marked; EUR/GBP figures are left in the listed currency. Most prices come from search-result snapshots of retailer pages, not from live checkout pages, so treat every figure as "approximately, verify before ordering". Where a price could not be found it is listed under Gaps rather than estimated. Anything in an "Inferences" block is my own reasoning or general optics/engineering background, not a sourced fact.

## Cameras: which low-cost cameras suit DIY structured light, and what do they cost?

### Takeaway
The cheapest credible global-shutter mono options found are the Raspberry Pi Global Shutter camera (IMX296, about $50-64, but it needs a Pi as a host, not a direct laptop tether) and Arducam USB3 mono global-shutter modules at about $150-165 that plug straight into a laptop. A branded USB3 Vision industrial camera (Hikrobot IMX273) was only found at about $330 on a marketplace listing.

### Cited Findings
- Raspberry Pi Global Shutter Camera launched at $50; uses Sony IMX296, 1.6 MP (1456x1088), 3.45 µm pixels, 6.3 mm sensor diagonal, C/CS mount — [PetaPixel](https://petapixel.com/2023/03/09/raspberry-pis-new-global-shutter-camera-costs-just-50/), [Raspberry Pi news](https://raspberrypi.com/news/new-raspberry-pi-global-shutter-camera)
- Current retail for the same camera is higher at some sellers: SparkFun $63.75; Farnell UK £37.59 ex VAT — [SparkFun](https://www.sparkfun.com/raspberry-pi-global-shutter-camera.html), [Farnell UK](https://uk.farnell.com/raspberry-pi/sc0926/global-shutter-camera-computer/dp/4163984)
- Arducam's IMX296 mono module lists external trigger mode support; trigger input on the official Pi board was not confirmed by the pages found — [Arducam IMX296 mono](https://www.arducam.com/1-58mp-imx296-mono-global-shutter-camera-module-with-m12-lens-for-raspberry-pi.html)
- Arducam 2.2MP Mira220 mono global shutter USB 3.0 camera: $149.99 (list $164.99), up to 80 fps, low-distortion M12 lens — [Arducam shop](https://arducam.com/shop/page/3)
- Arducam 5MP OG05B1B mono global shutter USB 3.0 camera, without enclosure: $164.99 special price (was $200.99) — [Arducam product page](https://www.arducam.com/arducam-12mp-imx477p-usb-3-0-camera-with-m12-manual-focus-lens-with-enclosure.html)
- Arducam 2MP OV2311 mono global shutter USB3 evaluation kit with M12 lens: $299.99 — [Arducam](https://www.arducam.com/?p=95836)
- Arducam also sells a 1.58MP IMX296 colour global shutter USB 3.0 module (price not captured) — [Arducam](https://www.arducam.com/?p=102610)
- Hikrobot MV-CS016-10UM (1.6 MP, IMX273 global shutter, mono, USB3 Vision): one eBay.de commercial-seller listing at US $329.12 (listed as "Hikang", authenticity unverified); the distributor spec page shows no price — [eBay.de](https://www.ebay.de/itm/388079105427), [Maxxvision](https://www.maxxvision.com/produkte/kameras/usb3-vision-kameras/2734/mv-cs016-10um)
- Raspberry Pi HQ Camera (12.3 MP IMX477, C/CS mount, rolling shutter): launched at about $50; Waveshare lists $59.99 — [Waveshare](https://www.waveshare.com/product/raspberry-pi-hq-camera.htm), [Digital Camera World](https://www.digitalcameraworld.com/uk/news/raspberry-pi-launches-123mp-camera-module-for-just-dollar50)
- Arducam IMX519 16 MP autofocus camera (the OpenScan Mini default): €39 — [OpenScan Mini](https://openscan.eu/pages/openscan-mini)
- For structured light, a global-shutter camera is preferred over rolling shutter; Polyga's DIY FlexScan3D software supports Basler/FLIR global-shutter cameras with standard HDMI DLP projectors and lists 80 µm accuracy for that class of hardware — [Polyga](https://www.polyga.com/diy-3d-scanner/technical-specifications)
- Without hardware triggering, a commercial projector plus webcam works but capture is slower — [SLStudio README](https://github.com/jakobwilm/slstudio/)
- The camera should have at least the projector's resolution, otherwise the model is limited by camera resolution — [Brown University desktop 3D scanning notes](http://mesh.brown.edu/desktop3dscan/ch3-calib.html)

### Inferences
- Sampling arithmetic (my calculation): to resolve 20 µm features you need roughly 10 µm per camera pixel on the object or finer. A 1456 px-wide IMX296 then covers only about 14.5 mm of object width; a 2.2 MP sensor about 19 mm; a 5 MP sensor (about 2592 px) about 26 mm; a 12 MP IMX477 (4056 px) about 40 mm. So "20 µm on a fist-sized object in one shot" is not available at hobby prices; it needs stitching/turntable views, or the 20 µm figure has to be read as depth noise rather than lateral point spacing.
- Depth noise can be much finer than pixel pitch because phase shifting interpolates sub-pixel, so a 1.6-5 MP mono camera can plausibly deliver 20 µm depth precision over a 30-50 mm field while lateral point spacing stays at 20-35 µm. This distinction (precision vs point spacing vs accuracy) is the same one commercial vendors blur.
- Mono is the better choice at this target: a Bayer sensor loses effective resolution to demosaicing and wastes light with a single-colour projector; colour only buys texture.
- The Pi cameras are cheapest per pixel but break the "cable to a laptop" requirement unless a Pi (not priced here) acts as the capture head and streams to the laptop. The Arducam USB3 UVC modules are the cheapest true laptop-tethered global-shutter mono option found.
- DSLR/mirrorless bodies give the most pixels per dollar if already owned, and suit photogrammetry (OpenScan supports USB-triggered DSLRs), but rolling/mechanical shutters and slow tethered capture make them awkward for many-pattern phase shifting.

### Gaps
- No prices found for Daheng (MER2), MindVision, The Imaging Source, or used FLIR/Basler cameras; the searches returned only Arducam and one Hikrobot marketplace listing.
- No authoritative price for Hikrobot from an authorised distributor.
- Pixel sizes for Mira220, OG05B1B and IMX477 were not captured from sources in this session.
- Whether the official Raspberry Pi GS camera's trigger pads are documented was not confirmed here (only the third-party Arducam module's trigger support was).
- No source found comparing webcams quantitatively for structured light.

## Lenses: C-mount, macro and telecentric options, prices, and depth of field

### Takeaway
Hobby C-mount lenses for the Pi HQ/GS cameras cost about $33-61; real telecentric 0.5x lenses start around €233 (VA Imaging) and run $800-2,500 for Computar/Opto Engineering. Depth of field at these magnifications is only a few millimetres.

### Cited Findings
- Waveshare C-mount lenses for the Pi HQ camera: 16 mm about $59.63-60.99; 25 mm about $32.63-33.99; 35 mm about $38.63-39.99; 6 mm wide angle about $23.63-24.99; a "100X industrial microscope lens" C/CS mount about $14.31-14.99 — [Waveshare camera listing](https://www.waveshare.com/product/raspberry-pi/cameras/10mp-pixels.htm?dir=asc&mode=grid)
- The 16 mm C-mount lens is listed at US$81.77 at another retailer — [Abra Electronics](https://abra-electronics.com/robotics-embedded-electronics/raspberry-pi-en-3/cameras-en-2/16mm-telephoto-lens-for-raspberry-pi-high-quality-camera.html?sl=en)
- VA Imaging 0.5x telecentric, 2/3" format, 110 mm working distance, coaxial light port, distortion < 0.05%: €233 — [VA Imaging](https://va-imaging.com/products/telecentric-lens-0-5x-wd110-1-5-coaxial-light-c-mount)
- Computar TEC-V05110MPW 0.5x telecentric, 1" 5 MP, 110 mm working distance: $799 — [cctv.net](https://www.cctv.net/computar-tec-v05110mpw-computar-tec-v05110mpw-5mp-1-110mm-telecentric-lens-5x-magnification-c-mount)
- Opto Engineering 0.50x telecentric (TCCX050-W): $2,541 — [Machine Vision Store](https://machinevisionstore.com/catalog/details/1387)
- Edmund Optics 0.5x 110 mm WD in-line CompactTL telecentric (#67-303) lists depth of field ±1.9 mm (20% contrast at 20 lp/mm) — [Edmund Optics](https://www.edmundoptics.com/p/0.5x-110mm-wd-in-line-compacttl-telecentric-lens/21811/)
- Keyence CA-LMH05 0.5x telecentric macro lens lists 0.00% TV distortion (2/3") and 3.1 mm depth of field — [Keyence](https://www.keyence.com/products/vision/vision-sys/ca-l/models/ca-lmh05/)

### Inferences
- The two telecentric data points (±1.9 mm and 3.1 mm DOF at 0.5x) show the central optical constraint: at the magnification needed for about 7-10 µm/pixel sampling, usable depth is roughly 2-4 mm per view. A coin fits; a fist-sized object needs focus stacking, a Z stage, or acceptance of coarser resolution.
- General optics background (not sourced in this session): total DOF is approximately 2·N·c·(1+m)/m², so stopping down helps, but diffraction blur (roughly 1.2·λ·N_effective) grows too; beyond about f/8 nominal at 0.5x the diffraction spot exceeds 10 µm in object space and erodes the resolution you stopped down to protect.
- At 0.3-0.5x, a cheap 25-35 mm C-mount lens on extension tubes/spacers is the lowest-cost route; the hobby lenses above are sold as "telephoto" CCTV-type lenses, not low-distortion metrology lenses, so distortion must be removed by calibration and edge sharpness may limit real resolution.
- The €233 VA Imaging telecentric is the notable bargain if a constant-magnification, near-zero-distortion view is wanted; its 2/3" image circle covers the small sensors above.

### Gaps
- No prices found for branded low-distortion fixed-focal C-mount lenses (Computar, Kowa, Hikrobot MVL, Edmund) or for generic AliExpress "5MP/10MP low distortion" C-mount lenses.
- No price found for the Edmund #67-303 or Keyence lens.
- No measured MTF or distortion data found for the cheap Waveshare/Arducam C-mount lenses.
- No sourced DOF calculator or formula reference was retrieved; the formula above is from background knowledge.

## Projectors and light sources: DLP EVMs, pico projectors, LCD vs DLP, and laser-line alternatives

### Takeaway
The cheapest programmable DLP engine is TI's DLP2000 EVM at about $99-102, but it is a bare module that needs a host board; the DLP230NP EVM is about $359 and the pattern-sequencing LightCrafter 4500 about $1,300-1,690. Laser-line turntable scanners (Ciclop/FabScan) cost about $99-250 but are documented at 0.1-0.5 mm, far from 20 µm.

### Cited Findings
- DLP LightCrafter Display 2000 EVM (DLPDLCR2000EVM): launched at $99; DigiKey Austria lists $102.24 with 10 in stock; element14 Singapore lists S$165.11 with a 55-week lead time — [DigiKey](https://www.digikey.at/en/product-highlight/t/texas-instruments/dlp-lightcrafter-display-2000-eval-module), [element14 SG](https://sg.element14.com/texas-instruments/dlpdlcr2000evm/evaluation-module-display/dp/3009788)
- DLP LightCrafter Display 230NP EVM (DLPDLCR230NPEVM): $358.80 at quantity 1 on a broker listing; TI's page says not available for direct purchase on TI.com; DigiKey China shows ¥3,258.56 inc. 13% VAT with 12-week lead time — [ichome](https://ichome.com/product-detail/texas-instruments/dlpdlcr230npevm), [TI](https://www.ti.com/tool/dlpdlcr230npevm), [DigiKey CN](https://www.digikey.cn/en/products/detail/texas-instruments/DLPDLCR230NPEVM/13531286)
- DLP LightCrafter 4500 (DLPLCR4500EVM): DigiKey UK £1,196.07 ex VAT; aggregator snapshots show about $1,559-1,624 at DigiKey US, $1,299 at TI and $1,688.82 at Mouser; 12-week standard lead time — [DigiKey UK](https://www.digikey.co.uk/en/products/detail/texas-instruments/DLPLCR4500EVM/4288515), [ciiva aggregator](https://ciiva.com/part/dlplcr4500evm-11682000), [edinventory aggregator](https://edinventory.com/parts/dlplcr4500evm_texas-instruments)
- Pico projectors have been used successfully for structured light; projector native resolution caps pattern resolution, and many TI DLP Pico DMDs have pixels rotated 45° (diamond array) — [Brown University notes](http://mesh.brown.edu/desktop3dscan/ch3-calib.html)
- A research comparison characterises LCD projectors as about 60 Hz with arbitrary patterns and DLP as 10 kHz or more with binary patterns; defocused binary patterns are used to sidestep projector gamma, and phase shifting works even with nearly focused binary patterns, with more steps (e.g. nine-step) reducing error — [Purdue Zhang Lab paper](https://engineering.purdue.edu/ZhangLab/publications/papers/2017-ole-microcompare.pdf)
- With short camera exposures, a DLP's time-multiplexed grey levels can be posterised in the captured image — [arXiv 1706.02698](https://arxiv.org/pdf/1706.02698.pdf)
- A patent filing argues DLP projectors show temperature-driven drift and proposes a binary mask plus LEDs as a cheaper, more stable alternative — [USPTO 11085761](https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/11085761)
- TI positions DLP structured light from micrometres to centimetres depending on sensor resolution; its sub-micron claims refer to high-end chipsets, not hobby EVMs — [TI DLPA122](https://www.ti.com/document-viewer/lit/html/DLPA122A/GUID-0F0A1278-7922-4E22-87BD-745B9F113DF3), [TI TIDA-00362 reference design](https://www.ti.com/tool/TIDA-00362)
- Laser-line turntable kits: CowTech Ciclop kit $99; BQ Ciclop listed at $219 in a review and €249.90 at a European retailer that marks it discontinued — [3DPrint.com](https://3dprint.com/120057/cowtech-ciclop-3d-scanner-kit/amp/), [3DSourced](https://www.3dsourced.com/rankings/best-diy-3d-scanner/), [Bot'n Roll](https://www.botnroll.com/en/scanners/1429-kit-ciclop-diy-3d-scanner-red.html)
- Ciclop accuracy figures conflict: 0.3-0.5 mm resolution in most sources, 100-300 µm in another review, and one hands-on reviewer claiming 0.07 mm — [3DSourced](https://www.3dsourced.com/rankings/best-diy-3d-scanner/), [Pick3DPrinter](https://pick3dprinter.com/?p=27178)
- FabScan Pi is usually self-printed, with parts costing roughly $95-120; the original FabScan was pitched as a 100-euro laser scanner; no measured accuracy figure was found — [3DSourced](https://www.3dsourced.com/rankings/best-diy-3d-scanner/), [RWTH Aachen FabScan](https://hci.rwth-aachen.de/fabscanoriginal?rCH=2), [FabScanPi docs](https://fabscanpi-server.readthedocs.io/en/latest/steps_ciclop.html)
- A 2011 Hackaday build used a Microvision SHOWWX laser pico projector and a modified webcam; focus-free laser projection gave good close-range depth, no accuracy number reported — [Hackaday](https://hackaday.com/2011/10/08/structured-light-3d-scanner/)

### Inferences
- For phase shifting, the practical hobby choice is a single-colour (LED) DLP engine driven over HDMI/parallel RGB as a second display, with camera exposure set to a whole number of projector frames (e.g. 1/60 s multiples) to average out DLP time multiplexing; this follows from the posterisation finding above. A colour-wheel consumer DLP adds a second timing artefact for the same reason. This sync advice is my inference; no DIY guide documenting it was found.
- The DLP2000 (nHD class resolution per my background knowledge, not sourced here) is cheap, but projector resolution is not the limit in phase shifting: phase is interpolated, so fringe contrast, linearity and stability matter more than pixel count. Its real costs are the host board/adaptor and the fact that its lens is set up for projection distances, so close focus needs lens refocusing or an added close-up lens.
- The LightCrafter 4500 is the "proper" engine (hardware-triggered pattern sequences), but at $1,300+ it alone exceeds the price of a commercial 0.02 mm-class scanner, so it does not belong in a cheapest-viable build.
- Laser-line plus turntable is a dead end for 20 µm: published figures are 5-25 times worse, and laser speckle plus line-width limits apply regardless of camera.

### Gaps
- No current prices found for consumer DLP pico projectors (AliExpress/Amazon) or used business DLP projectors, and no documented price for projector lens-refocus modifications.
- No DIY source found that documents gamma calibration, colour-wheel sync or exposure settings for a specific cheap projector.
- Native resolution and brightness of the DLP2000/DLP230NP EVMs were not captured from TI pages in this session.
- No prices found for laser line modules on their own.
- Thunk3D and Seeed/DLP DIY kits: nothing retrieved.

## Motion: turntables, controllers and linear stages

### Takeaway
A stepper turntable is the cheap part of the build: a NEMA 17 motor is about €16 and published DIY turntables use an Arduino Uno plus a commodity stepper driver. No priced source for a precision linear stage was found.

### Cited Findings
- NEMA 17 stepper motor (13 Ncm or 40 Ncm) sold by OpenScan: €16 — [OpenScan Mini](https://openscan.eu/pages/openscan-mini)
- An open turntable project for a camera-projector scanner uses an Arduino Uno, a NEMA 17 and a stepper driver module, with the turntable itself built from LEGO — [GitHub marcelbonnici/turntable](https://github.com/marcelbonnici/turntable)
- An academic off-the-shelf structured-light build drives its turntable from an Arduino and a switched-mode supply in 1° steps — [arXiv 2209.12101](https://arxiv.org/pdf/2209.12101)
- A Hackaday photogrammetry turntable uses a stepper, Arduino and EasyDriver in 10° steps, with an IR LED to fire the camera — [Hackaday](https://hackaday.com/2018/08/21/automated-turntable-for-3d-scanning)
- An OpenBuilds structured-light turntable scanner build exists as a reference design (details not retrieved) — [OpenBuilds](https://openbuilds.com/builds/structured-light-turntable-scanner.3773/)

### Inferences
- Turntable angular accuracy need not be trusted at the 20 µm level if views are registered by geometry (ICP) or by calibrated-axis plus refinement; the turntable's job is repeatable coverage, so a $15-30 motor/driver/microcontroller set is adequate. Runout and wobble matter only if registration relies purely on the commanded angle.
- Because depth of field is only a few millimetres (see Lenses), a small Z or focus stage becomes more valuable than a fancy turntable for anything thicker than a coin.

### Gaps
- No sourced prices for Arduino/ESP32 boards, stepper drivers (A4988/TMC2209), or cheap linear stages (manual micrometer stages, lead-screw rails); these are commodity items but were not looked up.
- No measured accuracy for any NEMA 17 turntable structured-light rig was found.

## Calibration targets and verification artifacts

### Takeaway
Calibration is where a 20 µm build spends real money: calib.io targets start at about €103, Edmund glass checkerboards run roughly €400-935, and a usable gauge-block set costs about $140 (economy) to $550+ (grade 0). OpenScan's own test used two precision steel spheres costing "more than €100".

### Cited Findings
- calib.io starting prices: checkerboard from €103; circle and asymmetric circle targets from €103; ChArUco from €116; checkerboard-marker from €114; Halcon from €127; custom from €159; Kalibr from €176 — [calib.io products](https://calib.io/collections/products)
- calib.io aluminium/LDPE composite targets are recommended by several calibration guides because flatness and pattern errors propagate directly into the calibration — [Asyril documentation](https://doc.eyeplusxtd.asyril.com/en/1.1/support/knowledge_database/camera_config_pattern.html), [lensboy calibration guide](https://robertleoj.github.io/lensboy/calibration_guide.html)
- Edmund Optics 20 x 20 mm glass/opal checkerboard target: roughly €400-510 depending on region; a NIST-certified 80 x 80 mm opal checkerboard: €935 — [Edmund 20x20](https://edmundoptics.co.uk/p/20-x-20mm-glass-checkerboard-target/43591), [Edmund 80x80 NIST](https://edmundoptics.co.uk/p/80-x-80mm-opal-checkerboard-target-nist-certified/53446)
- Some Edmund glass grid targets specify dot centre-to-centre accuracy of ±0.001 mm — [Edmund checkerboard family](https://edmundoptics.co.uk/f/checkerboard-calibration-targets/39496/)
- Imatest warns that even 0.1% deviation from squareness can invalidate calibration at very high accuracy requirements — [Imatest](https://www.imatest.com/docs/geometric-calibration/supported-targets)
- Gauge block sets: Tormach 36-piece economy steel set $139.50 with calibration certificate; Dasqua 81-piece Grade 2 £119; SPI 36-piece Grade 0 $549.95; Precise 81-piece Grade A+ $899.95 with NIST-traceable certificate — [Tormach](https://tormach.com/gauge-block-set-36pcs-33181.html), [Chronos](https://www.chronos.ltd.uk/?p=70134392), [Penn Tool SPI](https://penntoolco.com/spi-gage-block-set-rectangular-blocks-36-pc-set-grade-0-30-914-6), [Penn Tool Precise](https://www.penntoolco.com/precise-81-piece-1001-thru-4-grade-a-precision-gage-block-set-paa-81-57-046-004/)
- OpenScan's accuracy test used two chrome-alloy steel spheres (nominal 20 mm and 25 mm, claimed tolerance ±0.0025 mm, not lab-certified) that cost "more than 100€", coated with Aesub Orange vanishing scanning spray — [OpenScan blog](https://blog.openscan.eu/posts/chasing-microns-openscan-mini-accuracy-test-part-2/)
- Calibration accuracy directly determines reconstruction accuracy in structured light; NIST catalogues the error sources — [NIST, Sources of Errors in Structured Light 3D Scanners](https://tsapps.nist.gov/publication/get_pdf.cfm?pub_id=927473)

### Inferences
- For a 20-40 mm field, the target itself must be small (features of 1-2 mm) and accurate to a few microns; a laser-printed paper target glued to glass will not hold that (printer scale error and paper flatness are typically tens of microns or worse; this is my estimate, not sourced). A small photolithographic glass/ceramic target is the item that cannot be faked.
- Verification should be separate from calibration: a ball-bar or two-sphere artifact checks scale and form, a pair of wrung gauge blocks checks step height. A single gauge block or a couple of loose bearing balls costs far less than a full set, so the verification budget can be about $20-50 rather than $140+ (estimate, not sourced).
- Scanning spray is a real consumable: shiny steel references and most metal parts cannot be scanned at this level without it, and the coating thickness itself adds microns of error.

### Gaps
- calib.io size, substrate and tolerance options and their prices were not retrieved (only "from" prices).
- No prices found for cheap Chinese glass/ceramic dot or checker targets (AliExpress/Taobao), which are the obvious budget route.
- No prices found for individual gauge blocks, grade 5/10 bearing or ceramic balls, ball bars, or Aesub spray.
- No source retrieved on the accuracy of displaying a calibration pattern on a phone/tablet/LCD screen as a cheap flat target.

## Rigidity and thermal stability

### Takeaway
Sources agree that rigidity between camera and projector is the first hardware priority and that projector thermal drift is a known error source, but no source found gives quantitative guidance or prices for hobby frames.

### Cited Findings
- The SLTK hardware guide puts camera-projector rigidity as the top priority, with lens distortion calibrated against a known object — [SLTK Hardware Setup Guide](https://github.com/jhdewitt/sltk/wiki/Hardware-Setup-Guide)
- DLP projectors are described as subject to temperature-driven drift — [USPTO 11085761](https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/11085761)
- OpenScan ran its accuracy test at 20 °C ± 2 K across eight sets over two days — [OpenScan blog](https://blog.openscan.eu/posts/chasing-microns-openscan-mini-accuracy-test-part-2/)
- NIST lists mechanical and thermal effects among structured-light error sources — [NIST](https://tsapps.nist.gov/publication/get_pdf.cfm?pub_id=927473)

### Inferences
- Background-knowledge arithmetic (not sourced): aluminium expands about 23 µm per metre per kelvin, so a 150 mm camera-projector baseline grows about 3.5 µm per kelvin; a few kelvin of warm-up from an LED projector therefore consumes a meaningful slice of a 20 µm budget, and a printed PLA/PETG bracket (several times higher expansion, plus creep) is worse. Practical consequences: warm up for 20-30 minutes before calibrating, calibrate and scan in the same session, and keep the baseline on one solid aluminium plate rather than printed parts.
- A short, stiff single plate or a small section of extrusion is more valuable than an optical breadboard at this scale; the breadboard buys reconfigurability, not accuracy.

### Gaps
- No prices retrieved for aluminium extrusion, small optical breadboards (Thorlabs or generic), or mounts.
- No hobby build was found that publishes a drift-versus-temperature measurement.

## Existing open hardware builds: BOM cost and measured accuracy

### Takeaway
OpenScan Mini (photogrammetry, kit from €232) is the only open design found with a published, reasonably careful accuracy test: about 0.03 mm (2σ) surface deviation on 20-25 mm spheres. No open structured-light project found publishes both a BOM total and a measured accuracy near 20 µm.

### Cited Findings
- OpenScan Mini: kit "from €232" (the homepage also showed "from €203"); printed parts from €55; scan volume about 9x9x9 cm; vendor states accuracy "up to 0.02 mm" and that it "highly depends on the object preparation and user capability" — [OpenScan Mini](https://openscan.eu/pages/openscan-mini), [OpenScan home](https://www.openscan.eu/)
- OpenScan Classic is listed from €203 with a scan volume of about 18 cm — [OpenScan Classic](https://openscan.eu/openscan-classic)
- OpenScan Mini sphere test: fitted radii 12.493 mm (nominal 12.500) and 9.985 mm (nominal 10.000); surface deviation 2σ of 0.024 and 0.028 mm; 150 photos per set, about 11 minutes capture plus 6-8 minutes cloud processing; cross-polarised light and scanning spray; scale set manually in Meshmixer from the 25 mm sphere; the author says a valid accuracy statement cannot yet be made — [OpenScan blog, part 2](https://blog.openscan.eu/posts/chasing-microns-openscan-mini-accuracy-test-part-2/)
- An earlier OpenScan post claimed 0.008 mm from a feeler-gauge test; the author later acknowledged it was seen as click-bait and suggested 0.02 mm as more conservative — [OpenScan blog, part 1](https://blog.openscan.eu/posts/0-008mm-accuracy-with-the-openscan-mini-tutorial-mesh-analysis-using-cloudcompare/)
- Hackster reported sub-50 µm with the Pi Camera v2.1 and around 10 µm with the Pi HQ camera for the OpenScan system — [Hackster](https://www.hackster.io/news/thomas-megel-s-openscan-offers-10-micron-3d-scanning-on-a-raspberry-pi-and-hq-camera-module-308a2540c5b8)
- Open structured-light software/projects found: SLStudio (GPLv3, real-time camera-projector framework, 2x3 phase shifting, 20 Hz+ with triggering); sls-scan (Linux, two synchronised cameras plus pattern projector, in progress); fringe-projection-3d-reconstruction (consumer camera plus mini projector, 4-step phase shift, multi-frequency unwrapping); pranavkantgaur/3dscan (Gray code plus phase shift); elerac/structuredlight (pattern generation/decoding library); SLTK toolkit — [SLStudio](https://github.com/jakobwilm/slstudio/), [sls-scan](https://github.com/Jbsco/sls-scan), [fringe-projection](https://github.com/hectorcs23/fringe-projection-3d-reconstruction), [3dscan](https://github.com/pranavkantgaur/3dscan), [structuredlight](https://github.com/elerac/structuredlight), [SLTK](https://github.com/jhdewitt/sltk/wiki/Hardware-Setup-Guide)
- SL Sensor is an open-source ROS structured-light sensor aimed at high-accuracy construction robotics (research-grade hardware) — [arXiv 2201.09025](https://arxiv.org/pdf/2201.09025)
- A NIST document cites a custom research structured-light scanner at about 0.032 to under 0.050 mm using a commercial projector — [NIST](https://tsapps.nist.gov/publication/get_pdf.cfm?pub_id=927473)
- Ciclop and FabScan Pi figures are in the projector/laser section above: $99-250, 0.1-0.5 mm class — [3DSourced](https://www.3dsourced.com/rankings/best-diy-3d-scanner/)

### Inferences
- The best documented hobby result (OpenScan, about 0.03 mm 2σ form deviation, with a 7-15 µm radius bias and an admitted weak scaling method) is roughly at the target but only on ideal matte-sprayed spheres with texture supplied by the spray; photogrammetry has no absolute scale and fails on featureless surfaces without spray.
- Since even careful research rigs with commercial projectors are quoted at 32-50 µm, a hobby structured-light build should expect 30-100 µm verified accuracy at first, with 20 µm achievable as noise/repeatability on a small field before it is achievable as traceable accuracy.

### Gaps
- No BOM totals or measured accuracies were found for any open structured-light build; none of the GitHub projects surfaced publishes them in the material retrieved.
- Nothing retrieved on "Hesam H", 3DUnderworld, Thunk3D or Seeed/DLP DIY scanners; I could not confirm their current status.
- No 2024-2026 open-source structured-light project with a hardware BOM was found; the newer repositories found are software only.
- What the base €232 OpenScan Mini kit includes (Raspberry Pi, camera) is not stated on the page retrieved.

## Example builds and totals (cheapest-viable and upgraded)

### Takeaway
Using only prices found in this session, a cheapest-viable structured-light parts list lands around $450-600 and an upgraded one around $900-1,300, which overlaps or exceeds the $720-960 commercial scanners claiming 0.02 mm. The cheapest DIY route with published near-target results is OpenScan-style photogrammetry at about €230-330.

### Cited Findings
- Component prices used below are the cited ones above: Arducam Mira220 mono USB3 $149.99 and OG05B1B 5 MP mono USB3 $164.99 ([Arducam](https://arducam.com/shop/page/3)); 25 mm and 35 mm C-mount lenses about $33-40 ([Waveshare](https://www.waveshare.com/product/raspberry-pi/cameras/10mp-pixels.htm?dir=asc&mode=grid)); DLP2000 EVM $102.24 ([DigiKey](https://www.digikey.at/en/product-highlight/t/texas-instruments/dlp-lightcrafter-display-2000-eval-module)); DLP230NP EVM $358.80 ([ichome](https://ichome.com/product-detail/texas-instruments/dlpdlcr230npevm)); calib.io target from €103 ([calib.io](https://calib.io/collections/products)); NEMA 17 €16 ([OpenScan](https://openscan.eu/pages/openscan-mini)); VA Imaging 0.5x telecentric €233 ([VA Imaging](https://va-imaging.com/products/telecentric-lens-0-5x-wd110-1-5-coaxial-light-c-mount)); Tormach gauge block set $139.50 ([Tormach](https://tormach.com/gauge-block-set-36pcs-33181.html)); OpenScan Mini kit from €232 ([OpenScan](https://openscan.eu/pages/openscan-mini))

### Inferences
All totals below are my own sums. Lines marked "est." are unsourced allowances for commodity parts whose prices were not looked up, and should be checked.

- Build A, photogrammetry baseline (OpenScan Mini style): kit from €232; scanning spray and polariser est. €30-60; reference spheres est. €20-100. Total roughly €280-390. Published result about 0.03 mm 2σ on sprayed spheres. Caveat: the kit is Raspberry Pi based, so it is networked to the laptop rather than a pure USB tether.
- Build B, cheapest-viable structured light, single camera plus projector:
  - Arducam Mira220 mono USB3 global shutter: $150
  - 25 or 35 mm C-mount lens plus spacers (lens mount adaptor needed if the module is M12 only; not verified): $35-40, spacers est. $10
  - DLP2000 EVM: $102, plus host/adaptor board and cabling est. $30-70 (the EVM is not a plug-in HDMI projector)
  - NEMA 17 plus driver plus microcontroller: about $17 for the motor, est. $15 for the rest
  - Small accurate calibration target: €103 and up from calib.io, or an unpriced cheaper glass target
  - Aluminium plate/extrusion, mounts, fasteners: est. $40-60
  - Verification artifacts and spray: est. $40-60
  - Total roughly $540-650 with a calib.io target; roughly $450 if a sub-$30 glass target can be sourced (unverified).
  - Expected outcome (my judgement): 20-40 µm noise on a field of about 20-30 mm, verified accuracy more like 30-100 µm until calibration is refined.
- Build C, upgraded:
  - Arducam OG05B1B 5 MP mono USB3: $165 (or two cameras for stereo plus projector, $330, which removes the need to calibrate the projector as an inverse camera)
  - VA Imaging 0.5x telecentric: about €233, or a better fixed-focal lens (unpriced)
  - DLP230NP EVM: $359 (needs a Raspberry Pi class host per TI's design, not priced), or stay with the DLP2000
  - calib.io or glass target: €103-160
  - Gauge blocks (Tormach economy set): $140
  - Turntable, frame, spray as above: est. $100-150
  - Total roughly $1,000-1,300 single camera, more for stereo.
- Cost conclusion: Build B is not clearly cheaper than a $720-830 commercial unit once the calibration target and artifacts are included, and it costs weeks of software and calibration work. DIY wins on cost only via Build A, or if cameras, lenses or a projector are already owned.

### Gaps
- Every "est." line above lacks a source.
- Whether the Arducam USB3 modules accept C-mount lenses directly or only M12 was not verified.
- No used-market prices (eBay FLIR/Basler, used DLP projectors) were retrieved, though that is likely the best value route.

## Comparison with the cheapest commercial scanners claiming 0.02 mm (2026 prices)

### Takeaway
Commercial "0.02 mm" scanners cost about $720-960 in 2026 (Creality CR-Scan Otter, Revopoint MINI 2), and their 0.02 mm figure is a best-case single-frame or point-distance number, not independently verified volumetric accuracy.

### Cited Findings
- Revopoint MINI 2: about $829 for the Standard edition per a 2026 buyer's guide (checked July 2026 per the search summary); other listings show $743, $912 and $1,014 (Advanced edition) — [StackSheriff](https://stacksheriff.com/3d-printing/best-3d-scanner-miniatures/), [Filamino best scanners 2026](https://filamino.com/blog/best-3d-scanners-2026), [Revopoint trade-in page](https://global.revopoint3d.com/products/trade-in-mini-2), [Reach listing, Advanced Edition](https://m.reach.dog/shop/robotshop-usa/products/ysystems-kokusai-revpoint-mini-2-3d-scanner-blue-light-precision-002mm-advanced-edition)
- MINI 2 specifications are stated inconsistently: single-frame precision up to 0.02 mm with accuracy up to 0.05 mm at some retailers; Revopoint's table gives 0.05 mm single-frame accuracy and point distance up to 0.02 mm; figures are lab single-frame values not representative of real-world use — [3D Printing Industry](https://3dprintingindustry.com/news/a-closer-look-at-revopoints-new-mini-2-3d-scanner-technical-specifications-and-pricing-227920/), [3DSourced review](https://www.3dsourced.com/3d-scanner-reviews/revopoint-mini-2-review/), [MatterHackers](https://www.matterhackers.com/store/l/revopoint-mini-2-3d-scanner/sk/MVT5SWF8)
- Creality CR-Scan Otter: list $959, MatterHackers $899; Micro Center $799.99 (was $899.99); a Walmart marketplace listing $719 (was $899) — [MatterHackers](https://www.matterhackers.com/store/l/creality-cr-scan-otter-3d-scanner/sk/M5XL0M9L), [Micro Center](https://www.microcenter.com/quickview/683410/creality-cr-scan-otter-3d-scanner), [Walmart](https://www.walmart.com/ip/17463456402), [Stemfinity](https://stemfinity.com/products/14-day-return-refund-14-day-return-refund-100-secure-checkout-100-secure-checkout-up-to-12-month-warranty-up-to-12-month-warranty-lifetime-customer-support-lifetime-customer-support-cr-scan-otter-3d-scanner)
- The Otter's accuracy is stated as "up to 0.02 mm at 60 mm" working distance — [Fruugo listing](https://www.fruugo.ca/creality-3d-scanner-otter-for-3d-printing-reverse-engineering-handheld-scanner-with-002mm-accuracy/p-436437981), [Walmart](https://www.walmart.com/ip/17463456402)
- For scale, Polyga's hobby-buildable hardware class is quoted at 80 µm — [Polyga](https://www.polyga.com/diy-3d-scanner/technical-specifications)

### Inferences
- The commercial "0.02 mm" is the same kind of number a DIY rig can reach as noise on a small field; neither is a traceable 20 µm accuracy. The honest comparison is therefore: about $720-830 for a finished tool with software and tracking versus about $450-650 plus substantial effort for DIY structured light, or about €300 for OpenScan photogrammetry with slow capture.
- A DIY rig has one real advantage: a fixed, rigid, small-field macro setup with a telecentric or long lens can be optimised for coin-sized objects in a way handheld scanners are not, so DIY can plausibly beat the commercial units on fine surface detail for very small parts while losing on convenience and object size range.

### Gaps
- No prices or specifications retrieved for the 3DMakerpro Seal, Creality Raptor or Shining3D Einstar.
- No independent gauge-block or sphere test of the Revopoint MINI 2 or CR-Scan Otter was found; available reviews are qualitative.
- Attribution of the $829 and $743 MINI 2 figures to a specific page is uncertain, since they came from a search summary spanning several listings.
