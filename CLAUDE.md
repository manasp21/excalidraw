# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Structure

Excalidraw is a **monorepo** with clear separation between core library and applications:

- **`packages/excalidraw/`** - Main React component library published to npm as `@excalidraw/excalidraw`
- **`excalidraw-app/`** - Full-featured web application (excalidraw.com) that uses the library
- **`packages/common/`** - Shared utilities (colors, constants, keys, points)
- **`packages/element/`** - Element manipulation, rendering, collision detection
- **`packages/math/`** - Mathematical utilities (geometry, curves, transformations)
- **`packages/utils/`** - Export utilities and geometry helpers
- **`examples/`** - Integration examples (NextJS, browser script)

## Development Commands

```bash
# Testing
yarn test                  # Run tests in watch mode
yarn test:app              # Run vitest tests  
yarn test:update           # Run all tests with snapshot updates
yarn test:typecheck        # TypeScript type checking
yarn test:all              # Run all tests (typecheck, code, other, app)

# Linting and Formatting  
yarn fix                   # Auto-fix formatting and linting
yarn fix:code              # ESLint auto-fix
yarn fix:other             # Prettier auto-fix

# Building
yarn build                 # Build the app
yarn build:packages        # Build all packages
yarn build:excalidraw      # Build main excalidraw package
yarn start                 # Start development server

# Cleanup
yarn rm:build              # Remove all build artifacts
yarn clean-install        # Remove node_modules and reinstall
```

## Architecture

### Package System & Path Aliases

- Uses Yarn workspaces for monorepo management
- Internal packages use path aliases defined in `vitest.config.mts`:
  - `@excalidraw/common` → `packages/common/src`
  - `@excalidraw/element` → `packages/element/src` 
  - `@excalidraw/excalidraw` → `packages/excalidraw`
  - `@excalidraw/math` → `packages/math/src`
  - `@excalidraw/utils` → `packages/utils/src`

### Key Architectural Patterns

- **Element Management**: All drawing elements are handled through `packages/element/`
- **Math Operations**: Geometric calculations use `packages/math/` with typed Point interfaces
- **Scene Management**: Rendering and scene state in `packages/excalidraw/scene/`
- **Actions System**: User interactions handled via action system in `packages/excalidraw/actions/`
- **Component Architecture**: React components in `packages/excalidraw/components/`

### Testing Strategy

- Uses Vitest for unit testing with jsdom environment
- Snapshots for UI component testing
- Coverage thresholds: 60% lines, 70% branches, 63% functions
- Always run `yarn test:app` after modifications to catch regressions

### Code Standards

- TypeScript throughout with strict configuration
- Prefer immutable data patterns (const, readonly)
- Use optional chaining (?.) and nullish coalescing (??)
- Performance-focused: trade RAM for CPU cycles when possible
- Always use Point type from `packages/math/src/types.ts` instead of `{ x, y }`

### Development Workflow

1. **Core Features**: Work in `packages/*` for editor functionality
2. **App Features**: Work in `excalidraw-app/` for app-specific features  
3. **Testing**: Always run `yarn test:app` after changes
4. **Type Safety**: Run `yarn test:typecheck` before committing
