# Epic 1: Document Mode Initiation - User Stories

---

## US-001-FE: Entry Point A - Response Menu (Frontend)

**As a** lawyer using the chat interface,
**I want to** create a document based on a specific chat response,
**So that** I can transform research findings into a formal document.

**Role:** Frontend

### Description
Add a "Create Document" button to the action menu below each response in the chat.

### UI Location
- Action menu below chat response bubbles (alongside copy, thumbs up/down, download)

### Visibility Rules
- Visible for all responses within a "Legal Question" chat type

### Acceptance Criteria
- [ ] "Create Document" button appears in response action menu
- [ ] Button is visible only in Legal Question chat type
- [ ] Button has correct icon and label ("צור מסמך")
- [ ] Clicking button calls API to enter Document Mode
- [ ] Response ID is passed to API call

### Design Reference
See: Entry Point A mockup (response menu with "צור מסמך" button)

---

## US-001-BE: Entry Point A - Response Menu (Backend)

**As a** lawyer using the chat interface,
**I want to** create a document based on a specific chat response,
**So that** I can transform research findings into a formal document.

**Role:** Backend

### Description
Create API endpoint to initiate Document Mode from a specific response.

### API Endpoint
```
POST /api/document-mode/init
Body: {
  chat_id: string,
  response_id: string,
  entry_point: "response_menu"
}
Response: {
  session_id: string,
  mode: "document",
  context: object,
  proceed_to: "canvas" | "info_gathering"
}
```

### Acceptance Criteria
- [ ] API endpoint created and documented
- [ ] Endpoint extracts context from specified response
- [ ] Endpoint triggers Context Evaluation (US-004)
- [ ] Returns session ID and next step
- [ ] Error handling for invalid chat/response IDs

---

## US-002-FE: Entry Point B - Main Menu (Frontend)

**As a** lawyer,
**I want to** start creating a document from the main menu,
**So that** I can begin document creation without an existing chat context.

**Role:** Frontend

### Description
Add a "Create Document" option to the main navigation menu (sidebar).

### UI Location
- Main navigation sidebar/dashboard
- Listed alongside: Legal Question, Document Analysis, AI Thoughts, Search

### Button Components
- **Icon:** Document creation icon
- **Label:** "יצירת מסמך" / "Create Document"
- **Badge:** "Beta" label (optional)

### Visibility Rules
- Always visible to user regardless of current screen

### Acceptance Criteria
- [ ] "Create Document" option appears in main navigation menu
- [ ] Button displays correct icon and label
- [ ] "Beta" badge displayed if applicable
- [ ] Clicking calls API to create new chat in Document Mode
- [ ] User is navigated to new chat

### Design Reference
See: Main menu mockup showing "יצירת מסמך" option

---

## US-002-BE: Entry Point B - Main Menu (Backend)

**As a** lawyer,
**I want to** start creating a document from the main menu,
**So that** I can begin document creation without an existing chat context.

**Role:** Backend

### Description
Create API endpoint to initialize a new chat thread in Document Mode.

### API Endpoint
```
POST /api/document-mode/init-new
Body: {
  entry_point: "main_menu"
}
Response: {
  chat_id: string,
  session_id: string,
  mode: "document",
  proceed_to: "info_gathering"
}
```

### Acceptance Criteria
- [ ] API endpoint created and documented
- [ ] Endpoint creates new chat thread
- [ ] Chat is flagged as Document Mode
- [ ] Returns new chat ID and session ID
- [ ] Always proceeds to Info Gathering (no prior context)

---

## US-003-FE: Entry Point C - Prompt Selector (Frontend)

**As a** lawyer composing a query,
**I want to** specify that my prompt should generate a document,
**So that** the system knows my intent before processing my request.

**Role:** Frontend

### Description
Add a mode selector dropdown inside the prompt input field.

### UI Location
- Dropdown inside prompt input field (left side, internal)

### Visibility Rules
- Always visible when prompt input is active

### Behavior
1. User selects "Document" from dropdown
2. User types their query
3. On submit, mode is included in API call

### Acceptance Criteria
- [ ] Mode selector dropdown appears in prompt field
- [ ] "Document" option is available in dropdown
- [ ] Selection persists while typing query
- [ ] Visual indicator shows selected mode
- [ ] Mode is passed to submit API call

### Design Reference
See: Prompt field mockup with "מסמך" dropdown selector

---

## US-003-BE: Entry Point C - Prompt Selector (Backend)

**As a** lawyer composing a query,
**I want to** specify that my prompt should generate a document,
**So that** the system knows my intent before processing my request.

**Role:** Backend

### Description
Extend chat submit endpoint to handle document mode flag.

