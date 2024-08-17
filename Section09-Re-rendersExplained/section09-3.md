# Re-renders Explained

## Understanding component life-cycle
- Understanding the key phases in the life of a component is very important to performance optimization.
  - These pashes include: mounting, unmounting and re-endering.

  *Mounting:*
  - Happens when a component first appears on your screen.
  - During this phase, react sets up the component for the initializing it's state, executing it's hooks, and adding elements to the DOM.
  - The result is that the components output becomes visible on the screen.

  *Unmounting:*
  - Then comes unmounting, which occurs when react determines that a component is no longer necessary.
  - In this phase, react performs a final clean up, destroying the components instance and it's associated elements like the components state, and removes the DOM element.

  *Re-endering:*
  - This is when react updates an existing component with new data.
  - Is more efficient than mouting because react reuses the existing component instance runs, the hooks again performs the necessary calculations, the DOM elements with the new attributes.
  - Re-rendering begins with state change in react every time we use a hook like useState, useReducer or stage management library like Redux, we add interactivity to a component.
  - This means the component will now have data that persists throughout it's lifecycle.
  - If an event occurs that requires an interactive response like a user clicking a button or new external data arriving, we update the state with the new information.
  - So, understanding the re-rendering is very important to react.
  - It happens when new data prompts react to update a component and trigger all hooks reliant on that data.
  - Without this mechanism, there wwould be no updates to data, leading to no interactivity and static application.
  - State changes are the primary drives of re-renders in react apps, for example, in our sample application.
   - Clicking the button triggers the set visible setter function, changing the visible state from false to true. As a result, the app component hat holds this state re-render.
   - It means all the components inside of it are going to re-render.
  - Once the state is updated and the app component react needs to propagate the new data to other components that depend on it.
  - React handles this automatically re-rendering all components directly or indirectly rendered by the initial components or the main parent component, down to the component tree. This cascade continues until all the affected components are updated.
    - On our little aplication: all these low components it displays including this one which is really slow will be re-rendered when there is a state change (when you click the button or when you click the close on the modal dialog).
    - So consequently, opening the dialog takes almost a second because react has to re-render everything before the dialog can show up on the screen again.
  - It's important to know that react does not re-render components up the render tree. If a state updates starts in the middle of the component tree, only the components downward will be re-rendered.
  - So, for lower and lower level components to impact thoshe at the top level of the hierarchy, they must either directly trigger a state update in the top components, or pass component as functions.
