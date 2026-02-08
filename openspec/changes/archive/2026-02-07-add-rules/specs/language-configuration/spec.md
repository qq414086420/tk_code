## ADDED Requirements

### Requirement: Language configuration storage
The system SHALL support storing language preferences in configuration files using ISO 639-1 language codes.

#### Scenario: Store language preference in user-level config
- **WHEN** user sets language preference in ~/.openspec/config.yaml
- **THEN** system stores language code (e.g., "zh") under `language:` field

#### Scenario: Store language preference in project-level config
- **WHEN** user sets language preference in openspec/config.yaml
- **THEN** system stores language code under `language:` field

#### Scenario: Validate language code format
- **WHEN** configuration file contains invalid language code
- **THEN** system rejects configuration and displays error message

### Requirement: Configuration hierarchy and precedence
The system SHALL support multiple configuration sources with defined precedence order.

#### Scenario: Environment variable overrides project config
- **WHEN** OPENSPEC_LANGUAGE environment variable is set
- **THEN** system uses environment variable value regardless of project or user config

#### Scenario: Project config overrides user config
- **WHEN** language is set in both project-level and user-level configs without environment variable
- **THEN** system uses project-level configuration

#### Scenario: User config used when no project config exists
- **WHEN** only user-level config contains language preference
- **THEN** system uses user-level configuration

#### Scenario: Default to English when no config exists
- **WHEN** no language preference is configured at any level
- **THEN** system defaults to "en" (English)

### Requirement: Read language configuration
The system SHALL read language preference from the highest-precedence configuration source before executing language-dependent operations.

#### Scenario: CLI command reads language preference
- **WHEN** user executes any OpenSpec CLI command
- **THEN** system loads language preference from applicable configuration sources

#### Scenario: Language preference available to workflow
- **WHEN** OpenSpec workflow (e.g., new-change, continue-change) is executed
- **THEN** workflow receives language preference as context parameter

### Requirement: Apply language preference to LLM interactions
The system SHALL inject language preference instructions into LLM prompts for supported workflows.

#### Scenario: Inject language instruction into Chinese prompt
- **WHEN** language preference is "zh" and workflow supports Chinese
- **THEN** system includes "Respond in Chinese" instruction in LLM system prompt

#### Scenario: Use language-specific templates
- **WHEN** language-specific template exists for configured language
- **THEN** system loads template from openspec/locales/<lang>/ directory

#### Scenario: Fallback to English template
- **WHEN** no template exists for configured language
- **THEN** system uses English template from default location

### Requirement: Support for multiple languages in configuration
The system SHALL allow configuration structure to support future multi-language scenarios.

#### Scenario: Store single language preference
- **WHEN** configuration specifies single language
- **THEN** system stores as `language: "zh"` in config file

#### Scenario: Validate against supported language list
- **WHEN** configuration contains unsupported language code
- **THEN** system displays warning and falls back to English
