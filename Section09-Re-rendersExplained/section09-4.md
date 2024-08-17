# Re-renders Explained

## A Misconception About Re-rendering
- The misconception is: a component re-renders whrn it's props change.
- It's commonly accepted in the community, rarely questioned, but it's incorrect.
- Typically, react will re-render all nested components if a state update happens, regardless of any changes in props. If ther is no stat update changes in props are effectively ignored by react.
  - For example, if we have a component with a props and we just try to change those props without state update, nothing will happen.
  - See the code:
    - Instead of having state, let me create a local variable called 'visible', and it's going to be false.
    - Instead of saying set visible, I'm going to say visible receive true value/false.
    
    ```
    import { SlowComponent } from "./components/slow-component";
    import { AdditionalComplexThings, BlaBla } from "./components/dummy-components";
    import { Button } from "./components/button";
    import { useState } from "react";
    import { ModalDialog } from "./components/modal-dialog";

    export default function App() {
      const [visible, setVisible] = useState(false);
      return (
        <>
          {/* add the button */}
          <Button onClick={() => setVisible(true)}>Open Dialog</Button>
          {/* add the dialog itself */}
          {visible ? <ModalDialog onClose={() => setVisible(false)} /> : null}
          <SlowComponent />
          <BlaBla />
          <AdditionalComplexThings />
        </>
      );
    }
    ```

    - It simply won't work because it's not somehow re-rendering.
    - That's because clicking the button will change the local visible variable. However, without triggering the react lifecycle, the re-render output remains the same and the modal dialog is now going to appear.
    - In the scenario of re-renders changes in a component's props only matter if the component is enclosed by react memo. A Higher Order Component (HOC). So, only in this situation the react will halt it's routine re-render sequence to evaluate the props first. If there are no changes in the props, re-rendering ceases at that point and if any props change, the re-rendering proceeds as normal.
