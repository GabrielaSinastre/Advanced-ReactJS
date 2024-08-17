# Re-renders Explained

## Moving state
- We understand how react re-renders components works, so let's tackle the initial issue and solve the problem.
- Notice how the use of the state 'visible' is limited, it's only involved in the button and the modal dialog. The rest of the components do not rely on this state, so they don't need to re-render when the state changes.
- Putting these components in the react memo prevent their wasted re-rendering in our scenario. But react memo comes with it's own set of challengees drawbacks.
- HOWEVER, there is a more efficient solution:
  - We need to isolate only these two components that depends on the state in a smaller separate component!

  ```
  import { useState } from "react";
  import { Button } from "./button";
  import { ModalDialog } from "./modal-dialog";

  const ToggleButtonWithDialog = () => {
    const [visible, setVisible] = useState(false);

    return (
      <>
        <Button onClick={() => setVisible(true)}>Show dialog</Button>
        {visible && <ModalDialog onClose={() => setVisible(false)} />}
      </>
    );
  };

  export default ToggleButtonWithDialog;
  ```

  ```
  import { SlowComponent } from "./components/slow-component";
  import { AdditionalComplexThings, BlaBla } from "./components/dummy-components";
  import ToggleButtonWithDialog from "./components/toggle-button";

  export default function App() {
    return (
      <>
        <ToggleButtonWithDialog />
        <SlowComponent />
        <BlaBla />
        <AdditionalComplexThings />
      </>
    );
  }
  ```

  - Now, if you click on it, you see that we don't have that delay anymore!
  - With this change, when the button open dialog is clicked, the state update still happens, but now only the components within the toggle button with dialog component re-render. The older components will remain unaffected.

- Pratically, we just created a new sub branch in our render tree and relocated our state update downwards. Consequently, the modal dialog now appears intantly resolving a significant performance issue with a simple, straightforward compositional approach.