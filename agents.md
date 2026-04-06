# AGENTS.md
 
Shared project context for all AI agents (Claude Code, Codex, etc.) working on this repository.
 
## Project Overview
 
This is the **CTC** repository for Stitch Fix's STIBO STEP PIM system. It contains ~450 Business Rules (JavaScript), ~2300 XML config files, Jest tests, and deployment tooling.
 
- **Business Rules**: `step-configs/BusinessRule/` (~450 JS files)
- **XML Configs**: `step-configs/` (48 types — Attributes, LOVs, UserTypes, Workflows, etc.)
- **Tests**: `test/BusinessRule/` (unit, integration, validate)
- **Deployment**: `tools/step-deploy/` (CLI + Web UI)
 
 
For BR types, library deps, layer placement rules, and approval orchestration: see `docs/ai-playbook/architecture.md`
 
## Rhino JS Engine (v1.8.0 on Java 21)
 
BR code runs in Mozilla Rhino, NOT Node.js.
 
**Safe ES6**: `let`/`const`, arrow functions, template literals, destructuring, `Map`/`Set`, `for...of`, `Promise`
**NOT supported**: `class`, `async`/`await`, ES modules (`import`/`export`)
**CRITICAL**: Never use `const`/`let` inside loop bodies — Rhino holds the first iteration's value. Use `var` for loop-body variables.
**Java interop**: `java.util.HashMap`, `ArrayList`, `HashSet` always available. Use `.iterator()` with `.hasNext()`/`.next()`.
**Legacy style**: Existing BRs use ES5 (`var`, `function(){}`). When patching legacy BRs, match existing style.
 
## Core Principles
 
- Minimize STEP API calls; ensure bulk-safe execution
 
## Safety Rules (NON-NEGOTIABLE)
 
- NEVER auto-deploy to production
- NEVER modify STEP metadata blocks
- ALWAYS propose approach before code
- ALWAYS manually review diffs
- ALWAYS validate in dev before preprod
- NEVER accept code that doesn't satisfy Acceptance Criteria
