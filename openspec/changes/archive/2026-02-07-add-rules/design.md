## Context

OpenSpec currently operates exclusively in English, which creates accessibility barriers for Chinese-speaking developers. The system lacks a configuration layer to store and apply language preferences. Adding language support requires:
- A configuration system to persist language preferences
- Language detection and translation/internationalization (i18n) capabilities
- Integration with CLI and user interaction flows
- Support for future language extensions beyond Chinese

## Goals / Non-Goals

**Goals:**
- Implement a flexible rules/configuration system for language preferences
- Add Chinese language support for OpenSpec conversations
- Enable system to detect and respond in the configured language
- Create extensible architecture for future language additions

**Non-Goals:**
- Full translation of OpenSpec codebase
- Automatic language detection without user configuration
- Support for all languages (starting with Chinese only)
- Breaking changes to existing English workflows

## Decisions

**Configuration Storage Format:**
- Use YAML for configuration files (consistent with existing .openspec.yaml)
- Store language preferences in user-level config (~/.openspec/config.yaml or project-level openspec/config.yaml)
- Support environment variable override (OPENSPEC_LANGUAGE)
- Rationale: YAML is already used in the project, provides readability, and allows structured configs. User/project level hierarchy allows flexibility without global changes.

**Language Representation:**
- Use ISO 639-1 language codes (e.g., "en", "zh")
- Store as top-level field `language: "zh"` in config
- Default to "en" if not specified (backward compatible)
- Rationale: Standards-based, concise, and easy to validate. Default ensures no breaking changes.

**Integration Points:**
- CLI commands read config before executing language-dependent operations
- Skill/prompt generation injects language preference into context
- OpenSpec artifacts maintain English in code/references, but descriptions can be language-specific
- Rationale: Keeps system consistent, separates concerns, and allows gradual adoption.

**Chinese Implementation Strategy:**
- Add Chinese instruction templates for key workflows (onboard, new-change, continue-change)
- Language preference flows through to LLM prompts as system instructions
- CLI help messages and error messages remain English initially
- Rationale: Focus on conversational language support (primary user need), not full UI localization. System messages can be phased in later.

**Extensibility:**
- Design config to support multiple languages for future use (e.g., array or object structure)
- Language-specific templates stored in openspec/locales/<lang>/ directory
- Fallback to English if template missing for configured language
- Rationale: Future-proof for additional languages without major refactoring.

## Risks / Trade-offs

[Risk] Inconsistent language mixing if only partial templates translated → Mitigation: Document which workflows support Chinese and which don't; clear default to English for unsupported workflows.

[Trade-off] Configuration adds complexity for users who only need English → Mitigation: Default config is implicit (no file needed), English-only users see no changes.

[Risk] LLM may not reliably respond in requested language → Mitigation: Explicit system instructions; validate language in responses during testing; provide fallback instructions.

[Trade-off] Maintaining multiple language templates creates maintenance overhead → Mitigation: Keep Chinese focused on core workflows; use community contributions for other languages; version templates alongside specs.

[Risk] Language preference may cause confusion in multi-language teams → Mitigation: Document config precedence (env var > project > user > default); recommend team-level config in openspec/config.yaml.
