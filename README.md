# Figma Plugin Bundle

Figma Plugin Bundle is a Vite-based tool for building plugins for Figma. It focuses on one specific problem: making communication between the UI and the plugin code simple, type-safe, and pleasant to use.

## Why Figma Plugin Bundle

Building Figma plugins is powerful. You can extend the editor with custom workflows tailored to your team or product. The problem starts when the UI (running in an iframe) needs to talk to the plugin code (running in the main thread).

By default, this means working with postMessage, setting up event listeners, defining payload contracts manually, and keeping types in sync on both sides. It works, but it adds boilerplate, weakens type safety, and makes refactoring harder than it should be.

There are libraries that improve this experience, including figwire. Figma Plugin Bundle builds on that idea and integrates it directly into the build process so you don’t have to think about wiring at all.

## How it works

With Figma Plugin Bundle, you write plugin-side functions normally and export them from files that follow a simple naming convention. During the build, Figma Plugin Bundle detects those files, bundles them into a single plugin file, and automatically connects them to the UI using an internal figwire instance.

From the UI perspective, those functions behave like regular async functions. You import them and call them. TypeScript provides full autocomplete and type inference across the UI–plugin boundary.

**Here’s an example.**

Plugin code (get-collections.code.ts):
```typescript
export async function getCollections() {
  const collections = await figma.variables.getLocalVariableCollectionsAsync();
  return collections.map(collection => ({
      id: collection.id,
      name: collection.name
    }));
}
```

UI usage:
```typescript
import { getCollections } from './get-collections.code.ts';

const collections = await getCollections();
```

There is no manual messaging, no event handling, and no duplicated types. You call a function and await the result.

## The result

Figma Plugin Bundle gives you a simple mental model: treat plugin-side functions as remote async functions. Everything is bundled into a single plugin file, fully typed, and easy to reason about.

The goal is robustness with minimal abstraction. The behavior is explicit, the implementation is predictable, and the developer experience stays clean.


## API
