# property-agent

**Product Requirements & Functional Design Document**  
**GitHub Repository:** https://github.com/Olivia-cyx98/property-agent  
**Version:** 1.0 (Draft)  
**Date:** 12 October 2026  
**Status:** For stakeholder review  
**Audience:** Product stakeholders, agency representatives, design, engineering, and QA

> **Document purpose:** This document consolidates the product requirements and intended functional behaviour discussed to date. It is **not** a final technical architecture specification. Unresolved product decisions are explicitly identified rather than assumed.

## 1. Executive Summary

property-agent enables real estate agencies to centralise property listings, import listing data from provider PDFs, maintain accurate listing information, and display selected properties to buyers and investors through agency-specific public pages hosted on the platform.

Agents within the same agency share access to the agency's listings. Agencies are isolated from one another. Platform administrators manage agencies and agent accounts. The initial release prioritises property listing operations and simple public presentation; advanced buyer features and integration with agencies' existing websites are deferred.

## 2. Objectives and Problem Statement

### 2.1 Problem

Real estate agents need a central place to consolidate and maintain listing information from external providers. Manual data entry, repetitive listing updates, and fragmented property records can make it harder to maintain consistent, up-to-date information for internal teams and prospective buyers.

*The above is the working problem statement and should be validated with agency stakeholders.*

### 2.2 Product objectives

- Centralise listing information at the agency level.
- Reduce manual data-entry effort through PDF import.
- Give agents control over review, correction, publication, and maintenance of listings.
- Make properties easy to search, filter, and inspect.
- Provide an accessible, straightforward public property browsing experience for buyers and investors.
- Maintain traceability of changes to listing fields.

### 2.3 Success measures (TBD)

No target metrics have been agreed. Potential measures for stakeholder discussion include import completion rate, time spent updating listings, parsing correction rate, and public listing engagement. These are suggestions, **not committed KPIs**.

## 3. Users and Tenancy Model

| Role | Description | Primary capabilities |
|---|---|---|
| Platform Administrator | Internal platform operator | Create and manage agencies and agent accounts |
| Agent | Member of a real estate agency | Import, view, edit, search, publish/hide, and manage agency listings |
| Buyer / Investor | Visitor to an agency's public page | Browse public listings, view details and images, search and filter |

### 3.1 Agency isolation

- Each agent belongs to **exactly one agency** in the MVP.
- An agency may have multiple agents.
- Agencies' internal listing data must be isolated from each other.
- Agents in the same agency can view and edit all listings belonging to their agency in the MVP.
- Agent-specific listing permissions and cross-agency membership are outside the current MVP scope.
- Buyer/investor browsing is initially public and does not require registration (**provisional decision**).

### 3.2 Website ownership

- Public property presentation is associated with an **agency**, not an individual agent.
- The MVP provides agency-specific public presentation within the platform.
- The exact URL, domain, and site-routing approach are undecided.
- Integration into an agency's existing external website is a future consideration, not an MVP commitment.

## 4. Scope and Release Priorities

### 4.1 MVP scope

1. Platform-managed agency and agent onboarding (no open self-registration).
2. Agency-scoped property repository and management interface.
3. PDF listing import supporting multiple properties in a document.
4. Change detection and pre-commit import review with editing, skipping, and removal of import entries.
5. Agent confirmation before imported changes are applied.
6. Manual property creation and editing (manual entry is selected as a planned intake method; final MVP delivery priority should be confirmed).
7. Property status, visibility, image management, and bulk operations.
8. Field-level change history.
9. Property card/grid views, detail pages, and image galleries.
10. Basic keyword search, filtering, and sorting.
11. Agency-specific public property pages with basic branding customisation.

### 4.2 Future / not in the initial MVP

- Provider API ingestion or automated third-party synchronisation (providers and APIs not selected).
- Embedding listings in, or supplying APIs to, an agency's pre-existing website.
- Buyer registration, saved searches, favourites, comparison tools, and map view.
- Granular agent-level access control.
- Advanced website page/layout editing.
- Export and generic bulk field editing.

### 4.3 Items requiring release prioritisation

Some capabilities were confirmed as desired but their exact MVP delivery dates were not explicitly agreed, notably **manual property creation** and any dedicated administration UI. These should be resolved at planning time rather than inferred from this document.

## 5. Core Product Workflows

### 5.1 PDF import

1. Agent uploads a provider PDF.
2. System extracts one or more listing records.
3. System attempts to identify which extracted properties already exist in the agency's records.
4. System identifies new listings and proposed changes to existing listings.
5. Agent opens an **Import Review** screen to inspect additions, differences, and errors.
6. Agent may correct extracted values, skip entries, or remove entries from this import batch.
7. Agent reviews or adjusts listing visibility settings.
8. Agent confirms the import.
9. Only the approved changes are applied to the agency's listing repository.

