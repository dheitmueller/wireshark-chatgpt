# Corpus selection conventions

Build the candidate set from the actual `mr_<iid>.json` blobs in the pinned corpus Git tree, then subtract exact reviewed-MR membership reconstructed from all available tracking. Do not infer corpus gaps or reviewed coverage from range filenames or partial directory listings. !1956 is the regression example: an older ledger marked it absent at corpus commit `ddcaa22b51c68f594e425a23388c3a2086813054`, but the recursive tree at that same commit contains `mr_1956.json`.
