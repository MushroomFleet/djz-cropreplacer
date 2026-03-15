# DJZ CropReplacer

**AI-powered region replacement for images — draw a box, describe the change, done.**

DJZ CropReplacer lets you select any rectangular area of an image, describe what you want there instead, and receive a composited result where the AI-generated patch is seamlessly blended back into your original image. Everything runs in your browser — no uploads, no accounts, no cloud storage. Just your image, your prompt, and your Gemini API key.

---

## What It Does

You draw a box over the part of your image you want to change. The app sends that region to Google's Gemini image model with your text instruction and receives a replacement patch. Before dropping it back into your image, CropReplacer automatically colour-matches the patch to the surrounding area and optionally feathers the edges so the result looks natural rather than pasted-in.

Every edit is saved locally in your browser's storage alongside the original image, the crop coordinates, your prompt, and the raw patch — so you can always trace back what produced a given result or re-edit from any earlier state.

---

## Quick Start

### 1. Get a Gemini API Key

CropReplacer uses Google's Gemini image generation API. You can get a free API key from [Google AI Studio](https://aistudio.google.com/app/apikey). No billing is required for moderate usage.

### 2. Open the App

Open `index.html` in any modern browser (Chrome, Edge, Firefox, Safari). No installation required for the web version.

> **Windows installer coming soon** — a standalone desktop app (MSI installer) is in preparation and will be available shortly. Watch this repository for the release.

### 3. Enter Your API Key

Click **⚙ Settings** in the top right corner. Paste your Gemini API key into the field and choose your preferred model (see [Choosing a Model](#choosing-a-model) below). Click **Save**. Your key is stored only in your browser's local storage and never leaves your device except in the API request itself.

---

## Using the App

### Loading an Image

Click **📁 Load Image** or drag and drop a JPEG, PNG, or WebP file onto the canvas. The image fills the editing area and a thumbnail appears in the **Input Gallery** strip at the bottom of the page. Previously loaded images are available there anytime — click a thumbnail to reload any of them.

### Drawing a Crop Box

Click and drag anywhere on the canvas to draw a selection box over the region you want to replace. The area inside your box stays at full brightness while everything outside dims, so you can see exactly what you've selected.

- Draw in any direction — the box will normalise correctly.
- If you want to redraw, click **🗑 Clear Box** and draw again.
- The selection must be at least 64×64 pixels. If it's too small, the app will tell you before proceeding.

### Opening the Edit Modal

Once you have a box drawn, click **✂ Crop**. The **Edit Region** panel opens showing a preview of your selected area, enlarged to full working resolution.

### Setting Up Your Edit

In the Edit Region panel you'll find:

**EDIT Prompt** — describe what you want in the selected area. Be specific: `"Replace with a red brick wall with ivy"` works better than `"change the wall"`. Leaving this blank will ask the model to fill the area based on context alone.

**Resolution** — choose the output resolution for the generated patch:
- **1K** — fastest, good for quick iterations and smaller crops
- **2K** — a solid balance of quality and speed (recommended starting point)
- **4K** — highest detail, slower and uses more API quota

**Strength** — how freely the model interprets your prompt versus preserving the original content. Higher values produce more dramatic changes; lower values stay closer to the original.

**Colour Match** — when ON (default), the app automatically adjusts the returned patch's colours to match the statistics of the original crop region. This is the most important setting for natural-looking results — keep it ON unless you specifically want the model's raw colour output.

**Edge Blend** — sets a feather radius (in pixels) at the edges of the patch box. A value of 8–16px creates a smooth transition between the patch and your original image. Set to 0 for a hard-edged placement.

### Generating and Applying

Click **🚀 Generate Patch**. The app upscales your crop to one megapixel, sends it to Gemini with your prompt, and waits for the result. A before/after preview appears in the panel when generation is complete.

If you're not happy with the result, adjust your prompt and click **Generate Patch** again — this discards the previous attempt without changing your image.

When you're satisfied, click **Apply to Image**. The patch is colour-matched (if enabled), edge-blended, and composited back into a full copy of your original image. The result appears on the canvas and a thumbnail is added to the **Output Gallery** strip.

---

## Output Gallery

The Output Gallery strip at the bottom of the page shows all your composited results. Click any thumbnail to open a preview with:

- The full composited image
- The original filename, crop coordinates, prompt, model used, and resolution
- A **Download** button to save the result as a PNG
- An **Open in Editor** button to load the result back into the canvas for further editing

---

## Choosing a Model

Two Gemini models are available, selectable in **⚙ Settings**:

| Model | Internal name | Best for |
|---|---|---|
| **Nano Banana 2** *(default)* | `gemini-3.1-flash-image-preview` | Fast iteration, most requests, lower API cost |
| **Nano Banana Pro** | `gemini-3-pro-image-preview` | Maximum quality, detail-critical edits |

Start with Nano Banana 2. Switch to Nano Banana Pro for final renders or when the flash model isn't producing the quality you need. The model you select is saved with each output record so you can always see which model produced a given result.

---

## Tips for Good Results

- **Prompt the content, not the action.** Say `"a wooden door with brass hardware"` rather than `"replace the door"`. Gemini responds to descriptions of what should be there.
- **Match your crop size to your subject.** Tight crops around the exact area you want changed give the model more context per pixel.
- **Use Colour Match + Edge Blend together.** Colour Match handles tone and hue consistency; Edge Blend handles the spatial boundary. Both on, with a 12–16px blend radius, produces the most seamless results.
- **Start at 1K for exploration.** Once you have a prompt that's working, switch to 2K or 4K for the final version.
- **Re-edit outputs.** Open any output in the editor and draw a new box to make a second edit on top of the first. The output is stored as a new input image, so the pipeline chains naturally.

---

## Storage and Privacy

All images are stored in your browser's IndexedDB — a local database that persists between sessions but stays entirely on your device. Nothing is stored on any server. The only external network call is the Gemini API request, which sends the cropped image region and your prompt to Google's API using your own API key.

You can clear all stored images and history at any time from **⚙ Settings → Clear DB**.

---

## Browser Compatibility

CropReplacer runs in any modern browser with Canvas 2D API and IndexedDB support:

| Browser | Supported |
|---|---|
| Chrome 90+ | ✅ |
| Edge 90+ | ✅ |
| Firefox 90+ | ✅ |
| Safari 15+ | ✅ |

A dedicated **Windows desktop installer (MSI)** is coming soon — ideal if you prefer a standalone app without managing a browser tab. Watch this repository for the announcement.

---

## 📚 Citation

### Academic Citation

If you use this codebase in your research or project, please cite:

```bibtex
@software{djz_cropreplacer,
  title = {DJZ CropReplacer: AI-powered region replacement for images},
  author = {[Drift Johnson]},
  year = {2025},
  url = {https://github.com/MushroomFleet/djz-cropreplacer},
  version = {1.0.0}
}
```

### Donate:

[![Ko-Fi](https://cdn.ko-fi.com/cdn/kofi3.png?v=3)](https://ko-fi.com/driftjohnson)
