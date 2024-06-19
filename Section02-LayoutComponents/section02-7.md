# Design Patterns: Layout Components

## Modals
- How to create a modal using a layout component.
- It is simple and reusable!
- Observe that the children got the parent styles.

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

  export const Modal = ({ children }) => {
    const [show, setShow] = useState(false);

    return (
      <>
        <button onClick={() => setShow(true)}>Show Modal</button>
        {show && (
          <ModalBackground onClick={() => setShow(false)}>
            <ModalContent onClick={(e) => e.stopPropagation()}>
              <button onClick={() => setShow(false)}>Hide Modal</button>
              {children}
            </ModalContent>
          </ModalBackground>
        )}
      </>
    );
  };
  ```

  App
  ```
  import { Modal } from "./components/Modal";
  import { LargeBookListItem } from "./components/books/LargeListItems";
  import { books } from "./data/books";

  function App() {
    return (
      <>
        <Modal>
          <LargeBookListItem book={books[0]} />
        </Modal>
      </>
    );
  }

  export default App;
  ```
