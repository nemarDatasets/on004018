[![DOI](https://img.shields.io/badge/DOI-10.82901%2Fnemar.on004018-blue)](https://doi.org/10.82901/nemar.on004018)

Grootswagers T.*, Robinson A.K.*, Carlson T.A. (2019). The representational dynamics of visual objects in rapid serial visual processing streams. NeuroImage, 188, 668-679 https://doi.org/10.1016/j.neuroimage.2018.12.046

See also https://osf.io/a7knv/

## NEMAR curation changes (2026-05-21)

BIDS validator: 0 errors + 2148 warnings -> 0 errors + 1857 warnings. Raw `.eeg`/`.vhdr`/`.vmrk` binary payloads unchanged.

### `dataset_description.json`
- Bumped `BIDSVersion` from `"1.0.0 + bep006"` to `"1.8.0"`. Why: the old value did not parse as a known BIDS schema version and fired the `UNKNOWN_BIDS_VERSION` warning; 1.8.0 is the modern EEG-supporting schema this dataset already conforms to.
- Added `"DatasetType": "raw"`. Why: without it, the validator can switch to derivative-dataset rules and surface spurious description warnings; this dataset is raw (top-level subjects, not under `derivatives/`).
- Added `GeneratedBy` array naming `nemar-cli`. Why: closes the `JSON_KEY_RECOMMENDED:GeneratedBy` warning and documents the rehost tooling.
- Removed the duplicate-typo `ReferenceAndLinks` key (singular `Reference`). Why: it shadowed the correctly-spelled BIDS-canonical `ReferencesAndLinks` already present alongside it; the canonical key carries the same two URLs.

### `participants.json` (new)
- Created a participants sidecar declaring the three non-`participant_id` columns: `testing_date` (free-text DD-MM-YYYY date), `sex` (with `Levels` mapping `M`/`F` to "male"/"female", matching the values already in the TSV), and `age` (with `Units: "years"`). Why: closes the `TSV_ADDITIONAL_COLUMNS_UNDEFINED:testing_date` warning and documents the columns per BIDS without altering the underlying TSV cells.

### `task-rsvp_eeg.json` (root inheriting sidecar)
- Added `"MISCChannelCount": 0` and `"TriggerChannelCount": 0`. Why: the dataset's 63-channel BrainVision recordings are exclusively EEG (per the channel-name list captured in `raw_meta.json`: 63 names, all standard 10-10 scalp electrodes, no `MISC*` or `STIM*`/`TRIG*` channels). Closes 192 `SIDECAR_KEY_RECOMMENDED` warnings (2 keys x 96 = 32 recordings x 3 file types per recording).
- Added `"EEGPlacementScheme": "10-10"`. Why: the channel-name set in `raw_meta.json` contains intermediate 10-10 electrodes (FC1/FC5/CP1/CP5/AF4/AF8/FCz, plus T7/T8/P7/P8 rather than the 10-20-only T3-T6 naming) which uniquely identifies the 10-10 system. Closes 96 `EEGPlacementScheme` warnings.

### Out of mechanical scope (left as warnings)
Remaining 1857 warnings are all `SIDECAR_KEY_RECOMMENDED` for fields that require external information not documented in this dataset (`Manufacturer`, `ManufacturersModelName`, `SoftwareVersions`, `DeviceSerialNumber`, `CapManufacturer`, `CapManufacturersModelName`, `EEGGround`, `HardwareFilters`, `HeadCircumference`, `SubjectArtefactDescription`, `TaskDescription`, `Instructions`, `CogAtlasID`, `CogPOID`, `InstitutionName`/`Address`/`DepartmentName`, `RecordingDuration`, `RecordingType`, `StimulusPresentation`, `HEDVersion`). These are left unset rather than filled with placeholders so the dataset remains defensible.
