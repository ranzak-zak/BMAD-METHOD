# Epic 3: Save & Document Management

## Overview
This epic covers saving documents from Canvas, the Document Card for accessing saved documents, and document management actions (rename, download, delete).

> **Phase 1 Constraint:** Current development phase supports only **one document per chat**. Multiple documents per chat (with Carousel UI) will be implemented in a future development cycle.

## Scope
- Save document flow with naming modal
- Document-to-Chat linkage
- Document Card UI (single document per chat)
- Document actions: Rename, Download, Delete
- Reopen saved document for editing

## User Stories

| ID | Title | Description |
|----|-------|-------------|
| US-018 | Save Document Flow | Save button, naming modal, save to chat |
| US-019 | Document-Chat Linkage | Link document to specific Chat ID |
| US-020 | Document Card | Display saved document in chat |
| US-021 | Reopen Document | Open saved document in Canvas + enter Document Mode |
| US-022 | Rename Document | Change document name |
| US-023 | Download Document | Export document to file |
| US-024 | Delete Document | Remove document with confirmation |

## Flow Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                         SAVE FLOW                               │
│                                                                 │
│  ┌──────────────────┐                                          │
│  │  User clicks     │                                          │
│  │  "Save Document" │                                          │
│  └────────┬─────────┘                                          │
│           ▼                                                     │
│  ┌──────────────────┐                                          │
│  │  Naming Modal    │                                          │
│  │  (enter name)    │                                          │
│  └────────┬─────────┘                                          │
│           ▼                                                     │
│  ┌──────────────────┐                                          │
│  │  Save to Chat    │                                          │
│  │  Thread folder   │                                          │
│  └────────┬─────────┘                                          │
│           ▼                                                     │
│  ┌──────────────────┐                                          │
│  │  Close Canvas    │                                          │
│  │  Exit Doc Mode   │                                          │
│  └────────┬─────────┘                                          │
│           ▼                                                     │
│  ┌──────────────────┐                                          │
│  │  Document appears│                                          │
│  │  in Document Card│                                          │
│  └──────────────────┘                                          │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                    DOCUMENT CARD                                │
│                                                                 │
│  ┌─────────────────┐                                           │
│  │  Document Name  │                                           │
│  │  Date/Time      │                                           │
│  │       ⋮         │                                           │
│  └─────────────────┘                                           │
│                                                                 │
│  Actions:                                                       │
│  • Click card → Reopen in Canvas + enter Document Mode         │
│  • ⋮ Menu → Edit | Rename | Download | Delete                  │
└─────────────────────────────────────────────────────────────────┘
```

## UI Components

### Save Button
- Label: "צור מסמך" / "Save Document"
- Location: Canvas action bar
- State: Enabled when Canvas has new unsaved content

### Naming Modal
- Input field for document name
- Default value: "Legal_Response_[Date]"
- Confirm / Cancel buttons

### Document Card
- Location: Bottom of chat, above prompt field
- Single document card per chat
- Card displays:
  - Document name
  - Date/time created
  - 3-dot menu for actions
- Click/tap card → Opens Canvas in editing mode and enters Document Mode

### Document Actions Menu
- Edit (ערוך) - Opens Canvas in editing mode
- Rename (שנה שם)
- Download (הורד)
- Delete (מחק) - in red

## Technical Details

### Document Storage
- Documents saved to Chat Thread folder
- Folder created automatically if doesn't exist
- Document linked to Chat ID (not specific response)

### Save Behavior
- Save overwrites previous version (no version history)
- Canvas closes after save
- Returns to Citations tab
- Focus returns to chat in Research Mode

### Reopen Behavior
- Click document card → Canvas opens + enters Document Mode
- Document content loaded into editor
- User can edit and save again

## Acceptance Criteria (Epic Level)
- [ ] User can save document with custom name
- [ ] Document linked to correct Chat
- [ ] Saved document appears in Document Card
- [ ] User can reopen and edit saved documents
- [ ] All document actions work (rename, download, delete)
- [ ] Delete shows confirmation before removing

## Dependencies
- Epic 2: Canvas Interface (provides save trigger)
- Existing chat infrastructure

## Out of Scope
- Multiple documents per chat with Carousel UI (Phase 2)
- Version history (future)
- Document sharing (future)
- Multiple export formats (future)
