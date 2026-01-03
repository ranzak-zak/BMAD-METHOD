# Epic 3: Save & Document Management - User Stories

---

## US-018-FE: Save Document Flow (Frontend)

**As a** lawyer with a document in Canvas,
**I want to** save my document with a custom name,
**So that** I can access it, update it, and export it later - just like a document on my personal computer.

**Role:** Frontend

### Description
Implement save button and naming modal for saving documents.

### UI Components

**Save Button:**
- Label: "צור מסמך" / "Save Document"
- Location: Canvas action bar
- State: Enabled when Canvas has new unsaved content

**Naming Modal:**
- Input field for document name
- Default value: "Legal_Response_[YYYY-MM-DD]"
- Buttons: Confirm / Cancel

### Behavior
1. User clicks "Save Document"
2. Naming modal opens with default name
3. User can edit name or accept default
4. User clicks Confirm
5. Document saves (loading indicator)
6. Canvas closes
7. Returns to Citations tab
8. Focus returns to chat in Research Mode
9. Document Card appears in chat

### Acceptance Criteria
- [ ] Save button visible in Canvas action bar
- [ ] Button disabled when no unsaved changes
- [ ] Naming modal opens on click
- [ ] Default name includes current date
- [ ] User can edit document name
- [ ] Loading indicator during save
- [ ] Canvas closes after successful save
- [ ] Document Card appears in chat

---

## US-018-BE: Save Document Flow (Backend)

**As a** lawyer with a document in Canvas,
**I want to** save my document with a custom name,
**So that** I can access it, update it, and export it later - just like a document on my personal computer.

**Role:** Backend

### Description
Implement API endpoint for saving document to chat.

### API Endpoint
```
POST /api/documents/save
Body: {
  chat_id: string,
  canvas_id: string,
  document_name: string,
  content: string,
  formatting: object
}
Response: {
  document_id: string,
  document_name: string,
  created_at: timestamp,
  chat_id: string
}
```

### Behavior
- Create document record linked to chat
- Store content and formatting
- Create chat folder if doesn't exist
- Return document metadata

### Acceptance Criteria
- [ ] API endpoint created and documented
- [ ] Document saved with name and content
- [ ] Document linked to chat ID
- [ ] Folder created automatically if needed
- [ ] Returns document metadata
- [ ] Error handling for save failures

---

## US-019-BE: Document-Chat Linkage (Backend)

**As a** system,
**I want to** link documents to specific chats,
**So that** documents are organized by chat context.

**Role:** Backend

### Description
Implement data model and logic for document-chat relationship.

### Data Model
```
Document {
  id: string (primary key)
  chat_id: string (foreign key)
  name: string
  content: text
  formatting: json
  created_at: timestamp
  updated_at: timestamp
}
```

### Constraints (Phase 1)
- One document per chat maximum
- If document exists for chat, save overwrites it

### Acceptance Criteria
- [ ] Document table created with chat_id foreign key
- [ ] One-to-one relationship enforced (Phase 1)
- [ ] Save to existing document overwrites content
- [ ] Document retrievable by chat_id
- [ ] Cascade delete when chat is deleted

---

## US-020-FE: Document Card (Frontend)

**As a** lawyer viewing a chat,
**I want to** see a card for my saved document,
**So that** I can quickly access it.

**Role:** Frontend

### Description
Display a Document Card at the bottom of chat for saved documents.

### UI Location
- Bottom of chat, above prompt field
- Visible only when document exists for this chat

### Card Components
- Document name
- Date/time created or last modified
- 3-dot menu icon for actions

### Acceptance Criteria
- [ ] Card appears when document exists for chat
- [ ] Card hidden when no document
- [ ] Document name displayed
- [ ] Date/time displayed
- [ ] 3-dot menu accessible
- [ ] Card styling matches chat design

---

## US-020-BE: Document Card (Backend)

**As a** lawyer viewing a chat,
**I want to** see a card for my saved document,
**So that** I can quickly access it.

**Role:** Backend

### Description
Provide API to retrieve document metadata for a chat.

### API Endpoint
```
GET /api/chats/{chat_id}/document
Response: {
  document: {
    id: string,
    name: string,
    created_at: timestamp,
    updated_at: timestamp
  } | null
}
```

### Acceptance Criteria
- [ ] API endpoint created
- [ ] Returns document metadata if exists
- [ ] Returns null if no document
- [ ] Efficient query (no content loading)

---

## US-021-FE: Reopen Document (Frontend)

**As a** lawyer,
**I want to** reopen my saved document for editing,
**So that** I can make changes to it.

**Role:** Frontend

### Description
Allow user to reopen saved document in Canvas by clicking Document Card or Edit action.

### Trigger Actions
- Click/tap on Document Card
- Click "Edit" (ערוך) from 3-dot menu

### Behavior
1. User triggers reopen action
2. System enters Document Mode
3. Canvas tab opens
4. Document content loads into editor
5. User can edit and save again

### Acceptance Criteria
- [ ] Clicking card opens Canvas
- [ ] Edit menu option opens Canvas
- [ ] Document Mode activated
- [ ] Document content loaded correctly
- [ ] All formatting preserved
- [ ] User can edit and re-save

---

## US-021-BE: Reopen Document (Backend)

**As a** lawyer,
**I want to** reopen my saved document for editing,
**So that** I can make changes to it.

**Role:** Backend

### Description
Provide API to retrieve full document content for editing.

### API Endpoint
```
GET /api/documents/{document_id}
Response: {
  id: string,
  chat_id: string,
  name: string,
  content: string,
  formatting: object,
  created_at: timestamp,
  updated_at: timestamp
}
```

