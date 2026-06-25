# Frontend Spec: Invoice Folder Upload

## Goal
The user can upload a folder containing new invoice files so that LedgerGate can ingest and process those invoices.

## Context
This is the first milestone of the project. Before automatic intake exists, manual folder upload is the primary entry point for new invoice files. The frontend must make the upload process understandable, explicit, and trustworthy.

## Data Sources
- Local file or folder selection by the user
- Backend HTTP endpoint for upload and processing
- Local UI state for selection, submission, and result visibility

## Displayed Information
The upload UI must display at minimum:
- a control for selecting a folder or supported group of files
- the number of selected files
- the current upload state
- the current processing state if upload and processing are separate stages
- a visible result after processing completes

## User Actions
The user can:
- choose a folder or file group containing new invoice files
- review the selection before upload
- start the upload
- retry after a failed upload or failed processing
- clear the current selection before upload if such an action is implemented

## UI States

### Idle
No files are selected yet.

### Files Selected
The UI confirms that files have been selected and are ready to upload.

### Uploading
The UI shows that selected files are being transferred to the backend.

### Processing
The UI shows that uploaded files are being processed by the backend.

### Success
All selected files were accepted and processed successfully.

### Partial Success
At least one selected file was processed successfully and at least one file failed, was skipped, or was rejected.

### Error
The upload or processing operation failed in a way that prevented successful completion.

## Rules
- The UI must clearly distinguish between no selection, uploading, processing, and completed states.
- The frontend must not claim success before the backend confirms successful processing.
- If upload and processing are distinct backend phases, the UI must not collapse them into one ambiguous state.
- The UI must support partial success, because some files may be valid and others invalid.
- The upload flow must be understandable to non-technical users.
- The frontend must degrade gracefully if browser folder-upload capabilities differ and must still allow supported file selection.

## Edge Cases
- the selected folder is empty
- the folder contains non-invoice files
- the folder contains unsupported file types
- the folder contains many files
- upload succeeds but processing fails for some files
- network error occurs during upload
- backend returns an unexpected error without file-level detail

<!-- ## Acceptance Criteria
- The user can select invoice files for upload.
- After successful processing, the user can proceed to an invoice list where the newest imported invoices appear first.
- The UI visibly confirms file selection.
- Starting upload changes the UI into an in-progress state.
- The UI distinguishes upload state from processing state if both exist.
- The UI distinguishes full success, partial success, and error.
- Retry is possible after failure. -->

## Acceptance Criteria
- The user can select invoice files for upload.
- The UI visibly confirms file selection.
- Starting upload changes the UI into an in-progress state.
- The UI distinguishes upload state from processing state if both exist.
- The UI distinguishes full success, partial success, and error.
- Retry is possible after failure.
- After successful processing, the user can proceed to the invoice list.

## Derived Test Obligations

### Component Tests
- renders idle state
- renders selected state
- renders uploading state
- renders processing state
- renders success state
- renders partial-success state
- renders error state

### Interaction Tests
- selecting files updates visible selection summary
- clicking upload starts submission
- retry starts a new attempt after failure

### Integration-Like Frontend Tests
- mocked successful backend response leads to success UI
- mocked partial processing failure leads to partial-success UI
- mocked network failure leads to error UI
