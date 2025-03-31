# API Reference

This document provides a detailed API reference for Excalidraw, including information on the `ExcalidrawProps` interface, `ExcalidrawImperativeAPI`, and other key types and interfaces.

## Table of Contents

1. [ExcalidrawProps](#excalidrawprops)
2. [ExcalidrawImperativeAPI](#excalidrawimperativeapi)
3. [Key Types and Interfaces](#key-types-and-interfaces)
4. [Exported Functions](#exported-functions)

## ExcalidrawProps

The `ExcalidrawProps` interface defines the properties that can be passed to the Excalidraw component.

### Key Properties

#### `onChange`

```typescript
onChange?: (
  elements: readonly OrderedExcalidrawElement[],
  appState: AppState,
  files: BinaryFiles,
) => void;
```

A callback function that is called when the scene changes. It provides the current elements, app state, and binary files.

#### `initialData`

```typescript
initialData?: (() => MaybePromise<ExcalidrawInitialDataState | null>) | MaybePromise<ExcalidrawInitialDataState | null>;
```

Initial data to load into the editor. Can be a function that returns a promise or the data directly.

#### `excalidrawAPI`

```typescript
excalidrawAPI?: (api: ExcalidrawImperativeAPI) => void;
```

A callback that provides access to the Excalidraw API.

#### `langCode`

```typescript
langCode?: Language["code"];
```

The language code for the editor. Defaults to the default language code.

#### `theme`

```typescript
theme?: Theme;
```

The theme for the editor (light or dark).

### Example Usage

```jsx
import { Excalidraw } from "@excalidraw/excalidraw";

function App() {
  return (
    <Excalidraw
      onChange={(elements, state, files) => {
        console.log("Scene has changed");
      }}
      initialData={() => {
        return { elements: [], appState: { theme: "dark" } };
      }}
      langCode="en"
      theme="dark"
    />
  );
}
```

## ExcalidrawImperativeAPI

The `ExcalidrawImperativeAPI` interface provides methods to interact with the Excalidraw instance programmatically.

### Key Methods

#### `updateScene`

```typescript
updateScene: InstanceType<typeof App>["updateScene"];
```

Updates the scene with new elements and/or app state.

#### `resetScene`

```typescript
resetScene: InstanceType<typeof App>["resetScene"];
```

Resets the scene to its initial state.

#### `getSceneElements`

```typescript
getSceneElements: InstanceType<typeof App>["getSceneElements"];
```

Returns the current scene elements.

#### `getAppState`

```typescript
getAppState: () => InstanceType<typeof App>["state"];
```

Returns the current app state.

### Example Usage

```jsx
import { Excalidraw } from "@excalidraw/excalidraw";

function App() {
  let excalidrawAPI;

  return (
    <div>
      <Excalidraw
        excalidrawAPI={(api) => {
          excalidrawAPI = api;
        }}
      />
      <button
        onClick={() => {
          const elements = excalidrawAPI.getSceneElements();
          console.log(elements);
        }}
      >
        Log Elements
      </button>
    </div>
  );
}
```

## Key Types and Interfaces

### AppState

The `AppState` interface represents the state of the Excalidraw application. It includes properties such as:

- `theme`: The current theme (light or dark)
- `zoom`: The current zoom level
- `viewModeEnabled`: Whether view mode is enabled
- `editingGroupId`: The ID of the group being edited

### ExcalidrawElement

The `ExcalidrawElement` type represents an element in the Excalidraw scene. It can be one of several types:

- `ExcalidrawLinearElement`
- `ExcalidrawImageElement`
- `ExcalidrawTextElement`
- ...and others

### BinaryFiles

The `BinaryFiles` type is a record of file IDs to `BinaryFileData` objects, which contain information about binary files (such as images) used in the scene.

## Exported Functions

Excalidraw exports several utility functions that can be used independently of the main component.

### `exportToCanvas`

```typescript
export function exportToCanvas(
  elements: readonly NonDeletedExcalidrawElement[],
  appState: AppState,
  files: BinaryFiles,
  opts?: ExportOpts,
): Promise<HTMLCanvasElement>;
```

Exports the given elements to a canvas.

### `exportToBlob`

```typescript
export function exportToBlob(
  elements: readonly NonDeletedExcalidrawElement[],
  appState: AppState,
  files: BinaryFiles,
  opts?: ExportOpts,
): Promise<Blob>;
```

Exports the given elements to a Blob.

### Example Usage

```javascript
import { exportToBlob } from "@excalidraw/excalidraw";

async function exportScene(elements, appState, files) {
  const blob = await exportToBlob({
    elements,
    appState,
    files,
    mimeType: "image/png",
    quality: 1,
  });
  // Use the blob...
}
```

This API reference provides an overview of the key interfaces, types, and functions available in Excalidraw. For more detailed information on specific components or advanced usage, please refer to the other sections of the documentation.