### Acceptance Criteria
- [ ] API endpoint created
- [ ] Returns full document with content
- [ ] Returns formatting data
- [ ] Error handling for not found

---

## US-022-FE: Rename Document (Frontend)

**As a** lawyer,
**I want to** rename my document,
**So that** I can give it a more meaningful name.

**Role:** Frontend

### Description
Implement rename action from Document Card menu.

### UI Flow
1. User clicks 3-dot menu on Document Card
2. User selects "Rename" (שנה שם)
3. Modal opens with current name in input field
4. User edits name
5. User confirms
6. Card updates with new name

### Acceptance Criteria
- [ ] Rename option in menu
- [ ] Modal shows current name
- [ ] User can edit name
- [ ] Save updates card immediately
- [ ] Cancel closes modal without changes
- [ ] Validation for empty name

---

## US-022-BE: Rename Document (Backend)

**As a** lawyer,
**I want to** rename my document,
**So that** I can give it a more meaningful name.

**Role:** Backend

### Description
Implement API endpoint for renaming document.

### API Endpoint
```
PATCH /api/documents/{document_id}/rename
Body: {
  name: string
}
Response: {
  id: string,
  name: string,
  updated_at: timestamp
}
```

### Acceptance Criteria
- [ ] API endpoint created
- [ ] Name updated in database
- [ ] updated_at timestamp refreshed
- [ ] Validation for empty/invalid name
- [ ] Error handling for not found

---

## US-023-FE: Download Document (Frontend)

**As a** lawyer,
**I want to** download my document,
**So that** I can use it outside the system.

**Role:** Frontend

### Description
Implement download action from Document Card menu.

### UI Flow
1. User clicks 3-dot menu on Document Card
2. User selects "Download" (הורד)
3. File downloads to user's device

### Export Format
- Format follows Legal Document Template styling
- File type: TBD (docx, pdf, or both)

### Acceptance Criteria
- [ ] Download option in menu
- [ ] File downloads on click
- [ ] File name matches document name
- [ ] Formatting preserved in export
- [ ] Loading indicator during export

---

## US-023-BE: Download Document (Backend)

**As a** lawyer,
**I want to** download my document,
**So that** I can use it outside the system.

**Role:** Backend

### Description
Implement API endpoint for document export.

### API Endpoint
```
GET /api/documents/{document_id}/export
Query: {
  format: "docx" | "pdf"
}
Response: Binary file stream
Headers: {
  Content-Type: application/octet-stream,
  Content-Disposition: attachment; filename="{name}.{format}"
}
```

### Acceptance Criteria
- [ ] API endpoint created
- [ ] Generates properly formatted file
- [ ] Legal Document Template applied
- [ ] RTL support in export
- [ ] Correct content-type headers

---

## US-024-FE: Delete Document (Frontend)

**As a** lawyer,
**I want to** delete my document,
**So that** I can remove documents I no longer need.

**Role:** Frontend

### Description
Implement delete action with confirmation from Document Card menu.

### UI Flow
1. User clicks 3-dot menu on Document Card
2. User selects "Delete" (מחק) - displayed in red
3. Confirmation modal appears
4. User confirms deletion
5. Document Card disappears

### Confirmation Modal
- Title: "מחיקת מסמך"
- Message: "האם אתה בטוח שברצונך למחוק את המסמך? פעולה זו אינה ניתנת לביטול."
- Buttons: "לא" (No) / "כן, מחק" (Yes, delete)

### Acceptance Criteria
- [ ] Delete option in menu (red text)
- [ ] Confirmation modal appears
- [ ] "No" cancels without deletion
- [ ] "Yes" deletes and removes card
- [ ] Success feedback to user

---

## US-024-BE: Delete Document (Backend)

**As a** lawyer,
**I want to** delete my document,
**So that** I can remove documents I no longer need.

**Role:** Backend

### Description
Implement API endpoint for document deletion.

### API Endpoint
```
DELETE /api/documents/{document_id}
Response: {
  success: boolean,
  deleted_id: string
}
```

### Acceptance Criteria
- [ ] API endpoint created
- [ ] Document removed from database
- [ ] Associated files cleaned up
- [ ] Error handling for not found
- [ ] Returns success confirmation

---

## Stories Summary by Role

### Frontend (FE)
| Story | Title |
|-------|-------|
| US-018-FE | Save Document Flow |
| US-020-FE | Document Card |
| US-021-FE | Reopen Document |
| US-022-FE | Rename Document |
| US-023-FE | Download Document |
| US-024-FE | Delete Document |

### Backend (BE)
| Story | Title |
|-------|-------|
| US-018-BE | Save Document Flow |
| US-019-BE | Document-Chat Linkage |
| US-020-BE | Document Card |
| US-021-BE | Reopen Document |
| US-022-BE | Rename Document |
| US-023-BE | Download Document |
| US-024-BE | Delete Document |

---

## Story Dependencies

```
US-019-BE (data model - must be first)
    │
    ▼
US-018-BE ◀── US-018-FE
    │
    ▼
US-020-BE ◀── US-020-FE
    │
    ├──▶ US-021-BE ◀── US-021-FE
    │
    ├──▶ US-022-BE ◀── US-022-FE
    │
    ├──▶ US-023-BE ◀── US-023-FE
    │
    └──▶ US-024-BE ◀── US-024-FE
```

## Definition of Done (All Stories)
- [ ] Code complete and reviewed
- [ ] Unit tests passing
- [ ] UI matches design specifications (FE)
- [ ] API documented in Swagger (BE)
- [ ] Acceptance criteria verified
- [ ] RTL support verified
- [ ] Deployed to staging environment
