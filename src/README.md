# conv2pdf

[conv2pdf](https://conv2pdf.com/en/api/) converts Word, Excel, PowerPoint, images and HEIC photos to PDF, merges, compresses, numbers and secures PDFs, and converts PDFs to Word, on servers in France.

## Connect conv2pdf to Make

1. Sign in to your [conv2pdf account](https://conv2pdf.com/en/login/).
2. In the [Developer tab](https://conv2pdf.com/en/dashboard/#developer) of the dashboard, create an API key and copy it. The first key opens the free Dev plan. conv2pdf shows a key only once, when you create it.
3. In Make, add a conv2pdf module to a scenario, click **Create a connection** and paste the key in **API key**.

Make checks the key by reading its quota, which uses no conversion.

## Modules

Every module takes a file from a previous module, for example a file downloaded from Google Drive or received by email, and returns the result as a file for the next modules, with its size and the quota of the API key. Each module uses one conversion.

- **Convert files**: Convert a document to PDF, Convert an image to PDF, Convert a HEIC photo to PDF, Convert a HEIC photo to JPG, Convert a PDF to Word, Convert a PDF to images (a ZIP file of PNG or JPG images).
- **Edit PDFs**: Merge PDFs, Extract pages from a PDF, Rotate a PDF, Add page numbers to a PDF, Add a watermark to a PDF, Compress a PDF.
- **Secure PDFs**: Protect a PDF with a password, Remove a password from a PDF.
- **Get quota** returns the plan, the conversions used and the limits of the API key.
- **Make an API call** sends any request to the [conv2pdf API](https://conv2pdf.com/en/api/docs/) with your connection.

To merge PDFs that come from several bundles, collect them with an **Array aggregator** and map the array to **PDFs**.

## Limits and errors

- The Dev plan accepts files up to 10 MB and merges 2 PDFs at a time. Paid plans accept 200 MB and merge up to 20 PDFs. [Plans](https://conv2pdf.com/en/api/pricing/).
- A file that the module cannot process (wrong type, empty, protected, a scan without text for Word) raises a data error: with **Allow storing of incomplete executions** on, the bundle waits in the incomplete executions and the scenario goes on. Failed conversions do not count against the quota.
- An invalid or revoked API key deactivates the scenario: create a new key and update the connection.
- When the quota is used up, the run fails with a link to the plans.
- The API accepts 20 conversions per minute per key. Past that, Make pauses the scenario for a while and resumes it.
- A conversion can take up to a minute and a half, so the app waits up to 120 seconds for an answer.

conv2pdf deletes the uploaded and converted files one hour after the conversion.
