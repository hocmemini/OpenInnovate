# ASDF Chrome Extension Integration
## RFC for Two-Way State Management

### 1. Enhanced Manifest
```json
{
    "manifest_version": 3,
    "name": "ASDF State Manager",
    "version": "1.0",
    "description": "Two-way state management for AI conversations using ASDF",
    "permissions": [
        "activeTab",
        "storage",
        "scripting"
    ],
    "host_permissions": [
        "https://claude.ai/*"
    ],
    "action": {
        "default_popup": "popup.html"
    },
    "background": {
        "service_worker": "background.js"
    },
    "content_scripts": [{
        "matches": ["https://claude.ai/*"],
        "js": ["content.js", "asdf.js"]
    }]
}
```

### 2. ASDF State Manager
```javascript
// asdf.js
class ASFDStateManager {
    constructor() {
        this.state = {
            session_info: {
                id: null,
                timestamp: null,
                purpose: null
            },
            context: {
                current_state: null,
                key_metrics: {},
                resources: []
            },
            developments: [],
            next_steps: []
        };
    }

    extractState() {
        const messages = document.querySelectorAll('.message-content');
        const developments = [];
        
        messages.forEach((message, index) => {
            const role = message.closest('.message')
                .classList.contains('user-message') ? 'Human' : 'Assistant';
            
            developments.push({
                type: 'conversation',
                role: role,
                content: message.textContent,
                metrics: {
                    order: index,
                    timestamp: new Date().toISOString()
                }
            });
        });

        return {
            session_info: {
                id: `session_${new Date().getTime()}`,
                timestamp: new Date().toISOString(),
                purpose: document.title || 'AI Conversation'
            },
            context: {
                current_state: 'conversation',
                key_metrics: {
                    messages: developments.length,
                    last_update: new Date().toISOString()
                },
                resources: [window.location.href]
            },
            developments: developments,
            next_steps: []
        };
    }

    async saveToGitHub(state) {
        const config = await new Promise(resolve => {
            chrome.storage.sync.get([
                'githubToken',
                'repoOwner',
                'repoName',
                'branchName'
            ], resolve);
        });

        if (!config.githubToken || !config.repoOwner || !config.repoName) {
            throw new Error('GitHub configuration missing');
        }

        const filename = `states/${state.session_info.id}.json`;
        const content = btoa(JSON.stringify(state, null, 2));

        const response = await fetch(
            `https://api.github.com/repos/${config.repoOwner}/${config.repoName}/contents/${filename}`,
            {
                method: 'PUT',
                headers: {
                    'Authorization': `token ${config.githubToken}`,
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                    message: 'Update ASDF state',
                    content: content,
                    branch: config.branchName || 'main'
                })
            }
        );

        if (!response.ok) {
            throw new Error(`GitHub API error: ${response.statusText}`);
        }

        return filename;
    }

    async loadFromGitHub(stateId) {
        const config = await new Promise(resolve => {
            chrome.storage.sync.get([
                'githubToken',
                'repoOwner',
                'repoName',
                'branchName'
            ], resolve);
        });

        const response = await fetch(
            `https://api.github.com/repos/${config.repoOwner}/${config.repoName}/contents/states/${stateId}.json`,
            {
                headers: {
                    'Authorization': `token ${config.githubToken}`
                }
            }
        );

        if (!response.ok) {
            throw new Error(`GitHub API error: ${response.statusText}`);
        }

        const data = await response.json();
        const content = JSON.parse(atob(data.content));
        return content;
    }

    applyState(state) {
        // Implementation specific to UI framework
        console.log('Applying state:', state);
        // Update UI based on state
    }
}
```

### 3. Enhanced Content Script
```javascript
// content.js
const stateManager = new ASFDStateManager();

// Listen for sync request
chrome.runtime.onMessage.addListener(async (request, sender, sendResponse) => {
    if (request.action === 'syncState') {
        try {
            const state = stateManager.extractState();
            const filename = await stateManager.saveToGitHub(state);
            console.log('State saved:', filename);
            sendResponse({success: true, filename});
        } catch (error) {
            console.error('Error syncing state:', error);
            sendResponse({success: false, error: error.message});
        }
        return true;
    }
    
    if (request.action === 'loadState') {
        try {
            const state = await stateManager.loadFromGitHub(request.stateId);
            stateManager.applyState(state);
            sendResponse({success: true});
        } catch (error) {
            console.error('Error loading state:', error);
            sendResponse({success: false, error: error.message});
        }
        return true;
    }
});
```

### 4. Enhanced Popup Interface
```html
<!-- popup.html -->
<!DOCTYPE html>
<html>
<head>
    <title>ASDF State Manager</title>
    <style>
        body { width: 300px; padding: 10px; }
        input, button { margin: 5px 0; width: 100%; }
        .tab-content { display: none; }
        .tab-content.active { display: block; }
    </style>
</head>
<body>
    <div class="tabs">
        <button data-tab="config">Configuration</button>
        <button data-tab="state">State Management</button>
    </div>

    <div id="config" class="tab-content">
        <h3>GitHub Configuration</h3>
        <input type="text" id="githubToken" placeholder="GitHub Token">
        <input type="text" id="repoOwner" placeholder="Repo Owner">
        <input type="text" id="repoName" placeholder="Repo Name">
        <input type="text" id="branchName" placeholder="Branch (default: main)">
        <button id="saveConfig">Save Configuration</button>
    </div>

    <div id="state" class="tab-content">
        <h3>State Management</h3>
        <button id="saveState">Save Current State</button>
        <input type="text" id="stateId" placeholder="State ID to load">
        <button id="loadState">Load State</button>
    </div>

    <div id="status"></div>
    <script src="popup.js"></script>
</body>
</html>
```

### 5. Usage

To use this enhanced integration:

1. Setup:
```bash
# Create extension directory
mkdir asdf-state-manager
cd asdf-state-manager

# Create necessary files
touch manifest.json popup.html popup.js content.js asdf.js background.js

# Copy respective code to each file
```

2. Configuration:
- Install as Chrome extension
- Configure GitHub credentials
- Select repository for state storage

3. State Management:
- Save state: Click "Save Current State"
- Load state: Enter State ID and click "Load State"
- States stored in `/states` directory of repository

### 6. Security Considerations

1. Token Storage:
- Use Chrome's secure storage
- Never expose in states
- Validate permissions

2. State Validation:
- Verify structure
- Sanitize content
- Validate metrics

3. Error Handling:
- Graceful degradation
- User feedback
- Logging system

### 7. Future Enhancements

1. Planned Features:
- State diffing
- Merge capabilities
- Conflict resolution
- Batch operations
- Real-time sync

2. Optimization:
- Compression
- Deduplication
- Lazy loading
- Cache management

### Contact

This integration specification maintains RFC compliance while enabling two-way state management. For questions or improvements, use the GitHub repository's issue system.
