# ADR-006: State Management Approach

## Status
Accepted

## Date
2025-01-01

## Context
Brand Central AI requires robust state management for complex application data:
- Multi-phase brand development progress (10 phases)
- Rich brand data (audience profiles, world bible, visual identity)
- AI-generated content history and interactions
- User activity tracking and recent actions
- Cross-module data sharing and consistency
- Data persistence across browser sessions

Requirements:
- Manage complex nested state structures
- Share state across multiple React components
- Persist state across browser sessions
- Handle asynchronous AI API responses
- Provide predictable state updates
- Minimize external dependencies for MVP

## Decision
We will use **React Hooks (useState, useEffect, useCallback) with localStorage persistence** for state management.

## Rationale

### Considered Options
1. **React Hooks + localStorage** - Built-in React state with local persistence
2. **Redux + Redux Toolkit** - Centralized state management library
3. **Zustand** - Lightweight state management library
4. **Context API + useReducer** - React's built-in context system
5. **Valtio** - Proxy-based state management
6. **SWR/React Query** - Server state management (with localStorage)

### Decision Factors

**React Hooks Advantages:**
- **No External Dependencies**: Built into React, no additional bundle size
- **Simple Learning Curve**: Standard React patterns, familiar to all React developers
- **Type Safety**: Excellent TypeScript integration without additional typing
- **Local Reasoning**: State co-located with components that use it
- **Performance**: No unnecessary re-renders with proper optimization
- **Flexibility**: Easy to refactor and adapt as requirements change
- **Debugging**: Simple debugging with React DevTools

**Comparison Analysis:**
| Feature | React Hooks | Redux | Zustand | Context API |
|---------|-------------|-------|---------|-------------|
| Bundle Size | 0KB | ~15KB | ~2KB | 0KB |
| Learning Curve | ⭐⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ |
| TypeScript | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| DevTools | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ |
| Boilerplate | ⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ |
| Performance | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ |

**Why Not Others:**
- **Redux**: Overkill for MVP, significant boilerplate, learning curve overhead
- **Zustand**: Additional dependency, abstracts away React patterns
- **Context API**: Performance issues with frequent updates, provider complexity
- **SWR/React Query**: Designed for server state, not local application state

## Implementation Architecture

### Core State Structure
```typescript
// App.tsx - Central state container
const [appData, setAppData] = useState<AppStateData>(getDefaultAppState());
const [currentOnboardingWeek, setCurrentOnboardingWeek] = useState<number>(0);
const [northStarMetrics, setNorthStarMetrics] = useState<NorthStarMetricData[]>(initialNorthStarMetrics);
const [sidebarOpen, setSidebarOpen] = useState(false);
const [apiKeyStatusMessage, setApiKeyStatusMessage] = useState<string | null>(null);
const [isDataLoaded, setIsDataLoaded] = useState(false);
```

### State Management Patterns

#### 1. Centralized App State
```typescript
interface AppStateData {
  currentPhaseId: number;
  audienceProfile: AudienceProfile;
  worldBible: WorldBible;
  visualIdentityKit: VisualIdentityKit;
  competitiveAnalysis: CompetitiveAnalysisData;
  recentActivity: ActivityItem[];
  contentIdeas: ContentIdea[];
  promptData: PromptEngineeringData[];
  generatedContentItems: GeneratedContentItem[];
  communityComments: CommunityComment[];
}
```

#### 2. Persistent State with localStorage
```typescript
// Auto-save state changes
useEffect(() => {
  if (isDataLoaded) {
    localStorage.setItem(LOCAL_STORAGE_KEY, JSON.stringify(appData));
  }
}, [appData, isDataLoaded]);

// Load state on mount
useEffect(() => {
  const savedData = localStorage.getItem(LOCAL_STORAGE_KEY);
  if (savedData) {
    const parsedData = JSON.parse(savedData) as AppStateData;
    setAppData(prev => ({...prev, ...parsedData}));
  }
  setIsDataLoaded(true);
}, []);
```

#### 3. Optimized State Updates
```typescript
// Memoized activity logger to prevent unnecessary re-renders
const addActivity = useCallback((description: string, type: ActivityItem['type'] = 'AI Task') => {
  const newActivity: ActivityItem = {
    id: Date.now().toString(),
    timestamp: new Date(),
    type,
    description,
  };
  setAppData(prev => ({
    ...prev,
    recentActivity: [newActivity, ...prev.recentActivity.slice(0, 49)]
  }));
}, []);
```

#### 4. Specialized Update Functions
```typescript
// Targeted state updates for specific data types
const updateAudienceProfile = (profile: AudienceProfile) => {
  setAppData(prev => ({ ...prev, audienceProfile: profile }));
  addActivity("Audience Profile updated.", "Asset Created");
};

const updateWorldBible = (wb: WorldBible) => {
  setAppData(prev => ({ ...prev, worldBible: wb }));
  addActivity("World Bible updated.", "Asset Created");
};
```

## Consequences

