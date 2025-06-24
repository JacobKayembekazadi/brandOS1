# ADR-005: Routing Strategy

## Status
Accepted

## Date
2025-01-01

## Context
Brand Central AI requires client-side routing to navigate between different modules:
- Dashboard (metrics and phase navigation)
- World Foundry (brand foundation tools)
- Content Studio (AI content creation)
- Distribution Hub (publishing and analytics)

Requirements:
- Seamless navigation between modules without page refreshes
- Support for static hosting deployment (GitHub Pages, Netlify, Vercel)
- Bookmarkable URLs for direct access to specific modules
- Browser back/forward button support
- Simple implementation without server configuration

## Decision
We will use **React Router DOM with HashRouter** for client-side navigation.

## Rationale

### Considered Options
1. **HashRouter** - Hash-based routing (#/path)
2. **BrowserRouter** - HTML5 History API routing (/path)
3. **MemoryRouter** - In-memory routing (no URL changes)
4. **Static Routing** - No routing, single page application

### Decision Factors

**HashRouter Advantages:**
- **Static Hosting Compatible**: Works with any static file server
- **No Server Configuration**: No need for URL rewrite rules
- **Universal Deployment**: Compatible with GitHub Pages, Netlify, Vercel
- **Immediate Functionality**: Works out-of-the-box on any hosting platform
- **Fallback Support**: Graceful degradation on older browsers
- **Simple Debugging**: URLs clearly show current route state

**Deployment Compatibility:**
| Platform | HashRouter | BrowserRouter | Configuration Required |
|----------|------------|---------------|----------------------|
| GitHub Pages | ✅ | ❌ | None vs Complex |
| Netlify | ✅ | ✅ | None vs _redirects file |
| Vercel | ✅ | ✅ | None vs vercel.json |
| Static Server | ✅ | ❌ | None vs Apache/Nginx config |

**Why Not Others:**
- **BrowserRouter**: Requires server configuration for SPA fallback (404 → index.html)
- **MemoryRouter**: No bookmarkable URLs, poor user experience
- **Static Routing**: Limited functionality, no deep linking

## Implementation Details

### Router Configuration
```typescript
// App.tsx
import { HashRouter, Routes, Route } from 'react-router-dom';

const App: React.FC = () => {
  return (
    <HashRouter>
      <Routes>
        <Route path="/" element={<Dashboard />} />
        <Route path="/world-foundry" element={<WorldFoundryModule />} />
        <Route path="/content-studio" element={<ContentStudioModule />} />
        <Route path="/distribution-hub" element={<DistributionHubModule />} />
      </Routes>
    </HashRouter>
  );
};
```

### Navigation Component
```typescript
// Navigation with hash-aware routing
const navItems = [
  { name: 'Dashboard', path: '/' },
  { name: 'World-Foundry', path: '/world-foundry' },
  { name: 'Content Studio', path: '/content-studio' },
  { name: 'Distribution Hub', path: '/distribution-hub' },
];

<NavLink to={item.path} className={...}>
  {item.name}
</NavLink>
```

### URL Structure
```
Production URLs:
https://brandcentral.ai/#/
https://brandcentral.ai/#/world-foundry
https://brandcentral.ai/#/content-studio
https://brandcentral.ai/#/distribution-hub
```

## Consequences

### Positive
- **Zero Configuration Deployment**: Works on any static hosting platform
- **Reliable Navigation**: No server-side routing dependencies
- **Bookmarkable URLs**: Users can bookmark and share specific module URLs
- **Browser Support**: Full back/forward button functionality
- **Development Simplicity**: No additional build or deployment configuration
- **Error Resilience**: Routing failures don't break the entire application
- **SEO Independence**: Since it's a private tool, SEO concerns are minimal

### Negative
- **URL Aesthetics**: Hash URLs are less clean than traditional paths
- **SEO Limitations**: Search engines handle hash routes differently (not relevant for this app)
- **Analytics Complexity**: Some analytics tools need special hash route configuration
- **Professional Perception**: Some users prefer clean URLs for professional tools

### Risk Mitigation
- **User Education**: Document URL structure in user guides
- **Analytics Configuration**: Properly configure Google Analytics for hash routing
- **Future Migration Path**: Design routing architecture for easy migration to BrowserRouter
- **URL Shortening**: Consider URL shortening for sharing if needed

## Routing Architecture

### Module-Based Structure
```
/#/                    → Dashboard (default)
/#/world-foundry       → Brand Foundation Module
/#/content-studio      → AI Content Creation Module  
/#/distribution-hub    → Publishing & Analytics Module
```

### State Management Integration
```typescript
// URL state synchronization
const currentModule = location.hash.replace('#/', '') || 'dashboard';

// Phase-based navigation hints
const getRecommendedModule = (phaseId: number): string => {
  if (phaseId <= 2) return 'world-foundry';
  if (phaseId <= 8) return 'content-studio';
  return 'distribution-hub';
};
```

### Navigation Guards
```typescript
// Module access control based on onboarding progress
const isModuleLocked = useCallback((moduleKey: ModuleKey): boolean => {
  const config = ONBOARDING_WEEKS_CONFIG[currentOnboardingWeek];
  return !config?.unlocks.includes(moduleKey);
}, [currentOnboardingWeek]);
```

## Performance Considerations

### Bundle Splitting
- No automatic route-based code splitting with HashRouter
- Consider manual dynamic imports for large modules
- Monitor bundle size and implement splitting if needed

### Navigation Performance
- Instant navigation between routes (no network requests)
- Preserved state during navigation for better UX
- No page refreshes or loading states

## Analytics & Tracking

### Google Analytics Configuration
```typescript
// Track hash route changes
useEffect(() => {
  const path = location.hash.replace('#', '') || '/';
  gtag('config', 'GA_TRACKING_ID', {
    page_path: path,
  });
}, [location.hash]);
```

### Custom Event Tracking
```typescript
// Track module navigation for usage analytics
const trackModuleVisit = (moduleName: string) => {
  addActivity(`Navigated to ${moduleName} module`, 'Navigation');
  // Additional analytics events
};
```

## Future Considerations

### Migration to BrowserRouter
If we decide to migrate to cleaner URLs in the future:

**Prerequisites:**
- Backend API server deployment
- Server-side rendering or proper SPA fallback configuration
- Deployment pipeline updates for server configuration

**Migration Strategy:**
- Implement BrowserRouter in parallel
- Add URL redirect logic from hash to clean URLs
- Gradual rollout with fallback to HashRouter
- User communication about URL changes

### Advanced Routing Features
- **Nested Routes**: For module sub-sections
- **Route Parameters**: For dynamic content (e.g., brand ID)
- **Query Parameters**: For filtering and search state
- **Protected Routes**: For premium features

## Testing Strategy

### Manual Testing
- Verify all navigation links work correctly
- Test browser back/forward buttons
- Confirm bookmarked URLs load correctly
- Validate routing on different hosting platforms

### Automated Testing
```typescript
// React Testing Library routing tests
describe('Navigation', () => {
  test('navigates to World Foundry module', () => {
    render(<App />);
    fireEvent.click(screen.getByText('World-Foundry'));
    expect(window.location.hash).toBe('#/world-foundry');
  });
});
```

### Deployment Testing
- Test routing on actual hosting platforms
- Verify URL sharing functionality
- Confirm deep linking works correctly
- Validate routing after page refresh 