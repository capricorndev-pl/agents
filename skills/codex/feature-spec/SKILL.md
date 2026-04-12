---
name: feature-spec
description: Create a feature spec from the PRD and feature map. Use when spinning up a new feature specification for implementation.
---

# Feature Spec

## Overview

Create a detailed feature specification for a single feature using the project PRD and feature map as the source of truth. Keep the spec concrete, implementation-oriented, and limited to the selected feature's scope.

## Workflow

1. Locate the project documents.
2. Identify the target feature.
3. Review the PRD and feature map for ambiguities.
4. Ask clarifying questions if needed.
5. Draft the feature spec and save it under `docs/features/`.

## Locate The Source Documents

Look for:

- PRD: `docs/prd.md`
- Feature map: `docs/feature-map.md`

If either file is missing, stop and tell the user exactly which document is unavailable. Do not invent missing source material.

## Confirm The Feature

Ask which feature should be specified unless the user has already named it.

If the feature matches an entry in the feature map, use that entry's scope and dependencies as a starting point.

If the feature is not in the feature map, proceed using the PRD as the primary source. Use the feature map for context about adjacent features and shared conventions, but tell the user that this feature is not currently represented in the map and confirm before proceeding.

Respect any conventions defined in the feature map. Reference them where relevant, but do not redefine them inside the feature spec.

## Clarify Before Drafting

Review the PRD and feature map in the context of the selected feature.

Ask clarifying questions before drafting if you find:

- unclear scope boundaries
- missing prerequisite decisions
- ambiguous ownership between neighboring features
- missing behavior required to make the spec implementable

Prefer asking over assuming. If no meaningful ambiguities remain, proceed directly to drafting.

## Drafting Rules

Save the spec to `docs/features/<feature-slug>.md` where `<feature-slug>` is the kebab-case form of the feature name.

Base the document strictly on the PRD and feature map:

- do not invent new requirements
- do not expand scope beyond the chosen feature
- do not describe behavior owned by other features except as dependencies or integration points
- keep the document concrete enough to implement against

Include technical context only where it changes scope or behavior, such as:

- high-level data model changes
- access control rules
- integration contracts
- operational constraints

Do not include implementation-level details such as:

- code samples
- file paths outside the destination spec path
- framework-specific patterns
- function signatures

## Required Structure

Include every section below. If a section does not apply, keep it and briefly explain why.

### Summary

What this feature is and does in 2-3 sentences.

### Motivation

Why this feature is needed, what problem it solves, and how it supports the broader product goals from the PRD.

### User Stories

Who does what and why, using the format: `As a [role], I want to [action] so that [benefit]`.

### Detailed Behaviour

The main behavior of the feature from the user's perspective. Break it into logical subsections when useful. This should be the most detailed part of the document.

### Data Model

New or modified entities, fields, relationships, and constraints. Keep this high-level. If the feature relies on entities defined elsewhere, reference the related feature spec instead of redefining them.

### Access Control

Who can do what. Include role-based permissions, visibility rules, and any data-level restrictions.

### Integration Points

How this feature interacts with other features or external systems: pages, routes, APIs, events, services, or dependent features. If there are no external interfaces, say so explicitly.

### Testing Strategy

Testing considerations specific to this feature: critical edge cases, business-rule-heavy scenarios, and integrations that need contract coverage. If normal unit and integration coverage is sufficient, state that briefly.

### Constraints & Edge Cases

Known limits, boundary conditions, error states, and expected handling.

### Out of Scope

What this feature explicitly does not include, especially things that could easily be assumed to belong here but are handled elsewhere or deferred.

## Final Response

After saving the file, respond with a short summary that includes:

- the feature that was specified
- the file path that was created
- whether any assumptions or open questions remain
