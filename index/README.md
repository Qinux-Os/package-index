# Generated index

The resolved index, produced from [`../profiles/`](../profiles/) by
`qinux-cli index generate`.

## Why generate

Inheritance, conflict detection and the reverse package-to-profile map are all
resolved here, once, rather than in every consumer. The package manager, the
installer and the website read the result instead of reimplementing the rules.

## Files

| File | Contents |
| --- | --- |
| `profiles.json` | Every profile with inheritance flattened |
| `owners.json` | Package name to profiles that include it |
| `conflicts.json` | Profile and package conflicts, fully expanded |

## Regenerating

```sh
qinux-cli index generate
```

CI regenerates the index and fails if it differs from what is committed. The
committed index is therefore always correct, and a forgotten regeneration is
caught before merge.
