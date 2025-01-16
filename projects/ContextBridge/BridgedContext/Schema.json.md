## Usage Instructions

1. Store session data in this format
2. Reference the format in future prompts
3. AI processes the context
4. Continues development with awareness

'''{
    "session_metadata": {
        "project": "project_name",
        "timestamp": "ISO-8601",
        "version": "1.0",
        "purpose": "development_focus"
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
'''
