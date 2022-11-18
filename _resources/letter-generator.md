---
title: "Grievance Letter Generator"
collection: resources
page_js:
 - https://unpkg.com/pdf-lib/dist/pdf-lib.min.js
 - https://unpkg.com/@pdf-lib/fontkit/dist/fontkit.umd.js
---


Fill in the forms below and download the pdf.

<script>

  const fetchBinaryAsset = (asset) =>
    fetch(`/assets/${asset}`).then((res) => res.arrayBuffer());

  const fetchStringAsset = (asset) =>
    fetch(`/assets/${asset}`).then((res) => res.text());

  const renderInIframe = (pdfBytes) => {
    const blob = new Blob([pdfBytes], { type: 'application/pdf' });
    const blobUrl = URL.createObjectURL(blob);
    document.getElementById('iframe').src = blobUrl;
  };

  const downloadPdf = (pdfBytes) => {
    const link = document.createElement('a');
    const blob = new Blob([pdfBytes], { type: 'application/pdf' });
    link.href = URL.createObjectURL(blob);
    link.download = "CSEA_Letter.pdf";
    document.body.append(link);
    link.click();
    link.remove();
    // in case the Blob uses a lot of memory
    setTimeout(() => URL.revokeObjectURL(link.href), 7000);
  };

  async function test() {
  const {
    clip,
    clipEvenOdd,
    closePath,
    cmyk,
    degrees,
    drawRectangle,
    endPath,
    grayscale,
    LineCapStyle,
    setLineJoin,
    LineJoinStyle,
    typedArrayFor,
    lineTo,
    moveTo,
    PDFDocument,
    popGraphicsState,
    pushGraphicsState,
    rgb,
    StandardFonts,
    AFRelationship,
  } = PDFLib;

// These should be Uint8Arrays or ArrayBuffers
// This data can be obtained in a number of different ways
// If your running in a Node environment, you could use fs.readFile()
// In the browser, you could make a fetch() call and use res.arrayBuffer()

const fonturl = "https://ncseasonal.com/assets/fonts/Carolina.ttf"
const fontBytes = await fetch(fonturl).then((res) => res.arrayBuffer())

// Load a PDF with form fields
const pdfDoc = await PDFDocument.load(
  await fetchBinaryAsset('pdfs/Letter to CSEA-Fillable.pdf'),
);



// Get the form containing all the fields
const form = pdfDoc.getForm()

pdfDoc.registerFontkit(fontkit)
const timesRoman = await pdfDoc.embedFont(StandardFonts.timesRoman)
const carolina =  await pdfDoc.embedFont(fontBytes)

// Get all fields in the PDF by their names
const nameField = form.getTextField('Name')
const idField = form.getTextField('CSEA_ID')
const emailField = form.getTextField('Email')
const phoneField = form.getTextField('Phone')
const titleField = form.getTextField('Title')
const signField = form.getTextField('sign')



// Fill in the basic info fields
nameField.setText('Nick', {font: timesRoman},)
idField.setText('12345678', {font: timesRoman},)
emailField.setText('nyynuge@gmail.com', {font: timesRoman},)
phoneField.setText('516-417-2758', {font: timesRoman},)
titleField.setText('GA Seas', {font: timesRoman},)
signField.setText('Nick S Nuge', {font: carolina},)



// Serialize the PDFDocument to bytes (a Uint8Array)
const pdfBytes = await pdfDoc.save()

// For example, `pdfBytes` can be:
//   • Written to a file in Node
//   • Downloaded from the browser
//   • Rendered in an <iframe>

  renderInIframe(pdfBytes)
  downloadPdf(pdfBytes)
}
test()
</script>
<iframe id="iframe"></iframe>
Then click this link to generate an email you can attach your letter to.
