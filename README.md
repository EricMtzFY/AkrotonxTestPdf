## Live Demo

You can see a live demo at [https://elegant-raindrop-716ab0.netlify.app/](https://elegant-raindrop-716ab0.netlify.app/).

# PDF Viewer with PDF.js

This project is an example of how to integrate a PDF viewer into a web page using the PDF.js library. The viewer allows loading and rendering a PDF document from an external URL and displaying it in an HTML container.

## Requirements

- Internet connection to load the PDF.js library from a CDN.
- A modern web browser.

## Project Structure

The project consists of a single HTML file that includes the necessary code to load and render the PDF.

### index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      /* Styles for the PDF container and canvas */
      .pdf-container {
        width: 100%;
        height: 100vh;
        overflow: auto;
        max-width: 770px;
        max-height: 1920px;
        display: flex;
        flex-direction: column;
        align-items: center;
        margin: auto;
      }
      .pdf-page {
        width: 100%;
        margin-bottom: 20px;
      }
      h1 {
        display: flex;
        justify-content: center;
        text-align: center;
      }
      .content {
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column; /* Change to column for small screens */
        gap: 10px;
        max-width: 90vw;
        margin: 10;
      }

      @media (min-width: 600px) {
        .content {
          flex-direction: row; /* Change to row for larger screens */
        }
      }
      .content a {
        word-break: break-all; /* Allow the link to break into multiple lines */
      }
    </style>
  </head>
  <body>
    <h1>This is the PDF</h1>
    <div class="content">
      <p>it's hosted in</p>
      <a href="https://akrotonx.pixxio.media/plk/Utilisation_papier.book.pdf"
        >https://akrotonx.pixxio.media/plk/Utilisation_papier.book.pdf</a
      >
    </div>

    <!-- Container for the PDF viewer -->
    <div class="pdf-container" id="pdf-container">
      <!-- PDF pages will be dynamically added here -->
    </div>
    <!-- Load the PDF.js library -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.10.377/pdf.min.js"></script>
    <script>
      // URL of the PDF to load
      const url =
        "https://akrotonx.pixxio.media/plk/Utilisation_papier.book.pdf";

      // Initialize the PDF.js library
      const pdfjsLib = window["pdfjs-dist/build/pdf"];
      pdfjsLib.GlobalWorkerOptions.workerSrc =
        "https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.10.377/pdf.worker.min.js";

      // Load the PDF document
      const loadingTask = pdfjsLib.getDocument(url);
      loadingTask.promise.then((pdf) => {
        // Get the total number of pages
        const numPages = pdf.numPages;

        // Function to render a page
        function renderPage(pageNumber) {
          pdf.getPage(pageNumber).then((page) => {
            const scale = 1.5; // Scale for rendering
            const viewport = page.getViewport({ scale });

            // Create a canvas for each page
            const canvas = document.createElement("canvas");
            canvas.className = "pdf-page";
            const context = canvas.getContext("2d");
            canvas.height = viewport.height;
            canvas.width = viewport.width;

            // Render the page on the canvas
            const renderContext = {
              canvasContext: context,
              viewport: viewport,
            };
            page.render(renderContext);

            // Add the canvas to the container
            document.getElementById("pdf-container").appendChild(canvas);
          });
        }

        // Render all pages
        for (let pageNumber = 1; pageNumber <= numPages; pageNumber++) {
          renderPage(pageNumber);
        }
      });
    </script>
  </body>
</html>
```

## How to Use This Example

Download or Clone the Project:

You can download the HTML file or clone the repository if it is available on a platform like GitHub.

Open the HTML File:

Open the index.html file in your web browser.

View the PDF:

The PDF will automatically load from the specified URL and display in the container.

## How to Implement in Other Projects

To implement a similar PDF viewer in your own project, follow these steps:

### Include PDF.js:

Make sure to include the PDF.js library in your HTML file. You can use a CDN as shown in the example:

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.10.377/pdf.min.js"></script>
```

### Set Up the Container:

Create a container in your HTML where the PDF pages will be rendered:

```html
<div class="pdf-container" id="pdf-container"></div>
```

### Load and Render the PDF:

Use the provided JavaScript code to load and render the PDF. Make sure to update the URL of the PDF you want to load:

```javascript
const url = "YOUR_PDF_URL";
```

### CSS Styles:

Adjust the CSS styles as needed to ensure the viewer is responsive and looks good on different devices.

## Notes

This example uses an external URL to load the PDF. Make sure the URL is accessible and the PDF is publicly available.
You can adjust the rendering scale (scale) to change the size of the PDF pages as needed.

We hope this example helps you integrate a PDF viewer into your own projects!
