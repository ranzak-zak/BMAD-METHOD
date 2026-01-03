# Epic 2: Canvas Interface - User Stories

---

## US-008-FE: Canvas Initialization (Frontend)

**As a** lawyer entering Document Mode,
**I want to** see the Canvas editor open automatically,
**So that** I can start working on my document.

**Role:** Frontend

### Description
Initialize and render the Canvas interface when document generation begins. Canvas opens as a new tab in the left panel.

### UI Location
- Left panel (alongside Citations tab)
- New tab labeled "Document"

### Behavior
1. Document Mode triggers Canvas initialization
2. Canvas tab is created and auto-focused
3. Canvas shares visual identity with Document Zone (chat sections related to document creation) to reflect that both are part of Create Document Mode

### Acceptance Criteria
- [ ] Canvas opens in left panel as new tab
- [ ] Tab is labeled "Document"
- [ ] Canvas tab auto-focuses on initialization
- [ ] User can switch between Canvas and Citations tabs
- [ ] Visual identity matches Document Mode styling (purple/pink theme)

### Design Reference
See: Canvas mockup showing tab alongside Citations

---

## US-008-BE: Canvas Initialization (Backend)

**As a** lawyer entering Document Mode,
**I want to** see the Canvas editor open automatically,
**So that** I can start working on my document.

**Role:** Backend

### Description
Provide API endpoint to initialize Canvas session and begin document generation.

### API Endpoint
```
POST /api/canvas/init
Body: {
  session_id: string,
  context: object
}
Response: {
  canvas_id: string,
  document_id: string,
  status: "generating" | "ready"
}
```

### Acceptance Criteria
- [ ] API endpoint created and documented
- [ ] Creates canvas session linked to document mode session
- [ ] Triggers document generation process
- [ ] Returns canvas and document IDs
- [ ] Handles errors gracefully

---

## US-009-FE: Content Rendering Sequence (Frontend)

**As a** lawyer,
**I want to** see my document being generated progressively,
**So that** I understand the system is working and can start reading early.

**Role:** Frontend

### Description
Implement the three-phase rendering sequence for document content.

### Rendering Phases
1. **Phase 1 - Skeleton:** Display skeleton loader while processing
2. **Phase 2 - Initial Content:** Render first 2-3 paragraphs together
3. **Phase 3 - Streaming:** Stream remaining content with typing effect

### Acceptance Criteria
- [ ] Skeleton loader displays during initial processing
- [ ] First 2-3 paragraphs appear together (not streamed character by character)
- [ ] Remaining content streams smoothly
- [ ] Transitions between phases are smooth
- [ ] Editor becomes interactive after streaming completes

---

## US-009-BE: Content Rendering Sequence (Backend)

**As a** lawyer,
**I want to** see my document being generated progressively,
**So that** I understand the system is working and can start reading early.

**Role:** Backend

### Description
Implement streaming endpoint for document content delivery.

### API Endpoint
```
GET /api/canvas/{canvas_id}/stream
Response: Server-Sent Events (SSE)
  - event: "chunk"
    data: { content: string, is_initial: boolean }
  - event: "complete"
    data: { total_characters: number }
```

### Behavior
- First event contains initial 2-3 paragraphs (is_initial: true)
- Subsequent events contain smaller chunks for streaming
- Final event signals completion

### Acceptance Criteria
- [ ] SSE endpoint implemented
- [ ] First chunk contains 2-3 paragraphs
- [ ] Subsequent chunks are appropriately sized for smooth streaming
- [ ] Completion event sent when done
- [ ] Error handling for connection issues

---

## US-009-DATA: Content Rendering Sequence (Data/AI)

**As a** lawyer,
**I want to** see my document being generated progressively,
**So that** I understand the system is working and can start reading early.

**Role:** Data/AI

### Description
Design document generation prompt and chunking strategy.

### Generation Requirements
- Generate document based on context from Document Mode
- Apply Legal Document Template formatting
- Include citations where relevant
- Respect 20K character limit

### Chunking Strategy
- Initial chunk: First 2-3 complete paragraphs (logical break point)
- Subsequent chunks: Sentence or paragraph boundaries

