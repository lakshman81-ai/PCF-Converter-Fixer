# Revised Smart Fixer Logic & Two-Pass Architecture

## Overview
The Smart Fixer Engine has been upgraded from a strict single-pass sequential walker to a **Two-Pass Point-to-Element (PTE) Architecture**. This ensures that disjointed PCF files (with shuffled component sequences, mixed line designations, and unlinked orphan elements) can be correctly re-assembled and fixed.

The core challenge in raw PCF extraction is that Isogen relies on perfect geometric end-to-end connectivity. The new system acts as an intermediary pipeline:
1. **Pass 1 (Sequence & Spatial PTE):** Reconstruct the primary sequence graph, attach logical orphans using Point-to-Element (PTE) mapping, and propose Tier 1/2 auto-fixes.
2. **Approval Gate:** The user reviews the proposed fixes via the UI Data Table. They can explicitly toggle an `Approve` or `Reject` state on every fix. Applying the fixes merges them into the `dataTable` state.
3. **Pass 2 (Refinement):** Once Pass 1 is baked into the spatial array, a secondary walker pass runs. This pass specifically focuses on closing gaps (R-GAP) and evaluating overlaps (R-OVR) that were only exposed *after* the primary alignment shifts from Pass 1 occurred.
4. **Final Approval & Export:** The user approves the Pass 2 fixes. Once applied, the Excel and PCF exports are unlocked.

## The Approval Workflow & Data Structure
The application uses an explicit state-machine gating mechanism:

```
Import -> Basic Validation -> [Smart Fix 1] -> [Approve/Reject] -> [Apply] -> [Smart Fix 2] -> [Approve/Reject] -> [Apply] -> Export
```

* **`fixingActionRejected` boolean:** Every element in the `dataTable` now holds a `fixingActionRejected` state.
* During the `applyFixesEngine()` execution, any component where `fixingActionRejected === true` will skip its `_proposedFix` logic.
* The UI offers a quick **"Auto Approve All"** button, resolving the rejection flag across the entire dataset.

## The Point-to-Element (PTE) Engine

When `AutoMultiPassMode` is enabled, the `runPteEngine()` executes before the standard `runSmartFix()` graph walker.

### 1. Line_Key Grouping (Optional but Recommended)
If the user maps a column (e.g., `PIPELINE-REFERENCE`) to `Line_Key`, the system chunks the processing array. PTE logic runs intensely within the same Line_Key boundaries before attempting cross-boundary sweeps.

### 2. Orphan Sweeping Algorithm
Instead of strict `EP2 -> EP1` end-to-end matching, PTE calculates a spatial boundary around every orphan component.
* **Bore Ratio Constraints:** `boreRatioMin` (default 0.7) and `boreRatioMax` (default 1.5). An orphan's bore must fall within this multiple of the target element's bore to even be considered a candidate.
* **Proximity Sweep:** `orphanSweepMinMult` (default 0.2) and `orphanSweepMaxRadius` (default 13000mm). The engine draws a dynamic sphere around the orphan to search for potential connection points (`CP`, `EP1`, `EP2`, or even `BP` of Tees/Olets).
* If a valid PTE link is established, the orphan's coordinates are flagged for `SNAP_AXIS` or `SNAP` to integrate it into the primary chain, and it is marked for processing in Pass 2.

## Configurable Settings in UI
These settings have been exposed in the **SMART FIXER SETTINGS** tab:
- **Chaining Strategy:** Strict Sequential vs Spatial Topology.
- **Line_Key Mapping:** Links the PTE engine to a specific header column.
- **AutoMultiPassMode:** Toggles the Two-Pass architecture.
- **Bore Ratio Min/Max:** Thresholds for valid element-to-element pairing during PTE.
- **Orphan Sweep Mult/Max:** The dynamic search radius constants for finding unlinked elements.

## Rule Book Alignment (PCF-MASTER-002)
- **R-GEO-01 & R-GEO-03:** Pass 1 focuses heavily on deleting micro-pipes and snapping off-axis elements.
- **R-GAP-02 & R-OVR:** Pass 2 is dedicated to analyzing the newly snapped graph, inserting gap-fill pipes, and trimming overlapping pipes safely.