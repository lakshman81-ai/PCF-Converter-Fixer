# PCF Validator & Smart Fixer: Benchmark Report

| Test ID | Group | Description | Expected Rule/Action | Actual Detected | Zero Errors on Re-Import? |
|---------|-------|-------------|----------------------|-----------------|---------------------------|
| BM-V-01 | validation | V1: Detect (0,0,0) in EP1 — must flag ERROR | V1 | [V1] ERROR, [V2] ERROR | N |
| BM-V-02 | validation | V1: Single zero axis (0,5000,3000) is valid — no error | V1 | [V2] ERROR | N |
| BM-V-03 | validation | V1: SUPPORT at (0,0,0) — must flag ERROR | V1 | [V1] ERROR, [V18] WARNING | N |
| BM-V-04 | validation | V2: Bore as integer (400) vs coords 4-decimal — ERROR at generation | V2 | [V2] ERROR | N |
| BM-V-05 | validation | V2: All coords and bore at 4-decimal — PASS | V2 | [V2] ERROR | N |
| BM-V-06 | validation | V3: REDUCER with EP1 bore=400, EP2 bore=400 — ERROR (not a reducer) | V3 | [V2] ERROR, [V3] ERROR | N |
| BM-V-07 | validation | V3: REDUCER EP1=400, EP2=350 — PASS | V3 | [V2] ERROR | N |
| BM-V-08 | validation | V3: PIPE bore changes 400→350 without being reducer — ERROR | V3 | [V2] ERROR, [V3] ERROR | N |
| BM-V-09 | validation | V4: BEND CP equals EP1 — degenerate bend ERROR | V4 | [V2] ERROR, [V4] ERROR, [V6] ERROR, [V7] WARNING | N |
| BM-V-10 | validation | V5: BEND CP equals EP2 — degenerate bend ERROR | V5 | [V2] ERROR, [V5] ERROR, [V6] ERROR, [V7] WARNING | N |
| BM-V-11 | validation | V6: BEND CP on EP1-EP2 line (collinear) — ERROR | V6 | [V2] ERROR, [V6] ERROR | N |
| BM-V-12 | validation | V6: BEND CP properly off EP1-EP2 line — PASS | V6 | [V2] ERROR | N |
| BM-V-13 | validation | V7: BEND dist(CP,EP1)=500, dist(CP,EP2)=505 — WARNING | V7 | [V2] ERROR, [V7] WARNING | N |
| BM-V-14 | validation | V7: BEND CP equidistant from EP1 and EP2 — PASS | V7 | [V2] ERROR | N |
| BM-V-15 | validation | V8: TEE CP off midpoint by 5mm — ERROR | V8 | [V2] ERROR, [V8] ERROR, [V9] ERROR, [V10] WARNING | N |
| BM-V-16 | validation | V8: TEE CP at exact midpoint — PASS | V8 | [V2] ERROR, [V9] ERROR | N |
| BM-V-17 | validation | V9: TEE CP bore=350 but EP bore=400 — ERROR | V9 | [V2] ERROR, [V9] ERROR | N |
| BM-V-18 | validation | V10: TEE BP 30° from perpendicular — WARNING | V10 | [V2] ERROR, [V9] ERROR, [V10] WARNING | N |
| BM-V-19 | validation | V10: TEE BP exactly perpendicular — PASS | V10 | [V2] ERROR, [V9] ERROR | N |
| BM-V-20 | validation | V11: OLET with EP1 populated — ERROR | V11 | [V2] ERROR, [V11] ERROR | N |
| BM-V-21 | validation | V11: OLET with only CP and BP, no EPs — PASS | V11 | [V2] ERROR | N |
| BM-V-22 | validation | V12: SUPPORT with CA1 populated — ERROR | V12 | [V12] ERROR, [V18] WARNING | N |
| BM-V-23 | validation | V12: SUPPORT with no CAs — PASS | V12 | [V18] WARNING | Y |
| BM-V-24 | validation | V13: SUPPORT bore=350 (must be 0) — ERROR | V13 | [V13] ERROR | N |
| BM-V-25 | validation | V13: SUPPORT bore=0 — PASS | V13 | [V18] WARNING | Y |
| BM-V-26 | validation | V14: FLANGE with empty SKEY — WARNING | V14 | [V2] ERROR, [V14] WARNING, [V16] INFO | N |
| BM-V-27 | validation | V14: PIPE with empty SKEY — PASS (not required for PIPE) | V14 | [V2] ERROR | N |
| BM-V-28 | validation | V15: EP2[0] and EP1[1] differ by 5mm — WARNING | V15 | [V2] ERROR, [V15] WARNING | N |
| BM-V-29 | validation | V15: EP2[0] exactly equals EP1[1] — PASS | V15 | [V2] ERROR | N |
| BM-V-30 | validation | V16: PIPE with CA8 (weight) populated — WARNING | V16 | [V2] ERROR, [V16] WARNING | N |
| BM-V-31 | validation | V16: FLANGE without CA8 — INFO (not error, just note) | V16 | [V2] ERROR, [V16] INFO | N |
| BM-V-32 | validation | V17: PCF output with \n instead of \r\n — ERROR | ERROR | CRASH: No input data | N/A |
| BM-V-33 | validation | V18: Bore=16 (not in std mm set, ≤48) — WARNING (likely inches) | V18 | [V2] ERROR, [V18] WARNING | N |
| BM-V-34 | validation | V18: Bore=400 (in std mm set) — PASS | V18 | [V2] ERROR | N |
| BM-V-35 | validation | V19: SUPPORT MESSAGE-SQUARE contains LENGTH token — WARNING | V19 | [V19] WARNING, [V18] WARNING | Y |
| BM-V-36 | validation | V19: SUPPORT MESSAGE-SQUARE with only RefNo/SeqNo/Name/GUID — PASS | V19 | [V18] WARNING | Y |
| BM-V-37 | validation | V20: SUPPORT_GUID missing UCI: prefix — ERROR | V20 | [V18] WARNING, [V20] ERROR | N |
| BM-V-38 | validation | V20: SUPPORT_GUID with UCI: prefix — PASS | V20 | [V18] WARNING | Y |
| BM-V-39 | validation | V3: PIPE EP1 bore=350, EP2 bore=350 — PASS (non-reducer, same bore OK) | V3 | [V2] ERROR | N |
| BM-V-40 | validation | V14: TEE with SKEY=TEBW — PASS | V14 | [V2] ERROR, [V9] ERROR | N |
| BM-SF-01 | smartfix | R-GEO-01: 3mm micro-pipe — DELETE (Tier 1) | R-GEO-01 | [R-GEO-01] | N |
| BM-SF-02 | smartfix | R-GEO-01: 10mm pipe — no action (above threshold) | R-GEO-01 | None | N |
| BM-SF-03 | smartfix | R-GEO-02: Bore 400→350 without reducer — ERROR | R-GEO-02 | [R-GEO-02] | N |
| BM-SF-04 | smartfix | R-GEO-03: Pipe with 1mm X-drift on Y-axis run — SNAP (Tier 2) | R-GEO-03 | [R-GEO-03], [R-TOP-02] | N |
| BM-SF-05 | smartfix | R-GEO-03: Pipe diagonal 50mm on 2 axes — ERROR | R-GEO-03 | [R-GEO-03], [R-TOP-02] | N |
| BM-SF-06 | smartfix | R-GEO-07: Flange with EP1=EP2 (zero length) — ERROR | R-GEO-07 | [R-GEO-07], [R-AGG-01], [R-TOP-02] | N |
| BM-SF-07 | smartfix | R-CHN-01: Pipe changes Y→Z axis without bend — ERROR | R-CHN-01 | [R-CHN-01] | N |
| BM-SF-08 | smartfix | R-CHN-02: 5mm fold-back pipe (reverses Y direction) — DELETE (Tier 2) | R-CHN-02 | [R-GEO-01], [R-CHN-02] | N |
| BM-SF-09 | smartfix | R-CHN-02: 50mm fold-back pipe — ERROR (too large to auto-delete) | R-CHN-02 | [R-CHN-02] | N |
| BM-SF-10 | smartfix | R-CHN-03: Two bends with no pipe between — WARNING | R-CHN-03 | [R-AGG-01] | N |
| BM-SF-14 | smartfix | R-GAP-01: 0.3mm micro-gap between elements — SNAP (Tier 1) | R-GAP-01 | [R-GAP-01] SNAP | N |
| BM-SF-15 | smartfix | R-GAP-02: 15mm axial gap along Y-South — INSERT pipe (Tier 2) | R-GAP-02 | [R-GAP-02] INSERT | N |
| BM-SF-22 | smartfix | R-OVR-01: 10mm pipe-pipe overlap — TRIM (Tier 2) | R-OVR-01 | [R-OVR-01] TRIM | N |
| BM-SF-25 | smartfix | R-OVR-03: Flange-flange overlap — ERROR (rigid-on-rigid) | R-OVR-03 | [R-OVR-03] ERROR | N |
| BM-SF-27 | smartfix | R-BRN-01: TEE branch bore (500) exceeds header bore (400) — ERROR | R-BRN-01 | [R-BRN-01], [R-AGG-01], [R-TOP-02] | N |
| BM-SF-32 | smartfix | R-TOP-02: Disconnected element (>25mm from any chain) — ERROR | R-TOP-02 | [R-TOP-02] | N |
| BM-SF-40 | smartfix | R-GAP-08: Verify inserted gap-fill element is PIPE, not a fitting | INSERT | [R-GAP-02] INSERT | N |