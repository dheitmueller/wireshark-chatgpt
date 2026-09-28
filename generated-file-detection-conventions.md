# Wireshark Generated-File Detection Conventions

## Generated-file detection and regeneration must agree with the repository's real source-of-truth path

Merged !6564 exposed a failure mode in both generated-source editing and checker recognition. Jörg Mayer caught that `packet-skinny.c` explicitly says it is generated, while Martin Mathieson found that the helper used by `check_typed_item_calls.py` looked for `Generated automatically` but the file actually said `Generated Automatically`. The attempted change could not be regenerated because the Skinny generator path itself failed under Python 3. Merged !6587 reverted the derivative edit; Guy Harris reproduced the generator failure on macOS/Python 3.8 and identified the generator/parser as needing Python 3 work.

**Review rule:** generated-file detection is part of the source-of-truth contract. It must recognize the canonical headers the repository actually emits; otherwise checkers may accidentally treat derivative output as handwritten source.

**Maintenance rule:** if the canonical generator cannot reproduce a desired generated change, fix the generator/template/input path first. Do not hand-edit the generated artifact and leave a broken regeneration path behind.

**Confidence:** Very high for the workflow rule. The problematic generated edit merged but was promptly reverted; Jörg Mayer, Martin Mathieson, and Guy Harris all identified the source/generator problem directly.