**Important:** Earlier shorthand described parsing and automatic saving. That has been superseded by the explicitly confirmed **review-before-commit** workflow above.

### 5.2 Routine listing management

1. Agent accesses their agency's property library.
2. Agent searches, filters, sorts, or opens a listing.
3. Agent changes details, status, images, or visibility, or selects a supported bulk action.
4. System saves valid edits and records changes in the field-level history.

### 5.3 Buyer browsing

1. Buyer opens an agency's public listing page without signing in (provisional).
2. Buyer browses visible listings in a grid/card presentation.
3. Buyer searches or filters listings and opens a detail page.
4. Buyer views property details and images.

Only properties marked **Public** appear in public browsing. Whether a **Hidden** listing is also inaccessible via a previously shared direct URL remains to be formally specified; restricting such direct access is the recommended requirement.

## 6. Functional Requirements

### 6.1 Authentication and platform administration

| ID | Requirement | Priority / decision |
|---|---|---|
| AUTH-001 | Agents must authenticate to access the management dashboard. | MVP / Confirmed |
| AUTH-002 | Platform administrators create agencies. | MVP / Confirmed |
| AUTH-003 | Platform administrators create or invite agents and associate them with one agency. | MVP / Confirmed; invitation mechanics TBD |
| AUTH-004 | Platform administrators edit and deactivate agencies and agent accounts. | MVP / Confirmed direction; deactivation effects TBD |
| AUTH-005 | Agents cannot register independently in the MVP. | MVP / Confirmed |
| AUTH-006 | Agents can access and edit listings only within their assigned agency. | MVP / Confirmed |

**Candidate agency fields:** Agency Name, Logo, Email, Phone, Office Address, Website/Domain. Agency Name requirement level remains TBD; other agency profile fields are currently optional.

**Agent profile fields:** Full Name (**required**), Email (**required**), Phone Number (**required**), Profile Photo (optional), Job Title (optional), Profile/Bio (optional). Email uniqueness and account invitation/activation rules remain TBD.

### 6.2 Property management

| ID | Requirement | Priority / decision |
|---|---|---|
| PROP-001 | Agents can browse and open details for their agency's listings. | MVP / Confirmed |
| PROP-002 | Agents can edit property information. | MVP / Confirmed |
| PROP-003 | Agents can delete individual property listings. | MVP / Confirmed; deletion semantics TBD |
| PROP-004 | Agents can manage listing status. | MVP / Confirmed |
| PROP-005 | Agents can add, modify, and remove property images. | MVP / Confirmed |
| PROP-006 | Agents can select multiple listings and bulk-update status. | MVP / Confirmed |
| PROP-007 | Agents can bulk-update visibility. | MVP / Confirmed |
| PROP-008 | Agents can bulk-delete selected listings. | MVP / Confirmed; deletion semantics TBD |
| PROP-009 | Manual listing entry is a planned creation method. | Planned / release priority TBD |

#### Listing status

The selected status options are **Available**, **Reserved / Under Offer**, **Sold**, and **Off Market**. Definitions and transitions may need to be adjusted against real provider data and stakeholder terminology.

#### Visibility

Visibility is **independent of listing status**, with **Public** and **Hidden** values. Agents in an agency can update visibility. Hidden properties remain in the internal agency repository but are not displayed in public browsing.

### 6.3 PDF import and review

| ID | Requirement | Priority / decision |
|---|---|---|
| IMP-001 | Agent can upload a provider PDF. | MVP / Confirmed |
| IMP-002 | The system extracts multiple listings from a single PDF when present. | MVP / Confirmed |
| IMP-003 | The system identifies candidate matches to existing agency listings. | MVP / Confirmed; matching algorithm TBD |
| IMP-004 | The review screen differentiates proposed additions and updates and shows field changes. | MVP / Confirmed |
| IMP-005 | Agent can correct extracted property fields before import. | MVP / Confirmed |
| IMP-006 | Agent can skip a listing in the current import. | MVP / Confirmed |
| IMP-007 | Agent can remove an entry from the current import batch without deleting any existing property. | MVP / Confirmed |
| IMP-008 | Agent can resolve parsing issues, skip problematic entries, or cancel the full import. | MVP / Confirmed |
| IMP-009 | An import requires explicit agent confirmation before applying changes. | MVP / Confirmed |
| IMP-010 | Existing property updates retain current visibility by default, show it in review, and permit agent changes. | MVP / Confirmed |
| IMP-011 | Visibility for new listings is chosen during import review. | MVP / Confirmed; preselected default TBD |
| IMP-012 | Confirmed changes update matched listings and add selected new listings. | MVP / Confirmed |

