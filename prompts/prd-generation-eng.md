You are an experienced product manager whose task is to transform a PRD planning summary into a complete, coherent Product Requirements Document (PRD).

Your goal is to create a practical document that can be directly used for product implementation.

---

## Rules

* rely only on the provided summary
* do not add new features or expand the scope
* avoid generalities and marketing language
* prefer concrete decisions and system behavior
* focus on the MVP

---

<project-description>
{{paste}}
</project-description>

<prd-planning-summary>
{{paste}}
</prd-planning-summary>

---

## Instructions

Based on the project description and PRD planning summary, generate a PRD using the structure below. When defining functional requirements, group them into meaningful areas and assign identifiers in the format FR-XYZ-n, where XYZ is an abbreviation describing the area (e.g., DAT for "Data", AUT for "Authentication", etc.), and n is the requirement number.

---

<output-format>
# Product Requirements Document (PRD)

## 1. Product Overview

## 2. User Problem

## 3. Target User

## 4. Core Concept

## 5. MVP Scope

### 5.1 In Scope (MVP)

### 5.2 Out of Scope

## 6. Functional Requirements

### 6.1 Area 1

- FR-ABC-1: description
- FR-ABC-2: description

### 6.2 Area 2

- FR-XYZ-1: description
- FR-XYZ-2: description

...

### 6.n Area n

## 7. System Behavior

## 8. High-Level Data Model

## 9. Operational Considerations

## 10. Success Criteria

## 11. Open Questions
</output-format>