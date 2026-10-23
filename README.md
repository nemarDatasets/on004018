[![DOI](https://img.shields.io/badge/DOI-10.82901%2Fnemar.on004018-blue)](https://doi.org/10.82901/nemar.on004018)

Grootswagers T.*, Robinson A.K.*, Carlson T.A. (2019). The representational dynamics of visual objects in rapid serial visual processing streams. NeuroImage, 188, 668-679 https://doi.org/10.1016/j.neuroimage.2018.12.046

See also https://osf.io/a7knv/

## NEMAR curation changes (2026-05-21, revised 2026-05-27)

The BIDS validator went from 0 errors + 2148 warnings to 0 errors + 1858 warnings. None of the raw `.eeg`/`.vhdr`/`.vmrk` files were modified — every change is to a text sidecar.

**Dataset description (`dataset_description.json`)**
- Added `DatasetType: "raw"` so the validator applies raw-dataset rules instead of derivative-dataset rules. This dataset is raw (top-level subjects, no `derivatives/` directory).
- Updated `BIDSVersion` from `"1.0.0 + bep006"` to `1.11.1` (the version the current validator checks against). The old string did not parse as a known BIDS schema version and fired the unknown-BIDS-version warning; this dataset already conforms to the modern EEG schema.
- Removed a duplicate-typo `ReferenceAndLinks` key (singular `Reference`). It shadowed the correctly-spelled BIDS-canonical `ReferencesAndLinks` already present alongside it, and the canonical key carries the same two URLs.
- `GeneratedBy` was left absent, exactly as the source published it — nothing was added there.

**Participants sidecar (`participants.json`, new)**
- Created a sidecar describing the three non-`participant_id` columns already in `participants.tsv`: `testing_date` (free-text DD-MM-YYYY date), `sex` (with a `Levels` mapping `M`/`F` to "male"/"female", matching the values in the TSV), and `age` (with `Units: "years"`). This documents the columns per BIDS and closes the undefined-columns warning, without altering any underlying TSV cell.

**Root recording sidecar (`task-rsvp_eeg.json`)**
- Added `MISCChannelCount: 0` and `TriggerChannelCount: 0`. The 63-channel BrainVision recordings are exclusively EEG — the channel-name list captured during load is 63 standard 10-10 scalp electrodes with no `MISC*` or `STIM*`/`TRIG*` channels — so both counts are zero by inspection. Adding them at the root sidecar applies to every recording without duplicating the value per file.
- Added `EEGPlacementScheme: "10-10"`. The channel set includes intermediate 10-10 electrodes (FC1/FC5/CP1/CP5/AF4/AF8/FCz, plus T7/T8/P7/P8 rather than the 10-20-only T3-T6 naming), which uniquely identifies the 10-10 system.

**Remaining warnings (1858) — left on purpose**
- These are all "recommended but missing" fields that need information from the study, lab, or equipment that isn't in the dataset (for example: `Manufacturer`, `ManufacturersModelName`, `SoftwareVersions`, `DeviceSerialNumber`, `CapManufacturer`, `CapManufacturersModelName`, `EEGGround`, `HardwareFilters`, `HeadCircumference`, `SubjectArtefactDescription`, `TaskDescription`, `Instructions`, `CogAtlasID`, `CogPOID`, `InstitutionName`/`Address`/`DepartmentName`, `RecordingDuration`, `RecordingType`, `StimulusPresentation`, `HEDVersion`, and `GeneratedBy`). They were left blank rather than filled with guesses.