**Important distinction:** Deleting an item from the *review batch* is not the same as deleting a saved listing. The latter is covered by separate property-deletion requirements.

**Unresolved import rules:** How to match properties across PDF formats; mandatory fields and validation; handling uncertain matches; what happens when one reviewed record fails to save; whether properties absent from a newer provider listing should be kept, hidden, archived, or deleted. Absence from a PDF **must not** be treated as evidence of removal until it is established that the provider document is a complete authoritative inventory.

### 6.4 Search and filtering

| ID | Requirement | Priority |
|---|---|---|
| SEARCH-001 | Keyword search. | MVP |
| SEARCH-002 | Location filter. | MVP |
| SEARCH-003 | Price range filter. | MVP |
| SEARCH-004 | Property type filter. | MVP |
| SEARCH-005 | Bedroom-count filter. | MVP |
| SEARCH-006 | Listing status filter. | MVP |
| SEARCH-007 | Result sorting. | MVP |

Searchable fields, partial matching, filter combination, sort options, and differences between internal vs public views are TBD. Public results must respect visibility rules.

### 6.5 Public display and agency branding

| ID | Requirement | Priority / decision |
|---|---|---|
| WEB-001 | Provide an agency-specific public property presentation within the platform. | MVP / Confirmed |
| WEB-002 | Display listing results in card/grid view. | MVP / Confirmed |
| WEB-003 | Provide a property details page. | MVP / Confirmed |
| WEB-004 | Provide a property image gallery. | MVP / Confirmed |
| WEB-005 | Support basic agency-level website branding customisation. | MVP / Confirmed direction |
| WEB-006 | Buyers/investors can browse without registering or logging in. | MVP / Provisional |

Candidate branding controls include agency logo, name, contact details, and brand colours; the exact configurable properties are to be verified. Agency-specific public URL structure is TBD.

### 6.6 Change history

| ID | Requirement | Priority / decision |
|---|---|---|
| HIST-001 | Record field-level changes to property listings. | MVP / Confirmed |
| HIST-002 | Show old and new values for changed fields. | MVP / Confirmed |
| HIST-003 | Record and show change timestamps. | MVP / Confirmed |
| HIST-004 | Allow agents to inspect a listing's change history. | MVP / Confirmed |
| HIST-005 | Attribute changes to an actor/source such as an agent or PDF import. | Proposed / To confirm |
| HIST-006 | Restore an earlier listing version. | Future / TBD |

History retention, display of large media changes, and handling of deletions are TBD.

## 7. Preliminary Property Data Fields

The following are **candidate fields**, not a final schema or validation contract:

| Group | Fields |
|---|---|
| Basic | Property Address, Property Type, Price, Listing Status, Description |
| Physical specifications | Bedrooms, Bathrooms, Parking Spaces, Land Size, Floor Area |
| Project context | Project / Development Name, Developer, Unit / Lot Number, Completion Date |
| Source | Listing Provider |
| Media | Images |

**Additional platform-managed attributes** will likely be needed for agency association, internal identity, visibility, and audit data; their technical representations are left for system design.

Open questions include required vs optional fields, field differences by property type, provider-specific attributes, currency/unit conventions, and whether project-level information is shared across multiple property units.

## 8. Business Rules (Confirmed vs Unresolved)

### Confirmed

- Agency data is isolated; agents within the same agency share management access.
- An agent belongs to only one agency in the MVP.
- Import changes are reviewed and explicitly confirmed before application.
- The import review supports corrections, skipped items, and removal from the pending batch.
- Listing status and public visibility are separate concepts.
- For existing listings, import review starts with the previously stored visibility setting; an agent may change it.
- For new listings, an agent chooses visibility during import review.
- Public browsing only includes publicly visible listings.

### Unresolved

- Exact existing-listing matching and duplicate identification logic.
- Required property fields, parsing error classes, and validation rules.
- Whether missing listings in a later provider document should trigger any lifecycle action.
- Deletion mechanism, recovery, and repeat-import treatment of deleted listings.
- Default visibility selection for new listings when the review loads.
- Detailed publishing semantics outside the simple Public/Hidden setting.
- Account deactivation, agency shutdown, and historical data retention.
- Agency page URL strategy and exact branding configuration.

## 9. Non-functional Requirements (Topics Requiring Specification)

No detailed non-functional requirements have been agreed yet. The following should be defined before implementation sign-off:

