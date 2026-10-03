# Memory/lifetime addendum from MRs !860-!909

## Bytewise struct keys

Merged !897, with !901/!902 backports, zero-initializes RTPS map keys before assigning members because hashing/equality used the complete struct representation. If an object is hashed or compared as raw bytes, initialize every byte, including padding; otherwise prefer member-wise identity.

## Scope must match lifecycle

Merged !866/!862 moves GIOP initialization-time storage out of packet scope. Follow-up !868/!863/!861 releases each generated buffer in the loop iteration that owns it. Treat allocator scope as an ownership contract and free explicitly owned loop temporaries at the narrowest correct lifetime.