### Acceptance Criteria
- [ ] Document generation prompt designed
- [ ] Output follows Legal Document Template format
- [ ] Chunking respects logical boundaries
- [ ] Generation respects character limit
- [ ] Quality consistent regardless of streaming

---

## US-010-FE: Rich Text Editor (Frontend)

**As a** lawyer,
**I want to** manually edit my document with full formatting options,
**So that** I can make precise corrections and adjustments.

**Role:** Frontend

### Description
Implement Rich Text Editor with comprehensive formatting capabilities.

### Toolbar Components
| Feature | Options |
|---------|---------|
| Alignment | Left, Right, Center, Justify |
| Text Direction | RTL, LTR toggle |
| Font Size | Size selector |
| Font Color | Color picker |
| Text Styles | Bold, Italic, Underline |
| Lists | Numbered, Bulleted (hierarchical) |
| Indentation | Increase, Decrease (also via TAB / SHIFT+TAB) |

### Behavior
- Editor is locked during content streaming
- Editor unlocks when streaming completes
- All formatting persists on save

### Acceptance Criteria
- [ ] All toolbar options functional
- [ ] RTL/LTR toggle works correctly
- [ ] Hierarchical lists supported (1, 1.1, 1.1.1)
- [ ] TAB/SHIFT+TAB for indentation works
- [ ] Editor locked during streaming
- [ ] Editor interactive after streaming completes
- [ ] Formatting preserved on save/export

### Design Reference
See: Canvas mockup showing toolbar

---

## US-011-FE: AI Co-Pilot Editing (Frontend)

**As a** lawyer,
**I want to** edit my document using natural language instructions,
**So that** I can make high-level changes without manual editing.

**Role:** Frontend

### Description
Enable document editing via chat instructions while Canvas is open.

### UI Behavior
- Prompt mode must be active to use AI Co-Pilot editing
- See US-012-FE for editing mode toggle details

### Example Instructions
- "Make the first paragraph shorter"
- "Change the tone to be more formal"
- "Add a section about liability"
- "Restructure the legal analysis"

### Acceptance Criteria
- [ ] Chat input active when in Prompt mode
- [ ] Instructions sent to AI for processing
- [ ] Document updates reflect in Canvas
- [ ] Updates are smooth (not full re-render)
- [ ] User can continue editing via chat or manual RTE

---

## US-011-BE: AI Co-Pilot Editing (Backend)

**As a** lawyer,
**I want to** edit my document using natural language instructions,
**So that** I can make high-level changes without manual editing.

**Role:** Backend

### Description
Implement API for processing editing instructions and updating document.

### API Endpoint
```
POST /api/canvas/{canvas_id}/edit
Body: {
  instruction: string,
  current_content: string
}
Response: {
  updated_content: string,
  changes: [
    {
      type: "insert" | "delete" | "replace",
      position: number,
      content: string
    }
  ]
}
```

### Acceptance Criteria
- [ ] API endpoint created
- [ ] Processes natural language instructions
- [ ] Returns updated content
- [ ] Returns change deltas for smooth UI update
- [ ] Maintains document structure and formatting

---

## US-011-DATA: AI Co-Pilot Editing (Data/AI)

**As a** lawyer,
**I want to** edit my document using natural language instructions,
**So that** I can make high-level changes without manual editing.

**Role:** Data/AI

### Description
Design AI prompts for interpreting editing instructions and generating updates.

### Prompt Requirements
- Understand editing intent from natural language
- Preserve existing formatting and structure
- Make targeted changes (not rewrite entire document)
- Maintain legal document conventions

### Supported Instruction Types
- Content changes (add, remove, modify)
- Structural changes (reorder, restructure)
- Tone/style changes
- Specific clause modifications

### Acceptance Criteria
- [ ] Editing prompt designed and tested
- [ ] Accurately interprets user intent
- [ ] Makes minimal necessary changes
- [ ] Preserves formatting
- [ ] Handles ambiguous instructions gracefully

---

## US-012-FE: Editing Mode Toggle (Frontend)

**As a** lawyer using the Canvas,
**I want to** switch between manual editing and AI-assisted editing,
**So that** I can choose the best editing method for my needs.

