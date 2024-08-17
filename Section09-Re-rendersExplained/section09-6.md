# Re-renders Explained

## Custom hooks drawback
- A very important concept that we need to consider when working with states, re-render and performance is the use of custom hooks.
- These custom hooks were created to help us abstract stateful logic. For example, the logic that we discussed in our little application could be encapsulated in a custom hook: useDialog.

- To make it even more clear: you can think of hooks as compartments in a backpack.
  - For example, carrying a heavy book in your hands makes it difficult to move quickly.
  - However, if you put the same heavy book in your backpack doesn't make running any easier because you still carry the same way.
  - But if you place that book on a motorized cart that follows you, you can move freely as the cart handles the load. 
  - In the context of components that cart represents how state should be managed.
  - The same principle applies when hooks utilize other hooks, any factor within the hook chain that might initiate a re-render, no matter how deeply embedded, will cause re-render in the component using the inital hook. 
  - Using the useCounter inside of useDialog, we're just accessing the use counter, we are not even making use of any state that coming from it. Thinking of it as the book and the backpack, you are doing the same thing now every second this counter state is being updated, and because of that, every state, every second this useToggleDialog will trigger a re-render for the app component and you will have the same performance issue as before, no matter how you separate the logic hooks.
  - So even in this scenario, to optimize your app, you will still need to segregate the button, the dialog and the custom hook into a separate component.

  ```
  import { useState, useEffect } from "react";

  const useCounter = () => {
    const [counter, setCounter] = useState(0);

    useEffect(() => {
      const interval = setInterval(() => {
        setCounter((prevCounter) => prevCounter + 1);
      }, 1000);

      return () => clearInterval(interval);
    }, [1000]);

    return null;
  };

  export default useCounter;
  ```

  ```
  import { useEffect, useState } from "react";
  import useCounter from "./useCounter";

  const useToggleDialog = () => {
    const [visible, setVisible] = useState(false);

    useCounter();

    return {
      isVisible: visible,
      show: () => setVisible(true),
      hide: () => setVisible(false),
    };
  };

  export default useToggleDialog;
  ```

  ```
  import { Button } from "./button";
  import { ModalDialog } from "./modal-dialog";
  import useToggleDialog from "../hooks/useDialog";

  const ToggleButtonWithDialog = () => {
    const { isVisible, show, hide } = useToggleDialog();

    return (
      <>
        <Button onClick={show}>Show dialog</Button>
        {isVisible && <ModalDialog onClose={hide} />}
      </>
    );
  };

  export default ToggleButtonWithDialog;
  ```
  
  ```
  import { SlowComponent } from "./components/slow-component";
  import { AdditionalComplexThings, BlaBla } from "./components/dummy-components";
  import ToggleButtonWithDialog from "./components/toggle-button";
  import useToggleDialog from "./hooks/useDialog";
  import { Button } from "./components/button";
  import { ModalDialog } from "./components/modal-dialog";

  export default function App() {
    //This hook has a counter that triggers state update every 1 second.
    //To solve it you need to extract the Button and the ModalDialog in a smaller component
    //const { isVisible, show, hide } = useToggleDialog();
    return (
      <>
        <ToggleButtonWithDialog />
        {/* <Button onClick={show}>Show daialog</Button>
        {isVisible ? <ModalDialog onClose={hide} /> : null} */}
        <SlowComponent />
        <BlaBla />
        <AdditionalComplexThings />
      </>
    );
  }
  ```