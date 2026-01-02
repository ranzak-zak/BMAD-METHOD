# Create Legal Document - Feature Overview

## Purpose
Enable lawyers to create legal documents directly within the system, based on legal research conducted in chat, with integrated AI writing and editing capabilities.

## System Context
This feature is part of the **legal-document-manager** module and integrates with existing capabilities:
- **Legal Research Chat** - already exists; provides context for document creation
- **Citations System** - already exists; case law and legislation integrated into documents

**Strategic Goal:** Integrating document creation with existing chat and citations capabilities transforms the system into a comprehensive **Legal Case Management System**.

## Create Legal Document Flow

The system has two main modes:
- **Research Mode** - Regular chat for legal research
- **Document Mode** - Activated when user requests to create a document, remains active until document is saved

```
                              ┌─────────────────────────────────────────────┐
                              │             DOCUMENT MODE                   │
┌───────────────────┐         │  ┌─────────────┐  ┌─────────┐  ┌─────────┐  │
│  1. ENTRY POINT   │────────▶│  │ 2. INFO     │─▶│ 3.CANVAS│─▶│ 4. SAVE │  │
│    (3 options)    │         │  │  GATHERING  │  │(editing)│  │(storage)│  │
└───────────────────┘         │  └─────────────┘  └─────────┘  └─────────┘  │
                              └─────────────────────────────────────────────┘
```

### Step 1: Entry Point
User initiates document creation via one of three options:
- **A** - "Create Document" button from response action menu in chat
- **B** - "Create Document" from main navigation menu
- **C** - Select "Document" from prompt field dropdown selector

---

### Document Mode (Steps 2-4)
Once user requests document creation, the system enters **Document Mode**. This mode is visually distinct from Research Mode and remains active until the document is saved or cancelled.

### Step 2: Info Gathering
System evaluates available context:
- **Sufficient info** → Proceeds directly to Canvas
- **Missing info** → Conducts information gathering dialog in visually distinct "Document Zone"
- User can click "Create Document" at any point to skip remaining questions

### Step 3: Canvas
Document editor opens in left panel:
- **Rendering sequence:** Skeleton loader → First 2-3 paragraphs → Streaming rest
- **Two editing modes:** Manual (Rich Text Editor) + AI Co-Pilot via chat
- **Constraints:** 20K character limit, temporary Undo/Redo history

### Step 4: Save
User saves the document:
- Click "Save Document" → Modal for document name
- Document saved to Chat Thread folder
- Appears in Document Carousel at bottom of chat
- Canvas closes → Returns to Citations tab → Exits Document Mode

User can reopen saved documents from the Carousel to edit in Canvas.

## Document Types in System (Broader Context)

> **Note:** This feature covers **only** document creation (type 3).
> The first two types belong to the broader legal-document-manager module and are out of scope.

| Type | Description | Examples | Status |
|------|-------------|----------|--------|
| Formal | Legal sources in database | Case law, Legislation | ✅ Already exists |
| Uploaded | Files uploaded by user | Contracts, Letters | ✅ Already exists |
| **Created** | **Documents created in system** | Drafts, Legal opinions | 🆕 **This feature** |

**⚠️ Current assumption: Documents are linked to a specific Chat. In future iterations, both documents and chats will be part of a Case (folder).**

## Legal Document Templates
Documents are based on two types of templates:

**1. Visual Template**
Uniform visual formatting for all documents displayed on Canvas and exported files. Follows standard legal document formatting conventions commonly used by lawyers: font type, font size, hierarchical structure reflected in headings and numbering, line spacing, paragraph spacing, etc.

**2. Content Templates**
Pre-defined structures for common legal document types:
- Court filings (briefs, motions, claims)
- Contracts
- General legal documents

## Current Limitations
| Limitation | Details |
|------------|---------|
| Characters | Maximum 20,000 characters in Canvas |
| Templates | Limited at two levels: single visual template, limited content templates |
| Versions | Save overwrites previous version |
| History | Undo/Redo cleared on save/cancel |

## Future Roadmap
- [ ] Additional templates (Statement of Claim, Contract, Warning Letter)
- [ ] Version management
- [ ] Track Changes
- [ ] Document sharing with colleagues

## Epics
| Epic | Name | Description |
|------|------|-------------|
| 1 | Document Mode Initiation | Entry points and information gathering dialog |
| 2 | Canvas Interface | Document editor and editing capabilities |
| 3 | Save & Document Management | Saving, carousel, and document actions |
