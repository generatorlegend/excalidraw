# Collaboration Setup

This guide explains how to set up and use Excalidraw's collaboration features, enabling real-time collaboration, managing collaborators, and implementing collaboration in various environments.

## Enabling Real-time Collaboration

To enable real-time collaboration in your Excalidraw instance, follow these steps:

1. Set up the `isCollaborating` prop when initializing the Excalidraw component:

```jsx
<Excalidraw
  isCollaborating={true}
  // ... other props
/>
```

2. Implement the `onCollaboratorChange` callback to handle updates to the list of collaborators:

```jsx
const onCollaboratorChange = (collaborators) => {
  // Handle collaborator updates
  console.log("Collaborators:", collaborators);
};

<Excalidraw
  isCollaborating={true}
  onCollaboratorChange={onCollaboratorChange}
  // ... other props
/>
```

## Managing Collaborators

Excalidraw provides several features to manage collaborators:

### Collaborator Object

The `Collaborator` object contains information about each collaborator:

```typescript
type Collaborator = Readonly<{
  pointer?: CollaboratorPointer;
  button?: "up" | "down";
  selectedElementIds?: AppState["selectedElementIds"];
  username?: string | null;
  userState?: UserIdleState;
  color?: {
    background: string;
    stroke: string;
  };
  avatarUrl?: string;
  id?: string;
  socketId?: SocketId;
  isCurrentUser?: boolean;
  isInCall?: boolean;
  isSpeaking?: boolean;
  isMuted?: boolean;
}>;
```

### Tracking Collaborators

Use the `collaborators` state in the `AppState` to keep track of active collaborators:

```typescript
collaborators: Map<SocketId, Collaborator>
```

### Updating Collaborator States

To update collaborator states, use the `syncActionResult` method:

```typescript
this.syncActionResult({
  collaborators: updatedCollaborators,
  // ... other state updates
});
```

## Implementing Collaboration Features

### Real-time Updates

To implement real-time updates, use the `onChange` prop:

```jsx
const onChange = (elements, appState, files) => {
  // Sync changes with other collaborators
  sendChangesToCollaborators(elements, appState, files);
};

<Excalidraw
  onChange={onChange}
  // ... other props
/>
```

### Following Users

Implement user following with the `userToFollow` state:

```typescript
userToFollow: UserToFollow | null;
```

Use the `onUserFollow` prop to handle user follow events:

```jsx
const onUserFollow = (payload) => {
  // Handle user follow/unfollow actions
  console.log("User follow action:", payload);
};

<Excalidraw
  onUserFollow={onUserFollow}
  // ... other props
/>
```

### Laser Pointer

The laser pointer tool can be used for highlighting during collaboration:

```typescript
if (this.state.activeTool.type === "laser") {
  this.laserTrails.addPointToPath(pointerCoords.x, pointerCoords.y);
}
```

## Best Practices

1. Use a robust WebSocket or real-time database solution for syncing data between collaborators.
2. Implement proper error handling and conflict resolution mechanisms.
3. Consider using operational transformation or a similar algorithm for handling concurrent edits.
4. Optimize network usage by sending only delta updates when possible.
5. Implement proper security measures to ensure only authorized users can collaborate.
6. Use the `userToFollow` feature judiciously to avoid disrupting other users' workflows.
7. Provide clear visual indicators for collaborator presence and actions.

## Troubleshooting

- If collaborators are not seeing real-time updates, check your WebSocket connection and ensure that the `isCollaborating` prop is set to `true`.
- If user following is not working correctly, verify that the `userToFollow` state is being updated properly and that the `onUserFollow` callback is implemented correctly.
- For performance issues during collaboration, consider optimizing your network calls and implementing debouncing for frequent updates.

By following this guide, you should be able to set up and use Excalidraw's collaboration features effectively in your application.