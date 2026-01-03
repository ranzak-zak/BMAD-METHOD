# Epic 2: Canvas Interface

## Overview
This epic covers the Canvas document editor - initialization, rendering, editing capabilities (manual and AI-assisted), and session management.

## Scope
- Canvas initialization and content injection
- Content rendering sequence (skeleton → paragraphs → streaming)
- Rich Text Editor (RTE) with full formatting capabilities
- AI Co-Pilot editing via chat
- Undo/Redo functionality
- Cancel flow with guardrails
- Tab management (Canvas alongside Citations)

## User Stories

| ID | Title | Description |
|----|-------|-------------|
| US-010 | Canvas Initialization | Open Canvas and inject content |
| US-011 | Content Rendering | Skeleton → 2-3 paragraphs → streaming |
| US-012 | Rich Text Editor | Manual editing with full formatting |
| US-013 | AI Co-Pilot Editing | Edit document via chat instructions |
| US-014 | Undo/Redo | Temporary editing history |
| US-015 | Cancel Flow | Cancel with warning modal |
| US-016 | Unsaved Changes Guardrail | Prevent accidental data loss |
| US-017 | Tab Management | Canvas tab alongside Citations |
| US-018 | Character Limit | 20K character maximum |

## Flow Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                         CANVAS INTERFACE                        │
│                                                                 │
│  ┌──────────────────┐                                          │
│  │  INITIALIZATION  │                                          │
│  │  (from Epic 1)   │                                          │
│  └────────┬─────────┘                                          │
│           ▼                                                     │
│  ┌──────────────────┐                                          │
│  │    RENDERING     │                                          │
│  │  1. Skeleton     │                                          │
│  │  2. 2-3 paragraphs                                          │
│  │  3. Streaming    │                                          │
│  └────────┬─────────┘                                          │
│           ▼                                                     │
│  ┌──────────────────────────────────────────────────────┐      │
│  │              INTERACTIVE EDITING                      │      │
│  │  ┌─────────────────┐    ┌─────────────────┐          │      │
│  │  │  Manual (RTE)   │◄──►│  AI Co-Pilot    │          │      │
│  │  │                 │    │  (via chat)     │          │      │
│  │  └─────────────────┘    └─────────────────┘          │      │
│  │                                                       │      │
│  │  [Undo/Redo]  [Character Count: X/20,000]            │      │
│  └──────────────────────────────────────────────────────┘      │
│           │                                                     │
│           ▼                                                     │
│  ┌──────────────────┐    ┌──────────────────┐                  │
│  │  CANCEL          │    │  SAVE            │                  │
│  │  (warning modal) │    │  (Epic 3)        │                  │
│  └──────────────────┘    └──────────────────┘                  │
└─────────────────────────────────────────────────────────────────┘
```

## UI Components

### Canvas Panel
- Location: Left side panel
- Tab: "Document" tab next to "Citations" tab
- Auto-focus: Canvas tab activates on initialization

### Canvas Header (Document Metadata)
Editable fields at the top of Canvas for each document:
- **Document Name** - editable text field
- **Document Type** - selectable/editable field
- **Brief Description** - short description field

### Toolbar
- Alignment: Left, Right, Center, Justify
- Text Direction: RTL / LTR toggle
- Typography: Font size, Font color, Bold, Italic, Underline
- Lists: Numbered, Bulleted (hierarchical)
- Indentation: Increase / Decrease

### Action Buttons
- **Cancel (בטל):** Triggers warning modal
- **Save Document (צור מסמך):** Saves to chat (Epic 3)

### Editing Modes
| Mode | Interface | Use Case |
|------|-----------|----------|
| Manual | Rich Text Editor | Direct text manipulation, precise corrections |
| AI Co-Pilot | Chat input | High-level instructions, structural changes |

## Technical Constraints

| Constraint | Value |
|------------|-------|
| Max Characters | 20,000 |
| History Lifecycle | Cleared on save/cancel |
| Supported Formats | RTL + LTR |

## Acceptance Criteria (Epic Level)
- [ ] Canvas opens with content from Document Mode
- [ ] Content renders in correct sequence
- [ ] RTE supports all formatting options
- [ ] AI Co-Pilot updates document in real-time
- [ ] Undo/Redo works correctly
- [ ] Cancel triggers warning modal
- [ ] Unsaved changes are protected
- [ ] Character limit enforced with indicator

## Dependencies
- Epic 1: Document Mode Initiation (provides context)
- Existing Citations tab infrastructure

## Out of Scope
- Save functionality (Epic 3)
- Document Carousel (Epic 3)
- Version management (future)