### Positive
- **Zero Dependencies**: No additional libraries or bundle size impact
- **Simple Mental Model**: Standard React patterns, easy to understand and maintain
- **Type Safety**: Full TypeScript support without additional configuration
- **Performance**: Optimized re-renders with useCallback and useMemo
- **Flexibility**: Easy to refactor state structure as requirements evolve
- **Debugging**: Clear data flow, easy to debug with React DevTools
- **Local Persistence**: Automatic data persistence without complex setup

### Negative
- **Manual Optimization**: Requires careful use of useCallback/useMemo for performance
- **Prop Drilling**: Potential props passing through multiple component levels
- **No Time Travel**: No built-in debugging tools like Redux DevTools
- **Manual Persistence**: Custom localStorage integration instead of automatic
- **State Scattering**: State logic distributed across multiple components
- **No Middleware**: No built-in support for logging, persistence middleware

### Risk Mitigation
- **Performance Monitoring**: Regular performance audits with React Profiler
- **State Architecture**: Keep state structure flat and normalized
- **Update Patterns**: Consistent patterns for state updates across the app
- **Error Boundaries**: Implement error boundaries for state corruption recovery
- **Data Validation**: Validate data structure on load from localStorage
- **Migration Path**: Design state structure for easy migration to external libraries

## State Organization Strategy

### 1. Component-Level State
```typescript
// Local UI state that doesn't need persistence
const [activeSection, setActiveSection] = useState<WorldFoundrySection>('positioning');
const [loading, setLoading] = useState(false);
const [errors, setErrors] = useState<string[]>([]);
```

### 2. Application-Level State
```typescript
// Shared business data that needs persistence
const [appData, setAppData] = useState<AppStateData>();
const [currentPhaseId, setCurrentPhaseId] = useState<number>(0);
```

### 3. Derived State
```typescript
// Computed values from existing state
const currentPhaseInfo = useMemo(() => 
  PHASES.find(p => p.id === currentPhaseId), 
  [currentPhaseId]
);

const isModuleLocked = useCallback((moduleKey: ModuleKey): boolean => {
  const config = ONBOARDING_WEEKS_CONFIG[currentOnboardingWeek];
  return !config?.unlocks.includes(moduleKey);
}, [currentOnboardingWeek]);
```

## Performance Optimizations

### 1. Memoization Strategy
```typescript
// Prevent unnecessary re-renders
const memoizedHandlers = useMemo(() => ({
  updateAudienceProfile,
  updateWorldBible,
  updateVisualIdentityKit,
  addActivity
}), []); // Dependencies managed carefully
```

### 2. State Update Batching
```typescript
// Batch related state updates
const handlePhaseAdvancement = useCallback(() => {
  const nextPhase = Math.min(currentPhaseId + 1, PHASES.length - 1);
  
  // Batch updates in single render cycle
  setAppData(prev => ({
    ...prev,
    currentPhaseId: nextPhase
  }));
  addActivity(`Advanced to Phase ${nextPhase}`, 'Phase Advanced');
}, [currentPhaseId, addActivity]);
```

### 3. Selective Re-rendering
```typescript
// Component-level optimization
const MemoizedComponent = React.memo(Component, (prevProps, nextProps) => {
  return prevProps.data.id === nextProps.data.id;
});
```

## Data Flow Architecture

### Update Flow
```
User Action → Event Handler → State Update Function → 
React State Change → localStorage Persistence → UI Re-render
```

### Loading Flow
```
App Mount → localStorage Read → Data Parsing → 
State Initialization → Component Rendering
```

## Future Migration Considerations

### To Redux (if needed)
```typescript
// Current state structure is already Redux-compatible
interface RootState {
  app: AppStateData;
  ui: UIState;
  settings: SettingsState;
}

// Current update functions can become Redux actions
const updateAudienceProfile = (profile: AudienceProfile) => ({
  type: 'UPDATE_AUDIENCE_PROFILE',
  payload: profile
});
```

### To Context API (if prop drilling becomes an issue)
```typescript
// Wrap state in context providers
const AppStateContext = createContext<AppStateData>();
const AppStateProvider = ({ children }) => (
  <AppStateContext.Provider value={appData}>
    {children}
  </AppStateContext.Provider>
);
```

## Testing Strategy

### Unit Testing State Logic
```typescript
describe('State Management', () => {
  test('updates audience profile correctly', () => {
    const { result } = renderHook(() => useAppState());
    
    act(() => {
      result.current.updateAudienceProfile(mockProfile);
    });
    
    expect(result.current.appData.audienceProfile).toEqual(mockProfile);
  });
});
```

### Integration Testing
```typescript
describe('State Persistence', () => {
  test('saves and loads state from localStorage', () => {
    // Test localStorage integration
    const savedData = localStorage.getItem(LOCAL_STORAGE_KEY);
    expect(JSON.parse(savedData)).toMatchObject(expectedState);
  });
});
```

## Monitoring & Performance

### Metrics to Track
- State update frequency and patterns
- Component re-render counts
- localStorage operation performance
- Memory usage with large datasets
- User interaction response times

### Optimization Triggers
- Component re-renders > 10 per user action
- localStorage operations > 100ms
- State object size > 1MB
- Memory leaks in development tools
- User-reported performance issues 