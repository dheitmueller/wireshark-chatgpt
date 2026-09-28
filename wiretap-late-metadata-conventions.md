# Wireshark Wiretap Late-Metadata Conventions

Merged master MR !5853 contains detailed Guy Harris review of pcapng files whose Interface Description Blocks are not front-loaded. Wiretap can begin with unknown encapsulation and discover link-layer information while records are read. The durable convention is that streaming consumers distinguish metadata known at open from metadata learned later; a fixed-link-type writer may need to delay output creation or reject a later incompatible type.
