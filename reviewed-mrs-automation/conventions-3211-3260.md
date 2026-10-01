# Conventions from MR review 3211-3260

Merged !3247, authored by Guy Harris, uses a block-data tvbuff as the parsing boundary for pcapng blocks and converts bounds failures into structural length diagnostics.

Merged !3255 makes TCP out-of-order reassembly conditional on sequence analysis because the reassembly state depends on sequence-number analysis.

Merged !3239 records that stream fragment insertion is first-pass state and is not repeated during redissection.