### API Endpoint
```
POST /api/chat/submit
Body: {
  chat_id: string,
  message: string,
  mode: "research" | "document"  // NEW FIELD
}
Response: {
  // existing fields...
  document_mode: {
    session_id: string,
    proceed_to: "canvas" | "info_gathering"
  } | null
}
```

### Acceptance Criteria
- [ ] Chat submit endpoint accepts mode parameter
- [ ] When mode="document", triggers Document Mode flow
- [ ] Context Evaluation runs on message + chat history
- [ ] Returns document mode session info

---

## US-004-BE: Context Evaluation (Backend)

**As a** system,
**I want to** evaluate if sufficient context exists for document creation,
**So that** I can either proceed directly or gather missing information.

**Role:** Backend

### Description
Implement logic to analyze available context and determine if enough information exists to generate a quality document.

### Evaluation Criteria
System checks for:
- Document type (what kind of document is needed)
- Parties involved (who is the document for/about)
- Key facts (relevant case details)
- Legal basis (applicable laws/precedents)
- Desired outcome (what the document should achieve)

### Decision Logic
```
IF sufficient_context THEN
    → Return proceed_to: "canvas"
ELSE
    → Return proceed_to: "info_gathering"
    → Return missing_fields: [...]
```

### Acceptance Criteria
- [ ] Evaluation function implemented
- [ ] Checks all required criteria
- [ ] Evaluation completes within <3 seconds
- [ ] Returns clear proceed_to decision
- [ ] Returns list of missing fields when insufficient
- [ ] Decision is logged for debugging/improvement

---

## US-004-DATA: Context Evaluation (Data/AI)

**As a** system,
**I want to** evaluate if sufficient context exists for document creation,
**So that** I can either proceed directly or gather missing information.

**Role:** Data/AI

### Description
Design and implement AI prompt/model logic for context evaluation.

### AI Prompt Design
Create prompt that:
- Analyzes chat history and current context
- Identifies document type intent
- Extracts key entities (parties, facts, legal issues)
- Determines completeness score
- Lists specific missing information

### Output Schema
```json
{
  "sufficient": boolean,
  "confidence": float,
  "document_type": string | null,
  "extracted_context": {
    "parties": [...],
    "facts": [...],
    "legal_basis": [...],
    "desired_outcome": string | null
  },
  "missing_fields": [
    {
      "field": string,
      "importance": "required" | "recommended",
      "suggested_question": string
    }
  ]
}
```

### Acceptance Criteria
- [ ] AI prompt designed and tested
- [ ] Accurately identifies document type
- [ ] Extracts relevant context from chat history
- [ ] Generates helpful questions for missing fields
- [ ] Performance within latency requirements

---

## US-005-FE: Info Gathering Dialog (Frontend)

**As a** lawyer in Document Mode,
**I want to** provide additional information through a guided dialog,
**So that** the system can generate a more accurate document.

**Role:** Frontend

### Description
Implement the "Document Zone" UI for the information gathering dialog.

### Document Zone UI
- Distinct background color (purple/pink gradient)
- Clear visual boundary from regular chat
- Mode indicator badge/header
- "Create Document" button visible (US-006)

### Persistence
- Document Zone conversation is saved in chat history
- Remains visible even after user exits Document Mode and returns to Research Mode
- Document Zone section is collapsible (collapse/expand toggle)

### Acceptance Criteria
- [ ] Document Zone has distinct visual styling
- [ ] Clear visual boundary from regular chat
- [ ] Mode indicator badge visible
- [ ] AI questions render correctly
- [ ] User input field works within zone
- [ ] Previous chat history remains visible but separated

---

## US-005-BE: Info Gathering Dialog (Backend)

**As a** lawyer in Document Mode,
**I want to** provide additional information through a guided dialog,
**So that** the system can generate a more accurate document.

**Role:** Backend

### Description
Implement API to handle info gathering conversation flow.

### API Endpoint
```
POST /api/document-mode/gather-info
Body: {
  session_id: string,
  user_response: string
}
Response: {
  next_question: string | null,
  context_updated: boolean,
  proceed_to: "continue" | "canvas"
}
```

### Acceptance Criteria
- [ ] API endpoint created
- [ ] Stores user responses in session context
- [ ] Triggers re-evaluation after each response
- [ ] Returns next question or signals completion
- [ ] Handles conversation state

---

## US-005-DATA: Info Gathering Dialog (Data/AI)

**As a** lawyer in Document Mode,
**I want to** provide additional information through a guided dialog,
**So that** the system can generate a more accurate document.

**Role:** Data/AI

### Description
Design AI prompts for generating targeted questions.

