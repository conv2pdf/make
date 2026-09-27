# conv2pdf for Make

The [Make](https://www.make.com) app of the [conv2pdf API](https://conv2pdf.com/en/api/): it converts Word, Excel, PowerPoint, images and HEIC photos to PDF, merges, compresses, numbers and secures PDFs, and converts PDFs to Word, on servers in France.

## Modules

| Group | Module | conv2pdf tool |
| --- | --- | --- |
| Convert files | Convert a document to PDF | `office-to-pdf` |
| | Convert an image to PDF | `image-to-pdf` |
| | Convert a HEIC photo to PDF | `heic-to-pdf` |
| | Convert a HEIC photo to JPG | `heic-to-jpg` |
| | Convert a PDF to Word | `pdf-to-word` |
| | Convert a PDF to images | `pdf-to-image` |
| Edit PDFs | Merge PDFs | `merge-pdf` |
| | Extract pages from a PDF | `split-pdf` |
| | Rotate a PDF | `rotate-pdf` |
| | Add page numbers to a PDF | `page-numbers-pdf` |
| | Add a watermark to a PDF | `watermark-pdf` |
| | Compress a PDF | `compress-pdf` |
| Secure PDFs | Protect a PDF with a password | `protect-pdf` |
| | Remove a password from a PDF | `unlock-pdf` |
| Account | Get quota | `GET /v1/quota` |
| Other | Make an API call | any path under `https://api.conv2pdf.com` |

Each file module sends the mapped file to `POST /v1/convert/{tool}` as multipart/form-data, then downloads the result from `GET /v1/download/{job_id}` in the same module and returns it as a Make file, with its size and the quota of the API key. conv2pdf deletes the files one hour after the conversion.

The connection asks for a conv2pdf API key, created in the Developer tab of the [conv2pdf dashboard](https://conv2pdf.com/en/dashboard/#developer), and checks it against `GET /v1/quota`, which uses no conversion. The key is sent only to `api.conv2pdf.com`.

## How it behaves

- The error codes of conv2pdf decide the Make error type. A problem with the file or the options of the run (`unsupported_content`, `file_too_large`, `pdf_scanned_needs_ocr`, `conversion_failed`...) raises a `DataError`, which Make can keep as an incomplete execution. A revoked key raises `InvalidAccessTokenError`, which deactivates the scenario.
- conv2pdf answers `429` both for a used-up quota and for the rate limit: `quota_exceeded` becomes a `RuntimeError`, since retrying cannot help, and `rate_limited` stays a `RateLimitError`, which Make retries after a pause. `5xx` answers such as `503 server_busy` are `ConnectionError`s, which Make retries.
- The messages for each conv2pdf error code are in the common data (`src/general/common.json`), because Make caps an error message expression at 1024 characters.
- A conversion can take up to 90 seconds on conv2pdf's side: the app raises Make's 40-second request timeout to 120 seconds, like the conv2pdf node for n8n.

## Development

The source follows Make's local development layout: `src/makecomapp.json` lists the components and their files, and the Make Apps Editor for VS Code can open it. `scripts/make-app.mjs` checks the files and deploys them through the [SDK Apps API](https://developers.make.com/api-documentation/api-reference/sdk-apps), with no dependencies:

```bash
node scripts/make-app.mjs check
node scripts/make-app.mjs deploy --icon
```

Deploying needs a Make API token with the `sdk-apps:read` and `sdk-apps:write` scopes, in `MAKE_API_TOKEN` or in `.secrets/apikey` (ignored by git). `branding/make-logo-512.png` is the logo: Make paints its transparent areas in the theme colour `#E44A30` and its black areas in white.

## License

MIT
