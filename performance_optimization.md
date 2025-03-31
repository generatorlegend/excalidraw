# Performance Optimization

This guide provides tips and best practices for optimizing Excalidraw performance in large-scale applications, especially when dealing with complex diagrams and a large number of elements.

## Efficient Rendering

### 1. Use the StaticCanvas and InteractiveCanvas

Excalidraw uses two separate canvases for rendering: StaticCanvas and InteractiveCanvas. This separation allows for more efficient rendering and updates.

- StaticCanvas: Renders the main scene elements
- InteractiveCanvas: Handles interactive elements and temporary drawings

Ensure that you're utilizing both canvases correctly in your implementation:

```jsx
<StaticCanvas
  canvas={this.canvas}
  rc={this.rc}
  elementsMap={elementsMap}
  allElementsMap={allElementsMap}
  visibleElements={visibleElements}
  // ... other props
/>

<InteractiveCanvas
  containerRef={this.excalidrawContainerRef}
  canvas={this.interactiveCanvas}
  elementsMap={elementsMap}
  visibleElements={visibleElements}
  // ... other props
/>
```

### 2. Implement Efficient Element Caching

Use the `ShapeCache` to store and retrieve element shapes, reducing the need for frequent recalculations:

```javascript
if (ShapeCache.get(element)) {
  // Use cached shape
} else {
  // Calculate and cache new shape
  ShapeCache.set(element, calculatedShape);
}
```

### 3. Optimize Viewport Rendering

Implement viewport-based rendering to only draw elements that are currently visible:

```javascript
const visibleElements = this.scene.getVisibleElements(this.state);
```

## State Management

### 1. Use Immutable State Updates

When updating the state, use immutable update patterns to improve performance and prevent unnecessary re-renders:

```javascript
this.setState((prevState) => ({
  ...prevState,
  selectedElementIds: {
    ...prevState.selectedElementIds,
    [element.id]: true,
  },
}));
```

### 2. Batch Updates

Use `withBatchedUpdates` to combine multiple state updates into a single render cycle:

```javascript
withBatchedUpdates(() => {
  this.setState({ /* update 1 */ });
  this.setState({ /* update 2 */ });
  // ... more updates
});
```

### 3. Implement Efficient Selection Management

Use `makeNextSelectedElementIds` for efficient selection state updates:

```javascript
this.setState((prevState) => ({
  selectedElementIds: makeNextSelectedElementIds(
    updatedSelectedElementIds,
    prevState
  ),
}));
```

## Handling Large Numbers of Elements

### 1. Implement Virtual Scrolling

For applications with a large number of elements, implement virtual scrolling to render only the elements currently in view:

```javascript
const elementsInViewport = this.getElementsInViewport(this.state);
```

### 2. Use Efficient Data Structures

Utilize efficient data structures like Maps for faster element lookups:

```javascript
const elementsMap = this.scene.getNonDeletedElementsMap();
```

### 3. Optimize Element Updates

When updating elements, use `mutateElement` for efficient single-element updates:

```javascript
mutateElement(element, {
  x: newX,
  y: newY,
});
```

For bulk updates, use `this.scene.replaceAllElements()`:

```javascript
this.scene.replaceAllElements(updatedElements);
```

## Additional Performance Tips

### 1. Debounce and Throttle Events

Use debouncing and throttling for frequent events like scrolling or resizing:

```javascript
const debouncedUpdateDOMRect = debounce(() => {
  this.updateDOMRect();
}, 100);
```

### 2. Optimize Image Handling

Implement efficient image loading and caching:

```javascript
const imageElement = await this.initializeImage({
  imageFile,
  imageElement: newImageElement,
});
```

### 3. Use Web Workers for Heavy Computations

Offload heavy computations to Web Workers to keep the main thread responsive:

```javascript
const worker = new Worker('heavy-computation.js');
worker.postMessage({ /* data */ });
worker.onmessage = (event) => {
  // Handle result
};
```

By implementing these optimizations, you can significantly improve the performance of Excalidraw in large-scale applications with complex diagrams and numerous elements. Remember to profile your application regularly to identify and address performance bottlenecks specific to your use case.