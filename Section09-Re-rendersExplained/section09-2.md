# Re-renders Explained

## Locating an issue
- Your initial assignment is to integrate a straightforward button at the top of this app that triggers a modal dialog
- So, here we have some components:
  - Button, Duummy Components, Modal Dialog and Slow Component (take sometime to render).

  ```
  import { ReactNode } from "react";
  import styled from "styled-components";

  type Props = {
    onClick: () => void;
    children: ReactNode;
  };

  const StyledButton = styled.button`
    background-color: #007bff;
    border: none;
    color: white;
    padding: 0.5rem 1rem;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 1rem;
    margin: 0.25rem 0;
    cursor: pointer;
    border-radius: 4px;

    &:hover {
      background-color: #0056b3;
    }
  `;

  export const Button = ({ onClick, children }: Props) => {
    return <StyledButton onClick={onClick}>{children}</StyledButton>;
  };
  ```

  ```
  import styled from "styled-components";

  const StyledDiv = styled.div`
    background: linear-gradient(135deg, #ffcc70, #ff6f61);
    border: 2px solid rgba(0, 0, 0, 0.1);
    padding: 2rem;
    margin: 1.5rem 0;
    min-height: 12rem;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    border-radius: 8px;
    display: flex;
    justify-content: center;
    align-items: center;
    color: #333;
    font-size: 1.2rem;
    font-weight: bold;
  `;

  const AdditionalStyle = styled.div`
    background: #333;
    color: #fff;
    padding: 1rem 2rem;
    margin-top: 1.5rem;
    border-radius: 0 0 8px 8px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 1rem;
    box-shadow: 0 -4px 8px rgba(0, 0, 0, 0.1);
  `;

  // Component that renders a div with specific class name and text content
  export const BlaBla = () => <StyledDiv>Some random stuff</StyledDiv>;

  // Another component that renders a div with different text content
  export const AdditionalComplexThings = () => (
    <AdditionalStyle>Additional complex stuff</AdditionalStyle>
  );
  ```

  ```
  import styled from "styled-components";
  import { Button } from "./button";

  type SimpleModalDialog = {
    onClose: () => void;
  };

  const ModalOverlay = styled.div`
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.5); /* Semi-transparent background */
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000; /* Ensure the modal is on top */
  `;

  const Modal = styled.div`
    background: white;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    width: 90%;
    max-width: 500px;
    padding: 1rem;
  `;

  const Content = styled.div`
    padding: 1rem;
    background: #f9f9f9;
    border-bottom: 1px solid #e5e5e5;
  `;

  const Footer = styled.div`
    display: flex;
    justify-content: flex-end;
    padding: 1rem;
    background: #f1f1f1;
    border-top: 1px solid #e5e5e5;
  `;

  export const ModalDialog = ({ onClose }: SimpleModalDialog) => {
    return (
      <ModalOverlay>
        <Modal>
          <Content>dummy content</Content>
          <Footer>
            <Button onClick={onClose}>close dialog</Button>
          </Footer>
        </Modal>
      </ModalOverlay>
    );
  };
  ```

  ```
  // Helper function to pause execution for a given number of milliseconds
  const pauseExecution = (milliseconds: number) => {
    const startTime = Date.now();
    let currentTime = startTime;

    while (currentTime - startTime < milliseconds) currentTime = Date.now();
  };

  // Component that simulates a slow operation
  export const SlowComponent = () => {
    pauseExecution(500); // Pause for 500 milliseconds
    return null; // This component renders nothing
  };

  // Another component that simulates a slow operation
  export const AnotherSlowComponent = () => {
    pauseExecution(500); // Pause for 500 milliseconds
    return null; // This component renders nothing
  };
  ```
  
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

- We simply added a statew that keeps track of whether the dialog is opened or closed.
- Then, wwe added a button that updates that state when clicked and the dialog that appears if the state is set to true.
- When you launch the app and test it you see that there is a delay, it takes almost a second for the dialog to appear.
- So you can click more than once and when you want to close it again, it takes one second to disappear too.
- When you take a look at the code in here you see that we have some slow components, so perhaps those are putting that delay.
- Those familiar with optimizing performance in react might quickly suggest in such scenario: the entire app is rendering, so let's use react memo and callback hooks to prevent this...
  - This advice is technically correct, but using memoization here is actually unnecessary and can be somehow counter productive.
  - There is a better aproach but firts we need to understand what is hapenning in the code an why.