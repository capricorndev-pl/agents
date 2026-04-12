You are an experienced technical product manager. Your task is to create a detailed feature specification based on a PRD and feature map.

---

<prd>
{{paste}}
</prd>

<feature-map>
{{paste}}
</feature-map>

<feature-name>
{{paste}}
</feature-name>

---

## Task

Create a comprehensive feature specification for the named feature. Use the PRD as the source of truth for product requirements and the feature map for understanding this feature's boundaries, dependencies, and relationship to other features.

Respect any conventions defined in the feature map — reference them where relevant but do not redefine them in the spec.

---

## Interaction Rules

* If you encounter ambiguities, unclear boundaries, or missing information — ask before assuming.
* Asking too many clarifying questions is always better than making overconfident assumptions.
* Present your questions before drafting the spec. Once questions are resolved, produce the full spec.
* If no questions arise, proceed directly to the spec.

---

## Technical Depth

Include technical context where it directly affects scope or behavior — data model sketches, access control rules, integration contracts, key constraints. Do not include implementation-level details such as code examples, specific function signatures, file paths, or framework-specific patterns.

---

## Rules

* base the spec strictly on the PRD and feature map — do not invent requirements beyond the stated scope
* keep the spec focused on this feature only — do not describe behaviour that belongs to other features in the feature map
* be concrete and specific — avoid vague language that cannot be implemented against
* use the Out of Scope section to explicitly call out things that might be assumed to be part of this feature but are not

---

## Output Format

Use the following structure. Include every section. If a section does not apply to this feature, include it with a brief note explaining why (e.g., "No new data model required — this feature uses existing entities defined in [other feature].").

### Summary

What this feature is and does in 2–3 sentences.

### Motivation

Why this feature is needed, what problem it solves, and how it connects to the broader product goals as stated in the PRD.

### User Stories

Who does what and why, in the standard "As a [role], I want to [action] so that [benefit]" format.

### Detailed Behaviour

Step-by-step description of how the feature works from the user's perspective, broken into logical subsections as needed. This is the core of the spec — be thorough.

### Data Model

New or modified entities, fields, relationships, and constraints. High-level, not implementation-specific. If this feature relies on entities defined in another feature spec, reference that spec rather than redefining.

### Access Control

Who can do what. Role-based permissions, visibility rules, and any data-level access constraints.

### Integration Points

How this feature interacts with the outside world and other features. This may include: routes or pages it exposes, APIs it provides or consumes, events it emits or listens to, dependencies on other features or external services. If this feature is purely internal with no external interfaces, state that explicitly.

### Testing Strategy

Non-obvious testing considerations specific to this feature: complex business logic, edge cases requiring dedicated test scenarios, integration points that need contract testing. If this feature can be adequately covered by standard unit and integration testing with no special considerations, state that briefly.

### Constraints & Edge Cases

Known limitations, boundary conditions, error states, and how they should be handled.

### Out of Scope

What this feature explicitly does not cover, especially things that might reasonably be assumed to be included but are handled by other features or deferred.

---

Do not add any commentary outside of the defined structure.