### Question Generation Logic
- Questions based on missing_fields from US-004
- Questions should be specific, not open-ended
- Provide suggestions/examples where helpful
- Adjust based on document type

### Example Questions
- "What type of document do you need? (e.g., legal opinion, contract, court filing)"
- "Who are the parties involved?"
- "What is the main legal issue you're addressing?"

### Acceptance Criteria
- [ ] Question generation prompt designed
- [ ] Questions are relevant and focused
- [ ] Questions adapt to document type
- [ ] Provides helpful suggestions
- [ ] Understands user responses correctly

---

## US-006-FE: Manual Generation Trigger (Frontend)

**As a** lawyer in Info Gathering,
**I want to** force document generation at any point,
**So that** I can proceed even if the system wants more information.

**Role:** Frontend

### Description
Add "Create Document" button within Document Zone that allows skipping remaining questions.

### UI Location
- Inside Document Zone (within the Document Mode chat area)
- Not part of the main chat action bar

### Button Label
- Hebrew: "צור מסמך"
- English: "Create Document"

### Acceptance Criteria
- [ ] "Create Document" button visible during Info Gathering
- [ ] Button is clearly distinguishable from regular chat actions
- [ ] Clicking calls API to force generation
- [ ] Loading state while processing

---

## US-006-BE: Manual Generation Trigger (Backend)

**As a** lawyer in Info Gathering,
**I want to** force document generation at any point,
**So that** I can proceed even if the system wants more information.

**Role:** Backend

### Description
Implement API endpoint to force document generation with current context.

### API Endpoint
```
POST /api/document-mode/force-generate
Body: {
  session_id: string
}
Response: {
  proceed_to: "canvas",
  context: object
}
```

### Acceptance Criteria
- [ ] API endpoint created
- [ ] Stops info gathering flow
- [ ] Compiles all available context
- [ ] Triggers document generation (Epic 2)
- [ ] Returns signal to open Canvas

---

## US-007-FE: Document Mode Visual State (Frontend)

**As a** lawyer,
**I want to** clearly see when I'm in Document Mode,
**So that** I understand the system is focused on document creation.

**Role:** Frontend

### Description
Implement consistent visual indicators throughout Document Mode.

### Visual Elements

**Chat Area:**
- Document Zone background color (purple/pink)
- Mode indicator header/badge
- Clear boundary from previous chat history

**Prompt Field:**
- "Document" indicator visible
- Distinct styling when in Document Mode

**Overall:**
- Consistent color scheme across all Document Mode elements

### Acceptance Criteria
- [ ] Document Zone has distinct background color
- [ ] Mode indicator clearly visible
- [ ] Prompt field shows Document Mode state
- [ ] Visual transition is smooth when entering/exiting mode
- [ ] All Document Mode elements share consistent styling
- [ ] User can easily distinguish Document Mode from Research Mode

---

## Stories Summary by Role

### Frontend (FE)
| Story | Title |
|-------|-------|
| US-001-FE | Entry Point A - Response Menu |
| US-002-FE | Entry Point B - Main Menu |
| US-003-FE | Entry Point C - Prompt Selector |
| US-005-FE | Info Gathering Dialog |
| US-006-FE | Manual Generation Trigger |
| US-007-FE | Document Mode Visual State |

### Backend (BE)
| Story | Title |
|-------|-------|
| US-001-BE | Entry Point A - Response Menu |
| US-002-BE | Entry Point B - Main Menu |
| US-003-BE | Entry Point C - Prompt Selector |
| US-004-BE | Context Evaluation |
| US-005-BE | Info Gathering Dialog |
| US-006-BE | Manual Generation Trigger |

### Data/AI (DATA)
| Story | Title |
|-------|-------|
| US-004-DATA | Context Evaluation |
| US-005-DATA | Info Gathering Dialog |

---

## Story Dependencies

```
US-001-FE ──▶ US-001-BE ─┐
US-002-FE ──▶ US-002-BE ─┼──▶ US-004-BE ──▶ US-005-BE ──▶ US-006-BE
US-003-FE ──▶ US-003-BE ─┘        │              │
                                  ▼              ▼
                            US-004-DATA    US-005-DATA

US-007-FE (applies throughout all FE stories)
US-005-FE + US-006-FE (depend on US-005-BE)
```

## Definition of Done (All Stories)
- [ ] Code complete and reviewed
- [ ] Unit tests passing
- [ ] UI matches design specifications (FE)
- [ ] API documented in Swagger (BE)
- [ ] AI prompts tested and validated (DATA)
- [ ] Acceptance criteria verified
- [ ] Deployed to staging environment
