# Wireshark Umbrella-Header Conventions

## Use the controlled Wireshark umbrella header without weakening header self-containment

Wireshark deliberately uses `<wireshark.h>` as a small common prerequisite rather than requiring every source/header to spell out the same GLib and project-export prerequisites independently. That convention does not replace the requirement that a header compile stand-alone: dependencies not supplied by the umbrella still belong in the header that uses them.

Merged MR !10861 made the wsutil headers self-contained and moved each implementation's own header to the front of the implementation include list. During review, João Valverde explicitly directed the change away from adding `<glib.h>` everywhere and toward `<wireshark.h>`, explaining that the umbrella is intentionally kept small and that this property must be preserved. He also gave the intended split: when a component has a public header, that header includes `<wireshark.h>`; the C file keeps `"config.h"` first (with `WS_LOG_DOMAIN` established before headers that consume it where applicable) and then includes its own header before private dependencies. If there is no corresponding header, the C file includes `<wireshark.h>` itself.

**Implementation rule:** use the project's controlled common header for prerequisites it intentionally owns instead of redundantly including those transitive headers in every file. At the same time, keep `wireshark.h` intentionally small and continue to add direct includes for dependencies outside that common contract.

**Validation rule:** the own-header-first technique remains the test of self-containment. Do not let the umbrella become an excuse for accidental include-order dependencies, and do not move `config.h` into public headers.

**Confidence:** Very high. The broad wsutil cleanup merged after detailed João Valverde and Martin Mathieson review, with the include layout and umbrella-header policy stated explicitly.
