# AI Assistance

This project was developed with AI assistance across the full
workflow — problem framing, statistical method selection, code
review, and verification.

**Claude** was used for:
- Reviewing statistical approach and catching methodological issues
  (e.g. flagging that comparing regions by district name alone would
  miscount same-named districts across different cities — see
  `validation_log.md`)
- Debugging (e.g. the `plt.rc()` import-order error, the pandas
  multi-file date-parsing issue in `01_data_cleaning.ipynb`)
- Fact-checking claims before they went into this README or the
  notebooks — including verifying administrative boundary change
  dates, and rejecting a specific academic citation that could not
  be independently confirmed to exist

**ChatGPT** was used for a second-opinion review pass on the analysis
and README drafts, which surfaced additional issues later verified
and fixed here (e.g. the leave-one-out baseline for excess-risk
calculations, and the administrative-renaming bug described in
`validation_log.md`).

All statistical code, analysis decisions, and final write-ups were
reviewed and executed by the author; AI tools were used for
verification and critique, not as a substitute for understanding
the underlying methods.
