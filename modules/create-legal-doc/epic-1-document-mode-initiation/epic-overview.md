# Epic 1: Document Mode Initiation

## Overview
This epic covers the entry points for document creation and the transition from Research Mode to Document Mode, including the information gathering phase.

## Scope
- Three entry points to initiate document creation
- System logic to evaluate context sufficiency
- Information gathering dialog ("Document Zone")
- Manual override to force document generation

## User Stories

| ID | Title | Description |
|----|-------|-------------|
| US-001 | Entry Point A: Response Menu | Create document from chat response action menu |
| US-002 | Entry Point B: Main Menu | Create document from main navigation menu |
| US-003 | Entry Point C: Prompt Selector | Create document via prompt field dropdown |
| US-004 | Context Evaluation | System evaluates if sufficient info exists |
| US-005 | Info Gathering Dialog | Document Zone for collecting missing information |
| US-006 | Manual Generation Trigger | User forces document creation with available info |
| US-007 | Document Mode Visual State | Visual distinction from Research Mode |

## Flow Diagram

```
                    ┌─────────────────────────────────────┐
                    │           ENTRY POINTS              │
                    │  ┌─────┐   ┌─────┐   ┌─────┐       │
                    │  │  A  │   │  B  │   │  C  │       │
                    │  └──┬──┘   └──┬──┘   └──┬──┘       │
                    └─────┼────────┼────────┼───────────┘
                          └────────┼────────┘
                                   ▼
                    ┌─────────────────────────────────────┐
                    │      ENTER DOCUMENT MODE            │
                    └─────────────────┬───────────────────┘
                                      ▼
                    ┌─────────────────────────────────────┐
                    │       CONTEXT EVALUATION            │
                    │      (Sufficient info?)             │
                    └─────────────────┬───────────────────┘
                                      │
                       ┌──────────────┴──────────────┐
                       ▼                             ▼
              ┌─────────────────┐          ┌─────────────────┐
              │   YES: Proceed  │          │   NO: Info      │
              │   to Canvas     │          │   Gathering     │
              │   (Epic 2)      │          │   Dialog        │
              └─────────────────┘          └────────┬────────┘
                                                    │
                                                    ▼
                                           ┌─────────────────┐
                                           │ User can force  │
                                           │ "Create Doc"    │
                                           │ at any point    │
                                           └────────┬────────┘
                                                    │
                                                    ▼
                                           ┌─────────────────┐
                                           │ Proceed to      │
                                           │ Canvas (Epic 2) │
                                           └─────────────────┘
```

## UI Components

### Entry Point A: Response Menu
- Location: Action menu below each chat response
- Visibility: Available in "Legal Question" chat type
- Trigger: User clicks "Create Document" button

### Entry Point B: Main Menu
- Location: Main navigation sidebar
- Visibility: Always visible
- Trigger: Opens new chat in Document Mode

### Entry Point C: Prompt Selector
- Location: Dropdown inside prompt input field (left side)
- Visibility: Always visible when prompt is active
- Trigger: User selects "Document" before sending query

### Document Zone (Info Gathering)
- Visual: Distinct background color (purple/pink)
- Location: Within chat area
- Contains: AI questions to gather context
- Shows: "Create Document" button for manual override

## Acceptance Criteria (Epic Level)
- [ ] User can initiate document creation from all three entry points
- [ ] System correctly evaluates context sufficiency
- [ ] Document Zone is visually distinct from regular chat
- [ ] User can force document generation at any point
- [ ] Successful initiation leads to Canvas opening (Epic 2)

## Dependencies
- Existing chat infrastructure
- Existing response action menu

## Out of Scope
- Canvas interface (Epic 2)
- Save functionality (Epic 3)