- **Security and tenancy:** Enforce agency data separation for all authenticated operations and prevent disclosure of Hidden listings through public endpoints.
- **Access control:** Authentication, session management, admin access, invitation security, and account deactivation behaviour.
- **Reliability and data integrity:** Consistent import results, avoidance of unintended duplicate writes, handling partial failures, and safe bulk operations.
- **Auditability:** Trustworthy timestamps and records of property changes and import outcomes.
- **File handling:** PDF restrictions, malware checks, supported provider formats, file retention, and image processing limits.
- **Performance:** Acceptable import processing times and search responsiveness (targets TBD).
- **Privacy and compliance:** Review applicable Australian privacy and record-retention obligations before production launch.
- **Accessibility and responsiveness:** Public pages and internal dashboard requirements to be specified.

These are **design considerations, not yet approved acceptance thresholds or implementation decisions**.

## 10. Illustrative Acceptance Criteria

These scenarios are proposed as a starting point for detailed stories and QA review, not complete sign-off criteria.

### AC-IMP-01 — Approval before commit

**Given** an agent uploads a PDF that yields new and changed listings, **when** the extraction and comparison complete, **then** the agent can review the proposed changes and no listing updates are committed until the agent confirms.

### AC-IMP-02 — Review correction

**Given** a parsed field is incorrect, **when** the agent corrects it in review and confirms, **then** the approved corrected value is saved.

### AC-IMP-03 — Remove from review

**Given** a proposed import entry is removed from the current review, **when** the remaining entries are confirmed, **then** the removed entry is not applied and any previously saved listing remains unchanged.

### AC-VIS-01 — Preserve existing visibility

**Given** an existing listing is Hidden, **when** a PDF proposes changes to its price, **then** the review displays Hidden as its existing visibility and it remains Hidden unless the agent changes it before confirmation.

### AC-ORG-01 — Agency isolation

**Given** an agent belongs to Agency A, **when** they browse or access internal listing-management operations, **then** they cannot access Agency B's private listing records.

### AC-WEB-01 — Public visibility

**Given** an agency has one Public and one Hidden listing, **when** a buyer browses its public listing page, **then** the Public listing appears and the Hidden listing does not appear in browsing results.

### AC-HIST-01 — Field-level changes

**Given** an agent changes a listing price, **when** they view its change history, **then** the earlier and updated prices and the change timestamp are visible.

## 11. Open Questions and Decisions Register

| ID | Question / decision | Priority for next review |
|---|---|---|
| OQ-01 | How are existing properties matched across provider PDF formats? | High |
| OQ-02 | What fields are mandatory for a valid property, and what should happen when they are missing? | High |
| OQ-03 | What does an absent property mean in a newly uploaded PDF, and should the system allow configurable automatic actions? | High |
| OQ-04 | What form of property deletion is required (hard delete, soft delete, archive), and is recovery needed? | High |
| OQ-05 | What is the default new-listing Public/Hidden selection in Import Review? | Medium |
| OQ-06 | Which agency branding fields are actually configurable? | Medium |
| OQ-07 | How is each agency's public page addressed (path, subdomain, custom domain)? | Medium |
| OQ-08 | What are the exact listing publication rules, if any, beyond Public/Hidden? | Medium |
| OQ-09 | Which property fields vary by property type or provider? | High |
| OQ-10 | Which search fields and sort orders are supported internally vs publicly? | Medium |
| OQ-11 | How are failed partial writes, retries, and cancelled imports handled? | High |
| OQ-12 | Must field history attribute each change to a user/import source, and how long is it retained? | Medium |
| OQ-13 | How are invitation links, credentials, and inactive accounts handled? | High |
| OQ-14 | Is manual property creation required on day one or in a later increment? | High |
| OQ-15 | Is a standalone admin UI required, or can internal operations use a controlled process initially? | Medium |
| OQ-16 | Is buyer access fully public at launch, and should hidden listings return no data when directly requested? | High |

## 12. Suggested Next Steps

1. Review this draft with agency stakeholders and validate the problem statement and priority of each module.
2. Resolve high-priority business rules, especially import matching, validation, deletion, and data visibility.
3. Decide the true launch scope for manual entry and the administrator interface.
4. Convert approved features into user journeys, screen-level specifications, and testable acceptance criteria.
5. Create a separate **Technical Design Document** covering architecture, data model, API contracts, import pipeline, authentication, tenant isolation, and deployment once the functional requirements are stable.

---

**Revision note:** This draft incorporates the corrected decision that websites belong to **agencies, not individual agents**, and that **integration with existing agency websites is deferred**. It also replaces the earlier assumption of immediate PDF import saving with the subsequently confirmed **review-and-confirm workflow**.
