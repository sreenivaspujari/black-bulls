# MEDI-VISION AI

**See Beyond the Image. Understand the Risk.**

MEDI-VISION AI is a prototype decision-support website for medical imaging. It checks the quality of an uploaded scan, highlights regions that may need attention, explains why, and sorts patients into a review queue so the most urgent cases reach a doctor first.

> ⚠️ **Prototype, not a medical device.** MEDI-VISION does not diagnose disease. The AI findings, heatmaps and confidence scores are **illustrative** (see [What is real and what is simulated](#what-is-real-and-what-is-simulated)). Every case must be reviewed by a qualified healthcare professional.

---

## Problem statement

> Develop a decision-support tool that analyzes approved medical images or clinical datasets to identify potential signs of disease and help healthcare professionals prioritize cases for further assessment.

---

## Features

| Module | What it does |
|---|---|
| **Dashboard** | KPIs (total scans, cases awaiting review, high-priority cases, completed reviews), charts by priority and by body part, and a sortable, searchable patient queue with scan thumbnails |
| **Smart priority system** | Sorts every case into 🔴 Priority 1 (urgent), 🟠 Priority 2 (prompt) or 🟢 Priority 3 (routine) review |
| **Upload any body part** | Add a new patient or a new scan for an existing patient. Supports chest, head, heart, abdomen, kidneys, spine, pelvis, arms and legs, as X-ray, CT, MRI or ultrasound images |
| **Image quality & blur detection** | Measures sharpness (variance of the Laplacian), exposure and contrast, and checks whether the image is grayscale. Poor-quality images show *"Analysis confidence may be affected by image quality. Please upload a clearer image."* |
| **AI heatmap** | Overlays regions that may need further investigation. Switch between the original and the heatmap |
| **Radiology tools** | Zoom, pan, brightness, contrast, rotation, invert and heatmap opacity |
| **Confidence & reliability meter** | Gauge plus a table of potential finding, model confidence, image quality, analysis reliability and review status |
| **"Why was it flagged?" panel** | Plain-language explanation of each flag |
| **Doctor vs AI decision** | Accept, reject or request more exams, with comments. Updates the queue and the activity log |
| **Before vs after comparison** | Slider, side-by-side view, pixel-change highlighting and an examination timeline |
| **3D Medi-Twin** | Interactive 3D body (three.js) that highlights the flagged organ or bone. Rotate, zoom, show front or back, click a body part for details. Has Normal, AI and Comparison views |
| **Report generator** | Structured analysis report with images, observations and doctor's notes. Download as PDF (jsPDF) or copy as text |
| **Voice assistant** | Browser speech recognition, e.g. "show high-priority patients", "open patient MV-1024", "upload scan", "generate medical report". A typed command box works everywhere |
| **Billing (extra module)** | Itemised bills (radiology, lab, pharmacy), GST setting, discounts, insurance, partial payments, multiple payment methods, payment verification, refunds, receipts, overdue alerts, revenue by department and a financial audit trail |
| **Tutorials & help** | Cards for how-to videos, a list of voice commands, and an FAQ |

---

## What is real and what is simulated

Being clear about this matters, especially for judges.

| Real (computed on the image) | Simulated / illustrative |
|---|---|
| Sharpness / blur score (Laplacian variance) | Findings and heatmaps for the demo patients are prepared in advance |
| Exposure and contrast checks | Heatmaps for uploaded scans come from a simple brightness-anomaly heuristic, **not a trained model** |
| Grayscale / colour image check | Model confidence values |
| Pixel-difference change map in Compare | The 3D body is a simplified illustration, not the patient's real anatomy |
| All billing calculations | The demo X-ray and MRI images are drawn by code (synthetic) |

**Model confidence is not diagnostic accuracy.** In a production version, the heuristic would be replaced by a validated model (for example a CNN with Grad-CAM explanations) trained on approved datasets.

---

## Getting started

No build step, no server and no installation. The whole site is a single HTML file.

### Option 1: Open locally
1. Download or clone this repository.
2. Open `index.html` in **Google Chrome** or **Microsoft Edge**.

An internet connection is needed the first time, to load three.js, jsPDF and Google Fonts from their CDNs.

### Option 2: GitHub Pages
1. Push the repository to GitHub.
2. Go to **Settings → Pages**.
3. Under **Source**, choose the `main` branch and the `/ (root)` folder, then save.
4. The site will be live at `https://<your-username>.github.io/<repository-name>/`.

### Browser notes
- **Voice commands** need Chrome or Edge, plus microphone permission. GitHub Pages serves over HTTPS, so the microphone works there.
- **PDF download** works when the page is opened directly or from GitHub Pages. Some embedded previews block downloads; use "Copy text" there instead.

---

## How to use

1. **Dashboard → ＋ New patient**: enter the name, age and sex, choose the body part and image type, and upload a scan (PNG or JPG).
2. The scan opens in **Analyze scan**. Review the quality check, heatmap, confidence and explanation.
3. **Accept**, **Reject** or **Request more exams**, and add comments.
4. Use **⇪ Upload** on a patient's row to add follow-up scans, then open **Compare scans** to see what changed.
5. Open **3D Medi-Twin** to see the flagged body part on the 3D body.
6. Go to **Reports** to download the PDF report.
7. Use **Billing** to view the bill and record payments.

### Voice / typed commands
```
show high-priority patients
open patient MV-1024
new patient
upload scan
compare previous and current scans
generate medical report
open 3D twin
open billing
show dashboard
```

---

## Tech stack

- **HTML, CSS, vanilla JavaScript** in a single file, with no framework
- **[three.js r128](https://threejs.org/)** for the 3D Medi-Twin
- **[jsPDF 2.5.1](https://github.com/parallax/jsPDF)** for PDF reports
- **Canvas API** for image processing (blur detection, heatmaps, change maps)
- **Web Speech API** for voice commands
- **Google Fonts**: Space Grotesk, IBM Plex Sans, IBM Plex Mono

---

## Project structure

```
.
├── index.html   # The complete website (HTML + CSS + JavaScript)
└── README.md    # This file
```

---

## Customisation

All of these settings are near the top of the `<script>` section in `index.html`:

- **Tutorial videos**: paste your YouTube links into the `TUTORIALS` array:
  ```js
  {title:'Getting started in 2 minutes', desc:'...', dur:'2:00', url:'https://www.youtube.com/watch?v=XXXX'},
  ```
- **Body parts**: edit the `BODY` object (label and the 3D region it highlights).
- **Image types and prices**: edit the `MODS` object.
- **Blur thresholds**: in the `quality()` function, `lapVar<25` is Poor and `lapVar<80` is Fair. Tune these values on your own sample images.

---

## Limitations

- **No data is saved.** Patients, uploads, decisions and payments reset when the page is reloaded. There is no backend or database.
- **No trained AI model is integrated.** See [What is real and what is simulated](#what-is-real-and-what-is-simulated).
- **Compare does not align scans automatically.** Scans taken at different positions or exposures will show extra differences.
- **Use only anonymised or approved images.** Uploaded images stay in the browser and are not sent anywhere.

## Future scope

- Integrate a validated deep-learning model (e.g. a chest X-ray classifier) with Grad-CAM heatmaps
- Add a backend (e.g. Flask or Node.js) and a database for patients, scans and billing
- Add DICOM file support
- Add user login and role-based access (doctor, radiologist, billing staff)
- Make the system compliant with healthcare data-protection regulations

---

## Team

| Name | Role |
|---|---|
| _Add team member_ | _Role_ |
| _Add team member_ | _Role_ |

---

## License

_Add a license (for example MIT) before publishing, or state "All rights reserved"._

---

## Disclaimer

MEDI-VISION AI is an academic and hackathon prototype. It is **not** a certified medical device and must **not** be used for clinical diagnosis or treatment decisions. All demo patients are fictional.
