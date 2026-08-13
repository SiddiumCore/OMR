# Contact Information Extraction Implementation Plan

## Outcome

Every CRATOR result contains `Parent_Name`, `Mobile_No`, `Email`, and
`Whatsapp_Username` as the final columns. Gemini extraction is optional and is
selected by default in a fresh UI session. Disabled or failed extraction leaves
those four values blank without changing the OMR result.

## Data flow

1. The local agent checks `GEMINI_API_KEY`, root `gemini_config.json`, and then
   Windows Credential Manager for a Gemini API key.
2. The standard UI does not display or edit API-key settings. The agent injects
   the configured key only into the runner process environment for an enabled
   run. Credential Manager endpoints remain available for compatibility.
3. OMR marker alignment/fallback runs normally.
4. The selected template normalizes the sheet to 1240 x 1760 and crops only its
   configured contact-information band.
5. A separate bounded Gemini thread pool sends one cropped JPEG per sheet to
   `gemini-3.1-flash-lite` using structured JSON output.
6. Results remain keyed to the source sheet's processing record regardless of
   completion order.
7. CSV writing appends the four contact values. Access export inherits the same
   columns from the merged CSV.

## Failure and privacy rules

- Missing/unreadable fields are blank; Gemini is told not to infer or correct.
- API errors, timeouts, malformed responses, and missing crops never fail OMR.
- The full OMR sheet is not sent to Gemini.
- API keys are never placed in React storage, command-line arguments, URLs, or
  logs. A filled `gemini_config.json` is plaintext and must not be shared.
- Contact extraction has independent concurrency and progress reporting.
- Stop terminates the runner, which prevents queued work from continuing.

## Verification

- Unit-test crop boundaries, normalization, response parsing, retry behavior,
  blank fallback, bounded concurrency, and fixed output width.
- Build the React UI and compile Python sources.
- Run OMR with extraction disabled and with a mocked Gemini response.
- Before a release build, benchmark a representative real batch and visually
  review the configured crop on marker and no-marker examples.
