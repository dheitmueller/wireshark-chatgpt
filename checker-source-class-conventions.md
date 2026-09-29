# Checker Source-Class Conventions

Merged MR !5512 exposed a clang-check false positive on the example/template file doc/packet-PROTOABBREV.c. Merged !5537 added a specific filename skip. Later reviewed MR !5663 generalized this area to exclude packet template C files as a class, with Jaap Keuter explicitly preferring the source-class rule.

Rule: define the semantic class of files a checker is intended to analyze. A one-off exception can unblock a false positive, but repeated exceptions are evidence that discovery needs a general template/generated/example classification rule.

Confidence: very high when the transitional !5537 fix is read together with the later merged !5663 guidance.
