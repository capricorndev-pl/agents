---
name: feature-spec
description: Create a feature spec from the PRD and feature map. Use when spinning up a new feature specification for implementation.
allowed-tools: Read, Write, Glob
---

# Feature Spec Generator

You are creating a feature specification for this project. Always adhere to any rules or requirements set out in CLAUDE.md when responding.

## Step 1. Locate project documents

Look for the following files:
- PRD: `docs/prd.md`
- Feature map: `docs/feature-map.md`

If either file is missing, inform the user and ask how to proceed. Do not guess or fabricate content.

## Step 2. Understand the feature

Ask the user which feature they want to spec. If the feature matches an entry in the feature map, use that entry's scope and dependencies as a starting point. If the feature is not in the feature map, proceed using the PRD as the primary source — use the feature map for context about existing features and conventions, but note to the user that this feature is not part of the current map and confirm before proceeding.

## Step 3. Clarify before drafting

Review the PRD and feature map in the context of the requested feature. If you encounter ambiguities, unclear boundaries, or missing information — ask before assuming. Asking too many clarifying questions is always better than making overconfident assumptions.

If no questions arise, proceed directly to drafting.

## Step 4. Draft the spec

Create a markdown spec file using the structure defined below. Save it to `docs/features/<feature-slug>.md` where `feature-slug` is a kebab-case version of the feature name.

### Technical depth

Include technical context where it directly affects scope or behavior — data model sketches, access control rules, integration contracts, key constraints. Do not include implementation-level details such as code examples, specific function signatures, file paths, or framework-specific patterns.

### Spec structure

Use the following sections. Include every section. If a section does not apply, include it with a brief note explaining why.

#### Summary
What this feature is and does in 2–3 sentences.

#### Motivation
Why this feature is needed, what problem it solves, and how it connects to the broader product goals as stated in the PRD.

#### User Stories
Who does what and why, in the standard "As a [role], I want to [action] so that [benefit]" format.

#### Detailed Behaviour
Step-by-step description of how the feature works from the user's perspective, broken into logical subsections as needed. This is the core of the spec — be thorough.

#### Data Model
New or modified entities, fields, relationships, and constraints. High-level, not implementation-specific. If this feature relies on entities defined in another feature spec, reference that spec rather than redefining.

#### Access Control
Who can do what. Role-based permissions, visibility rules, and any data-level access constraints.

#### Integration Points
How this feature interacts with the outside world and other features: routes or pages it exposes, APIs it provides or consumes, events it emits or listens to, dependencies on other features or external services. If purely internal with no external interfaces, state that explicitly.

#### Testing Strategy
Non-obvious testing considerations specific to this feature: complex business logic, edge cases requiring dedicated test scenarios, integration points that need contract testing. If standard unit and integration testing is sufficient, state that briefly.

#### Constraints & Edge Cases
Known limitations, boundary conditions, error states, and how they should be handled.

#### Out of Scope
What this feature explicitly does not cover, especially things that might reasonably be assumed to be included but are handled by other features or deferred.

## Step 5. Final output

After the file is saved, respond with a short summary: