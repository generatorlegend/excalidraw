# Getting Started with Excalidraw

Excalidraw is a powerful and flexible drawing library that allows you to easily integrate a drawing interface into your web applications. This guide will walk you through the process of installing Excalidraw, basic usage, and an overview of its main features.

## Installation

To install Excalidraw in your project, use npm or yarn:

```bash
npm install @excalidraw/excalidraw
# or
yarn add @excalidraw/excalidraw
```

## Basic Usage

Once installed, you can import and use Excalidraw in your React application:

```jsx
import React from "react";
import { Excalidraw } from "@excalidraw/excalidraw";

function App() {
  return (
    <div style={{ height: "500px" }}>
      <Excalidraw />
    </div>
  );
}

export default App;
```

This will render the Excalidraw component with default settings.

## Main Features

Excalidraw offers a wide range of features:

1. Drawing tools: Rectangle, diamond, ellipse, arrow, line, free-draw, and text
2. Customizable styles: Color, fill, stroke width, font size, etc.
3. Collaboration features
4. Export/Import functionality
5. Customizable UI
6. Theming support

## Customizing Excalidraw

You can customize Excalidraw's behavior and appearance using props:

```jsx
import React from "react";
import { Excalidraw } from "@excalidraw/excalidraw";

function App() {
  return (
    <div style={{ height: "500px" }}>
      <Excalidraw
        initialData={{
          elements: [
            /* Your initial elements */
          ],
          appState: {
            viewBackgroundColor: "#f1f3f5",
          },
        }}
        onChange={(elements, state) => {
          console.log("Elements:", elements);
          console.log("State:", state);
        }}
        theme="dark"
        gridModeEnabled={true}
      />
    </div>
  );
}

export default App;
```

## Working with Elements

You can programmatically add, modify, or delete elements using the Excalidraw API:

```jsx
import React, { useEffect, useRef } from "react";
import { Excalidraw, exportToBlob } from "@excalidraw/excalidraw";

function App() {
  const excalidrawRef = useRef(null);

  useEffect(() => {
    const addRectangle = () => {
      const rectangle = {
        type: "rectangle",
        x: 100,
        y: 100,
        width: 200,
        height: 100,
        backgroundColor: "red",
      };
      excalidrawRef.current.updateScene({
        elements: [rectangle],
      });
    };

    addRectangle();
  }, []);

  const exportImage = async () => {
    const blob = await exportToBlob({
      elements: excalidrawRef.current.getSceneElements(),
      mimeType: "image/png",
      appState: excalidrawRef.current.getAppState(),
    });
    // Use the blob as needed (e.g., save it or display it)
  };

  return (
    <div style={{ height: "500px" }}>
      <Excalidraw ref={excalidrawRef} />
      <button onClick={exportImage}>Export Image</button>
    </div>
  );
}

export default App;
```

## Handling Events

Excalidraw provides various event handlers to respond to user interactions:

```jsx
import React from "react";
import { Excalidraw } from "@excalidraw/excalidraw";

function App() {
  const onPointerDown = (activeTool, pointerDownState) => {
    console.log("Pointer down:", activeTool, pointerDownState);
  };

  const onPointerUp = (activeTool, pointerUpState) => {
    console.log("Pointer up:", activeTool, pointerUpState);
  };

  return (
    <div style={{ height: "500px" }}>
      <Excalidraw
        onPointerDown={onPointerDown}
        onPointerUp={onPointerUp}
      />
    </div>
  );
}

export default App;
```

## Conclusion

This guide covers the basics of getting started with Excalidraw. For more advanced usage and a complete list of available props and methods, refer to the Excalidraw API documentation.

Remember to wrap your Excalidraw component in a container with a defined height, as Excalidraw will expand to fill its container.

Happy drawing!