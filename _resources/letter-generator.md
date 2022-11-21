---
title: "Grievance Letter Generator"
collection: resources
excerpt: "Use this form to generator a grievance letter to send to CSEA"
page_js:
 - https://unpkg.com/pdf-lib/dist/pdf-lib.min.js
 - https://unpkg.com/@pdf-lib/fontkit/dist/fontkit.umd.min.js
header:
  teaser: /assets/images/letter-preview.webp
page_checkbox : True
---


Fill in the forms below and download the pdf.
<form>
  <fieldset>
    Name: <input type="text" size="30" id="name"><br>
    Title: <input type="text" size="30" id="title"><br>
    CSEA ID (<a href="https://cseany.org/csea-member-id-lookup">Lookup Here </a>): <input type="test" size="30" id="csea_id"><br>
    Email: <input type="text" size="30" id="email"><br>
    Phone Number: <input type="text" size="30" id="phone"><br>
    <label for="newsletter" style="word-wrap:break-word">
      <input type="checkbox" class="larger-checkbox" id="newsletter" value=""/>   Subscribe to our Newsletter
    </label>
    <input type="button" value="Submit" class="btn--x-large" onclick="test()"/>

  </fieldset>
</form>

<script async>

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

const fonturl = "/assets/fonts/Carolina.ttf"
const fontBytes = await fetch(fonturl).then((res) => res.arrayBuffer())

// Load a PDF with form fields
const pdfDoc = await PDFDocument.load(
  await fetchBinaryAsset('pdfs/Letter to CSEA-Fillable.pdf'),
);
//Subscribe to newletter if checked
const email = document.getElementById("email").value;


const nlurl = "https://api.follow.it/subscription-form/ZjlOaGpqMno1MVFTTDU2WldkbmRTN0lpMkN1NlpHRDRnWlpQK01rV0swdjdqWHFVTjZEZVYvL2M4YUJhZHN3WHVUaHpkU2ppVGZYVGFUODZCTnBJb1cyQlovVHI5ZVo3RVhxcTUwbFYxekQvQ2ovaGptTzVrWmdYaHQvTyswZXJ8eTd4RTdoc1YvQUhPbXpHbnZXMGZmS1VNM3dXN0NaRzZMcTA3eFRLTmVHWT0=/8"
if (document.getElementById("newsletter").checked){
  document.getElementById("mce-EMAIL").value = document.getElementById("email").value;
  try {
    document.getElementById("mc-embedded-subscribe").click();
  } catch (e) {
    console.error(e);
  }

}


// Get the form containing all the fields
const form = pdfDoc.getForm()

pdfDoc.registerFontkit(fontkit)
const timesRoman = await pdfDoc.embedFont(StandardFonts.TimesRoman)
const carolina =  await pdfDoc.embedFont(fontBytes)

// Get all fields in the PDF by their names
const nameField = form.getTextField('Name')
const idField = form.getTextField('CSEA_ID')
const emailField = form.getTextField('Email')
const phoneField = form.getTextField('Phone')
const titleField = form.getTextField('Title')
const signField = form.getTextField('sign')



// Fill in the basic info fields
nameField.setText(document.getElementById('name').value)
idField.setText(document.getElementById('csea_id').value)
emailField.setText(document.getElementById('email').value)
phoneField.setText(document.getElementById('phone').value)
titleField.setText(document.getElementById('title').value)
signField.setText(document.getElementById('name').value)

form.updateFieldAppearances(timesRoman);

signField.updateAppearances(carolina);

form.flatten();
// Serialize the PDFDocument to bytes (a Uint8Array)
const pdfBytes = await pdfDoc.save()

// For example, `pdfBytes` can be:
//   • Written to a file in Node
//   • Downloaded from the browser
//   • Rendered in an <iframe>

  //renderInIframe(pdfBytes)
  downloadPdf(pdfBytes)
}

</script>

<a href="mailto:nick@ncseasonal.com?subject=Grievance&body=Please%20find%20my%20grievance%20letter%20attached%20">Click here to open an email to send to the CSEA executives.
Don't Forget to attach your letter!</a>
