# Countersign

A PDF signing tool that runs entirely in the browser. It is a single HTML file with no build step and no backend. Upload a PDF, add your signature, initials, a date or text, and download the signed copy.

Your documents never leave your device.

## Features

- **Three ways to create a signature:** draw it with a mouse, finger or stylus, type your name in one of six script fonts, or upload a photo or scan (with optional white-background removal).
- **Initials** work the same way as signatures.
- **Date and text fields** with black or blue ink and editable text.
- **Drag, resize and nudge:** click a saved signature, click where it should go on the page, then drag to move and use the corner handle to resize. Arrow keys nudge it (hold Shift for bigger steps) and Delete removes it.
- **Put on every page** copies an item to all other pages, which suits initials.
- **Multi-page viewer** with page navigation and zoom. Pages render lazily, so long documents stay responsive.
- **Saved signatures** can be kept in the browser's local storage for reuse (up to four signatures and four sets of initials).
- **Light and dark themes**, following the system setting.
- **Touch friendly:** works on phones and tablets, with the selected-item panel docked to the bottom on small screens.
- **Handles rotated pages** and PDFs with a crop box when exporting.

## Usage

1. Download `countersign.html`.
2. Open it in a modern browser (double-click it, or serve it from any static host).
3. Upload a PDF with the button or by dropping it onto the page.
4. Create a signature, click it in the left panel, then click the page where it should go.
5. Click **Download signed PDF**. The file is saved as `<original name>-signed.pdf`.

No installation or server is needed.

## How it works

| Step | Library |
| --- | --- |
| Rendering pages to `<canvas>` | [PDF.js](https://mozilla.github.io/pdf.js/) 3.11.174 |
| Embedding signatures and exporting | [pdf-lib](https://pdf-lib.js.org/) 1.17.1 |

- Signatures, initials, dates and text are all rendered to transparent PNG images.
- Placements are stored as fractions of the page width and height, so they stay put when you zoom or resize the window.
- On export, pdf-lib loads the original file bytes, embeds each image, and draws it at the matching position on the correct page. Page rotation and crop boxes are taken into account.
- The original PDF is never modified in place. A new file is generated.

Both libraries are loaded from [cdnjs](https://cdnjs.com/). PDF.js runs its worker code on the main thread, which keeps the tool to a single file and works from `file://`.

## Privacy

- Everything happens client-side. No file is uploaded anywhere.
- Saved signatures are stored in your browser's `localStorage` under the key `countersign.saved.v1`. Delete them from the tool, or clear site data, to remove them.
- The page does make network requests to cdnjs (libraries) and Google Fonts (signature fonts and UI fonts), but never sends your documents.

## Limitations

- **Visual signatures only.** Signatures are stamped onto the page as images. They are not certificate-based digital signatures and give no tamper-evidence or signer identity verification. Legal weight depends on your jurisdiction and use case. If you need audit trails or identity checks, use a dedicated e-signature service.
- Stamped content is flattened into the page and is not selectable text.
- It does not fill in interactive PDF form fields.
- Password-protected PDFs are not supported. Remove the password first.
- An internet connection is needed on first load for the libraries and fonts (see below for going offline).

## Running offline

To remove the CDN dependency, download these files and point the `<script>` tags at local copies:

- `pdf.min.js` and `pdf.worker.min.js` (PDF.js 3.11.174)
- `pdf-lib.min.js` (pdf-lib 1.17.1)

The signature fonts (Caveat, Dancing Script, Mr Dafoe, Yellowtail, Sacramento, Homemade Apple) come from Google Fonts. Without them the typed-signature tab falls back to a generic cursive font.

## Customising

Everything lives in `countersign.html`:

- **Colours and fonts:** edit the CSS variables in the `:root` block at the top of the `<style>` tag.
- **Ink colours:** change the `INK` constant and the `data-color` attributes on the colour buttons.
- **Signature fonts:** edit the `SIG_FONTS` array and the Google Fonts `<link>`.
- **Default sizes:** the signature and initials widths are set in `placeArmed()`.

## Ideas for later

- Multi-party flows with a request link per signer
- Certificate-based signing (this usually needs a small server or a dedicated signing library)
- Filling interactive form fields
- Saving and restoring a signing session
 
