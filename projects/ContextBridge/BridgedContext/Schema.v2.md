# Claude ASDF State Management
Standard Operating Procedure (SOP)

## 1. Overview
This document provides instructions for saving and loading Claude's ASDF state using a standardized schema. The process involves copying state information between a repository and Claude prompts.

## 2. Schema Structure
The following schema must be preserved exactly when copying between repository and prompts:

```json
{
  "session_metadata": {
    "project": "project_name",
    "timestamp": "ISO-8601",
    "version": "1.0",
    "purpose": "development_focus"
  },
  "communication_preferences": {
    "artifacts": {
      "enabled": "boolean",
      "preferred_types": ["code", "diagram", "document"],
      "auto_generate": "boolean",
      "retention_period": "duration_in_days"
    },
    "interaction_style": {
      "verbosity_level": "enum: concise|detailed|technical",
      "code_examples": "boolean",
      "step_by_step_explanations": "boolean",
      "include_diagrams": "boolean"
    },
    "notifications": {
      "enabled": "boolean",
      "channels": ["email", "chat", "system"],
      "frequency": "enum: real_time|digest|custom",
      "priority_threshold": "enum: all|high|critical"
    },
    "documentation": {
      "auto_generate": "boolean",
      "format": "enum: markdown|html|pdf",
      "include_examples": "boolean",
      "versioning": "boolean"
    },
    "feedback": {
      "request_feedback": "boolean",
      "feedback_format": "enum: structured|free_form",
      "anonymous": "boolean"
    }
  },
  "context_chain": [
    {
      "session_id": "unique_identifier",
      "topic": "specific_focus",
      "achievements": [
        "documented_outcome_1",
        "documented_outcome_2"
      ],
      "resources_created": [
        {
          "type": "document_type",
          "path": "file_location",
          "purpose": "resource_purpose"
        }
      ],
      "metrics": {
        "efficiency": "float",
        "impact": "float",
        "completion": "float"
      }
    }
  ],
  "current_state": {
    "active_development": "current_focus",
    "pending_tasks": [
      "task_1",
      "task_2"
    ],
    "resources_needed": [
      "resource_1",
      "resource_2"
    ]
  },
  "impact_metrics": {
    "efficiency_gained": "float",
    "resources_optimized": "float",
    "implementation_speed": "float"
  }
}
```

## 3. Usage Instructions

### Saving State to Repository
1. Copy the entire state from Claude:
   - Type "Export current ASDF state" in the prompt
   - Claude will output the current state in JSON format
   - Copy the entire JSON output, including curly braces

2. Save to Repository:
   - Create a new file named `[project_name]_state_[timestamp].json`
   - Paste the copied JSON, maintaining exact formatting
   - Commit with message: "Save ASDF state: [brief description]"

### Loading State into Claude
1. Prepare the Load Command:
   ```
   Load ASDF state:
   [paste entire JSON here]
   ```

2. Copy from Repository:
   - Open the desired state file
   - Copy the entire JSON content
   - Paste into the load command template above

3. Execute the Load:
   - Send the complete command to Claude
   - Claude will confirm successful state loading
   - Verify loaded state by requesting current status

### Quick Reference

Save State:
```
Human: Export current ASDF state
Claude: [outputs current state in JSON]
```

Load State:
```
Human: Load ASDF state:
[JSON state from repository]
```

Verify State:
```
Human: Verify current ASDF state
Claude: [confirms loaded configuration]
```

Remember:
- Always copy the entire JSON structure
- Maintain exact formatting when copying
- Verify state after loading
- Use consistent file naming in repository
- Include timestamps in filenames
