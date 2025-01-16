# GitHub CDSF Extension

## Architecture Overview

```
Extension/
├── manifest.json
├── src/
│   ├── background/
│   │   └── stateManager.js      # CDSF state handling
│   ├── content/
│   │   ├── githubInjector.js    # GitHub UI integration
│   │   └── claudeConnector.js   # Claude communication
│   ├── storage/
│   │   ├── artifactHandler.js   # Artifact conversion/storage
│   │   └── gistManager.js       # GitHub Gist integration
│   └── utils/
│       ├── cdsfParser.js        # CDSF encoding/decoding
│       └── formatters.js        # Content formatting
└── components/
    ├── StateViewer.jsx          # State visualization
    └── ArtifactManager.jsx      # Artifact management UI
```

## Core Features

### 1. CDSF State Management
```javascript
// stateManager.js
class CDSFStateManager {
  async saveState(claudeState) {
    const cdsf = this.encodeCDSF(claudeState);
    const gistId = await this.gistManager.createGist(cdsf);
    return this.storeStateReference(gistId);
  }

  async loadState(gistId) {
    const cdsf = await this.gistManager.getGist(gistId);
    return this.decodeCDSF(cdsf);
  }
}
```

### 2. Artifact Storage System
```javascript
// artifactHandler.js
class ArtifactHandler {
  async storeArtifact(artifact) {
    const cdsfArtifact = this.convertToCDSF(artifact);
    const repo = await this.getTargetRepo();
    return this.createGithubFile(repo, cdsfArtifact);
  }

  convertToCDSF(artifact) {
    return `<<CDSF/ARTIFACT>>
    λ:1.0~τ:${Date.now()}
    ∆{
      type:${artifact.type}~
      content:${this.compressContent(artifact.content)}
    }
    <<CDSF/ARTIFACT>>`;
  }
}
```

### 3. GitHub Integration

#### Manifest Configuration
```json
{
  "manifest_version": 3,
  "name": "GitHub CDSF Manager",
  "permissions": [
    "storage",
    "tabs",
    "https://github.com/*",
    "https://api.github.com/*"
  ],
  "content_scripts": [
    {
      "matches": ["https://github.com/*"],
      "js": ["content/githubInjector.js"]
    }
  ]
}
```

#### GitHub UI Integration
```javascript
// githubInjector.js
class GitHubUIManager {
  injectStateControls() {
    const controls = document.createElement('div');
    controls.innerHTML = `
      <div class="cdsf-controls">
        <button onclick="saveState()">Save CDSF State</button>
        <button onclick="loadState()">Load CDSF State</button>
        <button onclick="manageArtifacts()">Manage Artifacts</button>
      </div>
    `;
    document.querySelector('.repository-content').prepend(controls);
  }
}
```

### 4. Artifact Management

#### Storage Format
```javascript
// Artifact CDSF Format
<<CDSF/ARTIFACT>>
λ:1.0~τ:1705395200
∆{
  meta{
    type:code~lang:javascript,
    purpose:implementation,
    context:project_reference
  },
  content<
    compressed:true,
    format:base64,
    data:${compressedContent}
  >
}
<<CDSF/ARTIFACT>>
```

#### Repository Structure
```
repo/
├── .cdsf/
│   ├── states/
│   │   └── state_${timestamp}.cdsf
│   ├── artifacts/
│   │   ├── code/
│   │   ├── documents/
│   │   └── diagrams/
│   └── index.cdsf       # State/artifact registry
```

## Usage Examples

### 1. Save Current Claude State
```javascript
// In Claude conversation
async function saveToCDSF() {
  const state = await claude.getCurrentState();
  const cdsfState = CDSFStateManager.encode(state);
  await GitHubManager.saveToRepo(cdsfState);
}
```

### 2. Store Artifact
```javascript
// When Claude generates content
async function storeArtifact(artifact) {
  const cdsfArtifact = ArtifactHandler.convertToCDSF(artifact);
  const path = `artifacts/${artifact.type}/${generateFileName()}`;
  await GitHubManager.createFile(path, cdsfArtifact);
}
```

### 3. Load State into Claude
```javascript
async function loadState(stateId) {
  const cdsfState = await GitHubManager.getState(stateId);
  const decodedState = CDSFStateManager.decode(cdsfState);
  await claude.loadState(decodedState);
}
```

## Implementation Guide

1. Extension Setup:
   - Install through Chrome Web Store
   - Authenticate with GitHub
   - Configure target repository

2. State Management:
   - Use CDSF format for all state storage
   - Maintain state history in .cdsf directory
   - Index states for quick access

3. Artifact Handling:
   - Convert artifacts to CDSF format
   - Store in appropriate .cdsf subdirectory
   - Maintain references in state objects

4. Security Considerations:
   - Encrypt sensitive content
   - Use GitHub tokens for authentication
   - Validate CDSF format before storage/loading

## Extension Settings

```json
{
  "github": {
    "defaultRepo": "username/repo",
    "statePath": ".cdsf/states",
    "artifactPath": ".cdsf/artifacts"
  },
  "cdsf": {
    "compression": {
      "enabled": true,
      "level": "high"
    },
    "validation": {
      "strict": true,
      "recovery": true
    }
  }
}
```
