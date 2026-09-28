# Wireshark Threaded Exception Conventions

Merged master MR !5829, authored by Guy Harris, makes the exception catcher stack thread-local. Merged !5845 clarifies which state needs thread locality. Merged !5835 catches registration exceptions in worker threads, copies the message before exception-frame teardown, returns owned data through join, and rethrows on the controlling initialization thread.

The durable convention is that catcher/jump state stays with its executing thread, while errors crossing a worker boundary are transferred as owned data rather than live exception control flow.
