# Design Patterns: Controlled and Uncontrolled Components

## Controlled Modals
- On a previous chapter we have learned how to create a [Modal](../Section02-LayoutComponents/section02-7.md).
- This modal is uncontrolled because the modal itself manages its own state, like show and setShow, for displaying and hiding the component. So, if we have this component inside an App.js (parent component), other components cannot directly access its features. For example, if we want to press a button in App to display the modal, we can't because we don't have access to the show prop.

-> I can call the component below a controlled component.
-> Instead of creating a state to control the opening and closing of the modal, we can pass the state and the function to control the state as props. Now I can use this component and control it wherever the parent is.

  ```
  import { useState } from "react";
  import { styled } from "styled-components";

  const ModalBackground = styled.div`
    position: absolute;
    left: 0;
    top: 0;
    overflow: auto;
    background-color: #00000067;
    width: 100%;
    height: 100%;
  `;

  const ModalContent = styled.div`
    margin: 12% auto;
    padding: 24px;
    background-color: wheat;
    width: 50%;
  `;

  
  export const ControlledModal = ({ shouldShow, close, children }) => {
    return (
      <>
        {shouldShow && (
          <ModalBackground onClick={close}>
            <ModalContent onClick={(e) => e.stopPropagation()}>
              <button onClick={close}>Hide Modal</button>
              {children}
            </ModalContent>
          </ModalBackground>
        )}
      </>
    );
  };
  ```

  ```
  import { useState } from "react";
  import { ControlledForm } from "./components/controlled-form";
  import { ControlledModal } from "./components/controlled-modal";
  import { UncontrolledForm } from "./components/uncontrolled-form";

  function App() {
    const [showModal, setShowModal] = useState(false);
    return (
      <>
        <button onClick={() => setShowModal(!showModal)}>
          {" "}
          {showModal ? "Hide Modal" : "Show Modal"}{" "}
        </button>
        <ControlledModal shouldShow={showModal} close={() => setShowModal(false)}>
          <h1>I am the body of the modal!</h1>
        </ControlledModal>
      </>
    );
  }

  export default App;
  ```