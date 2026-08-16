[![DOI](https://img.shields.io/badge/DOI-10.82901%2Fnemar.on004853-blue)](https://doi.org/10.82901/nemar.on004853)

TX17 dataset

This is a placeholder dataset.

## NEMAR curation changes (2026-05-21, revised 2026-05-27)

The BIDS validator went from 1 error + 47 warnings to 0 errors + 37 warnings. None of the raw `.set`/`.fdt` signal payloads were modified; every change is to a text sidecar.

**Event dictionary (`task-nback_events.json`)**
- The one pre-curation validator error was a HED duplicate-tag error on `sub-001_task-nback_events.tsv`. This dataset (and the rest of the STRONG cohort, namely on004849/50/52/54/55, which carry the byte-identical defect) lists 197 onsets where two event rows share the same `onset` time. BIDS allows that, but the HED validator merges all HED annotations at a shared onset into one combined string, and the HED dictionary tagged about 80 of the roughly 107 distinct event codes with the same tag (`"Task"`), so most colliding pairs produced a `"Task,Task"` merge that fires the duplicate-tag error. To remove the error without inventing new annotations, the HED entries for codes `"1"` and `"307"` were dropped from the `value.HED` dictionary (105 of 107 entries preserved). Both codes appear in `events.tsv` only inside a collision with another code already tagged `"Task"`, so dropping their HED entries eliminates the `"Task,Task"` merges without affecting any standalone event. The `value.Levels` dictionary is unchanged; all 107 entries are still documented there.
- Added a top-level `sample` column definition, because the events table has a `sample` column that was previously undocumented in the sidecar.

**Channel table (`sub-001/eeg/sub-001_task-nback_channels.tsv`)**
- All 64 channels had `type=n/a` and `units=n/a`. The recording is a single EEG montage in microvolts, so both fields were filled in: `type=EEG` and `units=uV`. Channel names were left alone.

**Recording sidecar (`sub-001/eeg/sub-001_task-nback_eeg.json`)**
- Added `MISCChannelCount: 0` and `TriggerChannelCount: 0` (all 64 channels are scalp EEG, no auxiliary channels are present), and `EEGPlacementScheme: "10-10"` (the channel labels in `channels.tsv` are 10-10 names). Other keys were left as published.

**Dataset description (`dataset_description.json`)**
- Added `DatasetType: "raw"` so the dataset is validated as raw data rather than a derivative.
- Updated `BIDSVersion` from `1.8.0` to `1.11.1` (the version the current validator checks against).
- `ReferencesAndLinks` was published as `[""]` (a list containing an empty string, which is not a valid reference); changed to `[]` so the field is properly empty rather than holding a malformed entry.
- `GeneratedBy` was left absent, exactly as the source published it; nothing was added there.

**Remaining warnings (37), left on purpose**
- These are all "recommended but missing" fields that need information from the study, lab, or equipment that isn't in the dataset (for example: `Manufacturer`, `ManufacturersModelName`, `SoftwareVersions`, `DeviceSerialNumber`, `TaskDescription`, `Instructions`, `CogAtlasID`, `CogPOID`, `InstitutionName`, `InstitutionAddress`, `InstitutionalDepartmentName`, `CapManufacturer`, `CapManufacturersModelName`, `EEGGround`, `HeadCircumference`, `HardwareFilters`, `SubjectArtefactDescription`, `StimulusPresentation`, and `GeneratedBy` on `dataset_description.json`). They were left blank rather than filled with guesses. One remaining HED warning notes that value `1` in the events table no longer has a sidecar entry, which is the intended consequence of the duplicate-tag fix above.
