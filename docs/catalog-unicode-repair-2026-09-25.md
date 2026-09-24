# AssetBundle catalog Unicode correction (2026-09-25)

The legacy `category_scan.py` read a Windows Python child process as UTF-8 with
replacement decoding. The child used CP932. For
`live2d/model/05minori_sports`, the fullwidth `ｍ` (U+FF4D) was emitted as
CP932 bytes `82 8d` and stored as two replacement characters (`��`). A fresh
AssetBundle index fetch for `6.8.0.50` contains `05minori_ｍ_sports.2048`.

Both existing snapshots contain the same corrupted path. This correction is a
documented exception to the usual immutable-snapshot policy. It changes that
one path in `6.8.0.40`, `6.8.0.50`, and `catalog/current`, while preserving every
other catalog byte. Git history retains the originally committed files.

| Asset version | Previous catalog SHA-256 | Corrected catalog SHA-256 |
| --- | --- | --- |
| 6.8.0.40 | `e3acdac6e10ad77a2b287fa830fdcc2a63fa5ba022b5a7009d10dae61eceaa3a` | `d75de3008d19b3f52cb58e824b6d9d3b90273ca159771375d1fe8b2d90528c30` |
| 6.8.0.50 | `877209f7a6b414e8a1716401204d33d461d99013a8d987d4511e69565be5c9ea` | `d4985fcd27d030eadfb0d0548beb12dfc6c204eb4b7f0539a66c7ecb0bbd736c` |

Snapshot and current metadata, `versions.json`, and the diff's catalog checksum
fields have been updated to the corrected values. The diff entries and counts
remain the same because the affected bundle was identical in both versions.
