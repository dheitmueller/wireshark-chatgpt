# Wireshark endpoint-table API conventions

Merged !7869 and !7874, authored by Guy Harris, establish that endpoint tables describe endpoints, not necessarily hosts. Endpoint identity is distinct from a conversation or circuit identifier. Public API renames retain deprecated compatibility wrappers and exported-symbol metadata.