**Role:** Frontend

### Description
Implement toggle button to switch between Manual mode and Prompt mode, with corresponding chat behavior changes.

### UI Components
- Toggle button at bottom of Canvas
- Two states: Manual (pencil icon) / Prompt (stars icon)
- Default state: Manual mode

### Mode Behaviors

**Manual Mode (Default):**
- User edits directly in Rich Text Editor
- Chat prompt available for continuing legal research
- Chat operates outside Create Document Mode
- Chat takes on regular chat appearance (not Document Zone styling)
- No mode indicator in prompt field

**Prompt Mode:**
- Chat input is dedicated to document editing instructions
- Instructions sent to AI update document in real-time
- Mode indicator shows "Document" in prompt field
- Chat is part of Create Document Mode
- Chat takes on Document Zone styling (matching Canvas visual identity)

### Acceptance Criteria
- [ ] Toggle button visible at bottom of Canvas
- [ ] Default state is Manual mode
- [ ] Clicking toggle switches between modes
- [ ] Visual indication of current mode (icon change)
- [ ] Chat behavior changes according to mode
- [ ] Mode indicator appears in prompt field only in Prompt mode
- [ ] Switching modes preserves document content

### Design Reference
See: Canvas mockup showing pencil/stars toggle buttons

---

## US-013-FE: Undo/Redo (Frontend)

**As a** lawyer,
**I want to** undo and redo my changes,
**So that** I can recover from mistakes.

**Role:** Frontend

### Description
Implement undo/redo functionality with temporary history.

### Toolbar Location
- Undo/Redo buttons in Canvas toolbar

### History Lifecycle
- History starts when content is first injected
- History tracks both manual edits and AI edits
- History is cleared on save or cancel

### Acceptance Criteria
- [ ] Undo button reverts last change
- [ ] Redo button restores undone change
- [ ] Keyboard shortcuts work (Ctrl+Z, Ctrl+Y)
- [ ] History includes both manual and AI edits
- [ ] History cleared after save
- [ ] History cleared after cancel

---

## US-014-FE: Cancel Flow (Frontend)

**As a** lawyer,
**I want to** cancel document creation with a warning,
**So that** I don't accidentally lose my work.

**Role:** Frontend

### Description
Implement cancel button with confirmation modal.

### UI Components

**Cancel Button:**
- Label: "בטל" / "Cancel"
- Location: Canvas action bar

**Warning Modal:**
- Title: "האם ברצונך לבטל את יצוא המסמך?"
- Message: "השינויים שבצעת ימחקו ולא ניתן יהיה לשחזרם"
- Actions:
  - "לא" (No) → Close modal, return to editor
  - "כן" (Yes) → Discard changes, close Canvas

### Behavior on Confirm
1. Close modal
2. Discard all Canvas content
3. Clear editing history
4. Close Canvas tab
5. Return to Citations tab
6. Exit Document Mode
7. Focus returns to chat in Research Mode

### Acceptance Criteria
- [ ] Cancel button visible in Canvas
- [ ] Clicking Cancel opens warning modal
- [ ] "No" closes modal, preserves content
- [ ] "Yes" discards content and closes Canvas
- [ ] Canvas tab removed after cancel
- [ ] Returns to Citations tab

---

## US-015-FE: Unsaved Changes Guardrail (Frontend)

**As a** lawyer,
**I want to** be warned if I try to close Canvas without saving,
**So that** I don't accidentally lose my work.

**Role:** Frontend

### Description
Trigger warning modal when user attempts to close Canvas by any method other than Cancel button.

### Trigger Events
- Browser back button
- Closing browser tab
- Navigating away from chat
- Switching to different chat

### Warning Modal
Same modal as Cancel Flow (US-014)

### Acceptance Criteria
- [ ] Warning triggers on browser back
- [ ] Warning triggers on tab close (beforeunload)
- [ ] Warning triggers on navigation away
- [ ] No warning if content is already saved
- [ ] No warning if Canvas is empty/unchanged

---

## US-016-FE: Tab Management (Frontend)

