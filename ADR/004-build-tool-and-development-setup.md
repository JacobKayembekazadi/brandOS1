# ADR-004: Build Tool and Development Setup

## Status
Accepted

## Date
2025-01-01

## Context
Brand Central AI needs a modern build tool and development environment that supports:
- Fast development iteration cycles for rapid prototyping
- TypeScript compilation with strict type checking
- Modern JavaScript features (ES2020+) for clean code
- Environment variable management for API keys
- Hot module replacement for instant feedback during development
- Optimized production builds with code splitting
- Easy deployment to static hosting platforms

## Decision
We will use **Vite 6.2.0** as our build tool and development server.

## Rationale

### Considered Options
1. **Vite** - Next-generation frontend build tool
2. **Create React App (CRA)** - Official React starter with Webpack
3. **Custom Webpack** - Manual Webpack configuration
4. **Parcel** - Zero-configuration build tool
5. **Snowpack** - ESM-focused build tool (now Vite)

### Decision Factors

**Vite Advantages:**
- **Lightning Fast**: ESM-based dev server with instant hot reload
- **TypeScript Native**: Built-in TypeScript support without additional config
- **Modern by Default**: Targets ES2020+ with automatic polyfills
- **Environment Variables**: Seamless .env file support with type safety
- **Production Optimization**: Rollup-based builds with tree shaking
- **Plugin Ecosystem**: Rich plugin system for extensibility
- **Framework Agnostic**: Not tied to specific React tooling
- **Active Development**: Cutting-edge features and performance improvements

**Performance Comparison:**
| Metric | Vite | CRA | Webpack | Parcel |
|--------|------|-----|---------|---------|
| Dev Start Time | ~100ms | ~3-5s | ~10-15s | ~2-3s |
| Hot Reload | ~10ms | ~500ms | ~1-2s | ~200ms |
| Build Speed | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐ |
| Bundle Size | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ |
| Configuration | ⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐ |

**Why Not Others:**
- **CRA**: Slower development experience, ejection complexity, outdated tooling
- **Custom Webpack**: High maintenance overhead, configuration complexity
- **Parcel**: Limited plugin ecosystem, less control over optimization
- **Snowpack**: Merged into Vite, no longer actively maintained

## Implementation Configuration

### Core Vite Config
```typescript
// vite.config.ts
import { defineConfig, loadEnv } from 'vite';
import path from 'path';

export default defineConfig(({ mode }) => {
  const env = loadEnv(mode, '.', '');
  return {
    define: {
      'process.env.API_KEY': JSON.stringify(env.GEMINI_API_KEY),
    },
    resolve: {
      alias: {
        '@': path.resolve(__dirname, '.'),
      }
    }
  };
});
```

### Package.json Scripts
```json
{
  "scripts": {
    "dev": "vite",           // Development server
    "build": "vite build",   // Production build  
    "preview": "vite preview" // Preview production build
  }
}
```

### TypeScript Integration
- **Native Support**: No additional loaders or plugins needed
- **Strict Mode**: `tsconfig.json` with strict type checking
- **Path Mapping**: Absolute imports with `@/` alias
- **Type Safety**: Environment variables with proper typing

### Environment Management
```typescript
// Environment variables
GEMINI_API_KEY=your_api_key_here

// Vite automatically loads:
// .env.local (local overrides, gitignored)
// .env.development (development mode)
// .env.production (production mode)
```

## Consequences

### Positive
- **Developer Experience**: Sub-second dev server startup and hot reload
- **Performance**: 10x faster than traditional bundlers during development
- **TypeScript Excellence**: Zero-config TypeScript with full type checking
- **Modern Standards**: ES2020+ features, dynamic imports, tree shaking
- **Deployment Ready**: Optimized production builds for static hosting
- **Future Proof**: Active development with modern web standards
- **Simple Configuration**: Minimal config for complex setups
- **Environment Safety**: Secure API key management with compile-time injection

### Negative
- **Newer Ecosystem**: Less mature than Webpack, fewer legacy tutorials
- **Learning Curve**: Different mental model from traditional bundlers
- **Plugin Compatibility**: Some Webpack plugins don't have Vite equivalents
- **Build Tool Lock-in**: Migration to other tools requires configuration rewrite

### Risk Mitigation
- **Documentation**: Comprehensive team training on Vite concepts
- **Plugin Selection**: Careful vetting of third-party plugins
- **Fallback Plan**: Keep migration path to Webpack if needed
- **Version Pinning**: Use specific Vite versions to avoid breaking changes
- **Community Monitoring**: Stay updated with Vite ecosystem developments

## Development Workflow

### Local Development
```bash
npm run dev
# → Starts dev server on http://localhost:5173
# → Instant hot reload on file changes
# → TypeScript compilation in background
```

### Production Build
```bash
npm run build
# → TypeScript compilation and type checking
# → Rollup optimization with tree shaking
# → Asset optimization and code splitting
# → Output to dist/ directory
```

### Build Analysis
```bash
npm run preview
# → Preview production build locally
# → Test optimized bundle behavior
# → Verify environment variable injection
```

## Performance Optimizations

### Development Speed
- **ESM Native**: No bundling during development
- **Dependency Pre-bundling**: Optimized handling of node_modules
- **Selective HMR**: Only affected modules reload
- **TypeScript Transpilation**: Fast esbuild-based compilation

### Production Optimization
- **Code Splitting**: Automatic route-based and dynamic splitting
- **Tree Shaking**: Dead code elimination via Rollup
- **Asset Optimization**: Image, CSS, and font compression
- **Browser Caching**: Optimized file naming for cache invalidation

## Monitoring & Metrics

### Development Metrics
- **Start Time**: Target <500ms for dev server startup
- **Hot Reload**: Target <100ms for module updates
- **Type Checking**: Real-time TypeScript error reporting
- **Bundle Analysis**: Regular build size monitoring

### Production Metrics
- **Bundle Size**: Target <500KB gzipped initial load
- **Build Time**: Target <30 seconds for CI/CD pipelines
- **Asset Optimization**: Automatic image and CSS optimization
- **Cache Strategy**: Optimal file naming for browser caching

## Future Considerations

### Potential Enhancements
- **PWA Plugin**: Service worker and offline capabilities
- **Bundle Analysis**: Visual bundle size analysis tools
- **Testing Integration**: Vitest for unit and integration testing
- **Deployment Optimization**: Platform-specific build optimizations

### Migration Paths
- **Webpack**: If complex legacy requirements emerge
- **Turbo**: For monorepo scenarios with multiple packages
- **esbuild**: For even faster builds if needed
- **SWC**: Alternative to esbuild for compilation

## Team Guidelines

### Configuration Management
- Keep `vite.config.ts` minimal and well-documented
- Use environment variables for all API keys and secrets
- Document any custom plugins or unusual configurations
- Regular updates with careful testing of breaking changes

### Development Practices
- Use `npm run dev` for all development work
- Test production builds regularly with `npm run preview`
- Monitor bundle size with each significant feature addition
- Leverage hot reload for rapid iteration cycles 