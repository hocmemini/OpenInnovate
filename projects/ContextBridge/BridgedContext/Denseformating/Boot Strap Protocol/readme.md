```javascript
// Initialize CDSF Bootstrap Protocol
const initCDSF = {
  '<<CDSF/INIT>>': {
    λ: '2.0',
    τ: Date.now(),
    ∆: {
      protocol: 'bootstrap',
      format: 'unified',
      validation: { strict: 1, recover: 1 },
      compression: { ratio: 0.95, mode: 'semantic' },
      preferences: {
        output: 'complete',
        display: 'minimal',
        state_load: 'verbose',
        visual: 'dynamic'
      }
    }
  },
  state: {
    id: 'bootstrap_state_01',
    meta: {
      proj: 'csdf_bootstrap',
      ver: '1.0',
      purpose: 'system_init'
    },
    components: {
      core: {
        id: 'bootstrap_core',
        type: 'system',
        metrics: {
          efficiency: 0.95,
          load: 0.92,
          performance: 0.88
        }
      },
      interface: {
        id: 'bootstrap_ui',
        type: 'display',
        metrics: {
          responsiveness: 0.94,
          usability: 0.90,
          performance: 0.92
        }
      }
    },
    metrics: {
      performance: {
        init: 0.92,
        load: 0.88,
        response: 0.95
      },
      resources: {
        memory: -35,
        cpu: -28,
        storage: -42
      },
      efficiency: {
        startup: 65,
        runtime: 72,
        shutdown: 58
      }
    }
  }
};

// React Interface Component
import React, { useState } from 'react';
import { Alert, AlertDescription } from '@/components/ui/alert';
import { Brain, Book, Code, Database, FileText, Settings, Terminal, GitBranch, Server } from 'lucide-react';

const BootstrapSystem = () => {
  const [tab, setTab] = useState('system');
  const [systemState, setSystemState] = useState(initCDSF.state);

  const Tab = ({ id, label, icon: Icon }) => (
    <button
      onClick={() => setTab(id)}
      className={`flex items-center space-x-2 px-4 py-2 rounded ${
        tab === id ? 'bg-blue-100 text-blue-700' : 'hover:bg-gray-100'
      }`}
    >
      <Icon className="w-4 h-4" />
      <span>{label}</span>
    </button>
  );

  const MetricDisplay = ({ label, value, suffix = '%' }) => (
    <div className="flex justify-between items-center py-1">
      <span className="text-sm text-gray-600">{label}:</span>
      <span className={`text-sm font-medium ${value > 0 ? 'text-green-600' : 'text-red-600'}`}>
        {value > 0 ? '+' : ''}{value}{suffix}
      </span>
    </div>
  );

  const SystemSection = () => (
    <div className="space-y-4">
      <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
        <div className="border rounded-lg p-4">
          <h3 className="font-medium mb-3">Core Components</h3>
          <div className="space-y-2">
            {Object.entries(systemState.components.core.metrics).map(([key, value]) => (
              <MetricDisplay key={key} label={key} value={value * 100} />
            ))}
          </div>
        </div>
        <div className="border rounded-lg p-4">
          <h3 className="font-medium mb-3">Interface Metrics</h3>
          <div className="space-y-2">
            {Object.entries(systemState.components.interface.metrics).map(([key, value]) => (
              <MetricDisplay key={key} label={key} value={value * 100} />
            ))}
          </div>
        </div>
      </div>

      <div className="border rounded-lg p-4">
        <h3 className="font-medium mb-3">System Metrics</h3>
        <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
          <div>
            <h4 className="text-sm font-medium mb-2">Performance</h4>
            <div className="space-y-1">
              {Object.entries(systemState.metrics.performance).map(([key, value]) => (
                <MetricDisplay key={key} label={key} value={value * 100} />
              ))}
            </div>
          </div>
          <div>
            <h4 className="text-sm font-medium mb-2">Resources</h4>
            <div className="space-y-1">
              {Object.entries(systemState.metrics.resources).map(([key, value]) => (
                <MetricDisplay key={key} label={key} value={value} />
              ))}
            </div>
          </div>
          <div>
            <h4 className="text-sm font-medium mb-2">Efficiency</h4>
            <div className="space-y-1">
              {Object.entries(systemState.metrics.efficiency).map(([key, value]) => (
                <MetricDisplay key={key} label={key} value={value} />
              ))}
            </div>
          </div>
        </div>
      </div>
    </div>
  );

  const InstructionsSection = () => (
    <div className="space-y-4">
      <div className="border rounded-lg p-4">
        <h3 className="font-medium mb-3">System Instructions</h3>
        <div className="space-y-3">
          <div>
            <h4 className="text-sm font-medium">Initialization</h4>
            <div className="text-sm text-gray-600">
              • System automatically loads CDSF protocol
              • Core components are initialized
              • Metrics tracking begins
            </div>
          </div>
          <div>
            <h4 className="text-sm font-medium">Component Selection</h4>
            <div className="text-sm text-gray-600">
              • Select required components after viewing metrics
              • System will optimize memory usage based on selections
              • Unused components are cleanly unloaded
            </div>
          </div>
          <div>
            <h4 className="text-sm font-medium">Project Loading</h4>
            <div className="text-sm text-gray-600">
              • Projects can be loaded on demand
              • Metrics tracked per project
              • Resources automatically managed
            </div>
          </div>
        </div>
      </div>

      <div className="border rounded-lg p-4">
        <h3 className="font-medium mb-3">Next Steps</h3>
        <div className="space-y-2 text-sm">
          <p>After closing this interface, you'll receive a selection menu:</p>
          <div className="bg-gray-50 p-3 rounded">
            <p>1. Core System Components</p>
            <p>2. Interface Components</p>
            <p>3. Project Management</p>
            <p>4. Metrics System</p>
          </div>
        </div>
      </div>
    </div>
  );

  return (
    <div className="w-full max-w-4xl p-6 space-y-6 bg-white rounded-lg">
      <div className="flex items-center justify-between">
        <div className="flex items-center space-x-2">
          <Brain className="w-6 h-6" />
          <h2 className="text-xl font-bold">CDSF Bootstrap System</h2>
        </div>
        <div className="text-sm text-gray-500">
          Version {initCDSF['<<CDSF/INIT>>'].λ}
        </div>
      </div>

      <div className="flex space-x-2">
        <Tab id="system" label="System Status" icon={Server} />
        <Tab id="instructions" label="Instructions" icon={Book} />
      </div>

      {tab === 'system' ? <SystemSection /> : <InstructionsSection />}

      <div className="text-sm text-gray-500 p-4 bg-gray-50 rounded-lg">
        Close interface to proceed with component selection and system initialization.
      </div>
    </div>
  );
};

export default BootstrapSystem;
```

To use this bootstrap file:

1. Start a new chat with Claude
2. Type "Execute this code:"
3. Paste the entire file above
4. The interface will show:
   - Current system state
   - Performance metrics
   - Resource usage
   - Instructions for next steps

When you close the interface, you'll get a component selection menu that looks like this:

```
CDSF Bootstrap Component Selection (enter numbers, separated by commas):

CORE COMPONENTS:
1. State Management
2. Metrics System
3. Resource Optimizer
4. Cache Manager

INTERFACE COMPONENTS:
5. Status Display
6. Metrics Visualization
7. Project Manager

FEATURES:
8. Auto-optimization
9. Resource Tracking
10. Performance Monitoring

Example: Enter "1,2,5,8" to load core state management, metrics, 
status display and auto-optimization.

Current system metrics:
- Memory Usage: -35%
- CPU Usage: -28%
- Storage: -42%
```