**As a** lawyer,
**I want to** switch between Canvas and Citations tabs,
**So that** I can reference sources while editing.

**Role:** Frontend

### Description
Implement tab navigation in left panel.

### Tabs
- **Document:** Canvas editor (when active)
- **Citations:** Existing citations view

### Behavior
- Canvas tab added when Document Mode starts
- Canvas tab auto-focuses on creation
- User can freely switch between tabs
- Canvas tab removed after save or cancel

### Acceptance Criteria
- [ ] Both tabs visible when Canvas is active
- [ ] Clicking tab switches view
- [ ] Tab state preserved when switching
- [ ] Canvas tab removed on close
- [ ] Citations tab always available

---

## US-017-FE: Character Limit (Frontend)

**As a** lawyer,
**I want to** see how many characters I've used,
**So that** I know when I'm approaching the limit.

**Role:** Frontend

### Description
Display character count and enforce 20K limit.

### UI Components
- Character counter: "X / 20,000"
- Location: Bottom of Canvas or in toolbar
- Warning state when approaching limit (e.g., 90%)
- Error state when at limit

### Behavior
- Counter updates in real-time
- Warning at 18,000 characters (visual indicator)
- Prevent input beyond 20,000 characters
- AI edits also respect limit

### Acceptance Criteria
- [ ] Character counter visible
- [ ] Counter updates on every change
- [ ] Warning state at 90% (18K)
- [ ] Input blocked at 20K
- [ ] AI edits respect limit
- [ ] Clear feedback when limit reached

---

## US-017-BE: Character Limit (Backend)

**As a** lawyer,
**I want to** see how many characters I've used,
**So that** I know when I'm approaching the limit.

**Role:** Backend

### Description
Enforce character limit on document generation and edits.

### Validation
- Initial generation respects 20K limit
- AI edits validated against limit
- Save rejected if over limit

### API Updates
All content-modifying endpoints should:
- Validate character count
- Return error if limit exceeded
- Return current count in response

### Acceptance Criteria
- [ ] Generation stops at 20K characters
- [ ] AI edit endpoint validates limit
- [ ] Clear error message when limit exceeded
- [ ] Character count included in responses

---

## Stories Summary by Role

### Frontend (FE)
| Story | Title |
|-------|-------|
| US-008-FE | Canvas Initialization |
| US-009-FE | Content Rendering Sequence |
| US-010-FE | Rich Text Editor |
| US-011-FE | AI Co-Pilot Editing |
| US-012-FE | Editing Mode Toggle |
| US-013-FE | Undo/Redo |
| US-014-FE | Cancel Flow |
| US-015-FE | Unsaved Changes Guardrail |
| US-016-FE | Tab Management |
| US-017-FE | Character Limit |

### Backend (BE)
| Story | Title |
|-------|-------|
| US-008-BE | Canvas Initialization |
| US-009-BE | Content Rendering Sequence |
| US-011-BE | AI Co-Pilot Editing |
| US-017-BE | Character Limit |

### Data/AI (DATA)
| Story | Title |
|-------|-------|
| US-009-DATA | Content Rendering Sequence |
| US-011-DATA | AI Co-Pilot Editing |

---

## Story Dependencies

```
US-008-FE ──▶ US-008-BE ──▶ US-009-BE ──▶ US-009-DATA
                              │
                              ▼
US-009-FE ◀──────────────────┘
    │
    ▼
US-010-FE (unlocks after streaming)
    │
    ├──▶ US-011-FE ──▶ US-011-BE ──▶ US-011-DATA
    │        │
    │        ▼
    │    US-012-FE
    │
    ├──▶ US-013-FE
    │
    ├──▶ US-014-FE
    │
    ├──▶ US-015-FE
    │
    └──▶ US-017-FE ──▶ US-017-BE

US-016-FE (parallel to all)
```

## Definition of Done (All Stories)
- [ ] Code complete and reviewed
- [ ] Unit tests passing
- [ ] UI matches design specifications (FE)
- [ ] API documented in Swagger (BE)
- [ ] AI prompts tested and validated (DATA)
- [ ] Acceptance criteria verified
- [ ] RTL support verified
- [ ] Deployed to staging environment
