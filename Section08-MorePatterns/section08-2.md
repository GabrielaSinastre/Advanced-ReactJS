# Design Patterns: More Patterns

## Observer Pattern
- The Observer Pattern is a behavioral design pattern that allows one object (the subject) to notify multiple other objects (the observers) about changes in its state. In this pattern, observers subscribe to the subject, and when the subject's state changes, it automatically notifies all the subscribed observers.

- mitt is a library that provides a simple event emitter.
- emitter is an instance of the event emitter, which will be used as the subject in this pattern.
- The emitter allows different parts of the application to subscribe to events (like "increment" and "decrement") and react when those events are emitted.
- The emitter acts as the subject that manages the events. It allows other components to observe these events and react accordingly.


- The Buttons component defines two event handlers: onIncrementCounter and onDecrementCounter.
- These handlers use the emitter to emit "increment" and "decrement" events when the corresponding buttons are clicked.
- The Buttons component acts as the subject that triggers events ("increment" and "decrement"). It doesn't know who is observing these events or what actions will be taken in response; it simply emits the events.

  ```
  import { emitter } from "../App";

  const Buttons = (props) => {
    const onIncrementCounter = () => {
      emitter.emit("increment");
    };
    const onDecrementCounter = () => {
      emitter.emit("decrement");
    };
    return (
      <div>
        <button onClick={onIncrementCounter}>➕</button>
        <button onClick={onDecrementCounter}>➖</button>
      </div>
    );
  };
  export default Buttons;
  ```

- Counter maintains a local state count to keep track of the counter value.
- Inside the useEffect hook, onIncrement and onDecrement functions are defined to update the count state.
- The component subscribes to the "increment" and "decrement" events using emitter.on and updates the count state accordingly.
- The useEffect also ensures that when the component unmounts, the event listeners are removed with emitter.off.
- Counter acts as the observer. It subscribes to the "increment" and "decrement" events (subject), reacting to changes by updating its local state.
- The pattern is clear here as the Counter reacts to the events emitted by the Buttons component without direct interaction between the two components.

  ```
  import { useEffect, useState } from "react";
  import { emitter } from "../App";

  const Counter = () => {
    const [count, setCount] = useState(0);
    useEffect(() => {
      const onIncrement = () => {
        setCount((count) => count + 1);
      };
      const onDecrement = () => {
        setCount((count) => count - 1);
      };
      emitter.on("increment", onIncrement);
      emitter.on("decrement", onDecrement);
      return () => {
        emitter.off("increment", onIncrement);
        emitter.off("decrement", onDecrement);
      };
    }, []);
    return <div>#: {count}</div>;
  };
  export default Counter;
  ```

- ParentComponent simply renders both the Buttons and Counter components.
- There is no direct interaction between Buttons and Counter. They communicate through the emitter.
- This component facilitates the pattern by rendering both the subject (Buttons) and the observer (Counter), which interact through the emitter without directly depending on each other.

  ```
  import Buttons from "./buttons";
  import Counter from "./counter";

  const ParentComponent = (props) => {
    return (
      <>
        <Buttons />
        <Counter />
      </>
    );
  };
  export default ParentComponent;
  ```


- App serves as the entry point of the application, setting up the emitter and rendering the ParentComponent.
- It exports the emitter so that any component can access it to either emit or listen to events.
Observer Pattern:
- The emitter is provided globally to facilitate communication between components, making it easy to implement the observer pattern across different parts of the application.

  ```
  import ParentComponent from "./components/parent";
  import mitt from "mitt";

  export const emitter = mitt();

  function App() {
    return (
      <>
        <ParentComponent />
      </>
    );
  }

  export default App;
  ```

- This code is a textbook implementation of the Observer Pattern in React. The Buttons component acts as the subject by emitting events ("increment" and "decrement"), and the Counter component acts as the observer, reacting to these events by updating the displayed count. The emitter provides the necessary infrastructure for this pattern, allowing components to communicate in a decoupled manner.