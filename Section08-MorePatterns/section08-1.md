# Design Patterns: More Patterns

## Compound Components
- Imagine that you have instead a basic component, you have a product component which has a display, an image, maybe a description, a title price, several buttons and you have to display them based on some props. In this case, we have a easy way to do this.
- The compound components idea is very basic: it's about breaking down a component into several sub components. and then composing those sub components together in order to achieve the greater component.

  ```
  import { createContext, useContext } from "react";

  const Context = createContext(null);

  const Body = ({ children }) => {
    return <div style={{ padding: ".5rem" }}>{children}</div>;
  };

  const Header = ({ children }) => {
    const { test } = useContext(Context);
    return (
      <div
        style={{
          borderBottom: "1px solid black",
          padding: ".5rem",
          marginBottom: ".5rem",
        }}
      >
        {children}
        {test}
      </div>
    );
  };

  const Footer = ({ children }) => {
    return (
      <div
        style={{
          borderTop: "1px solid black",
          padding: ".5rem",
          marginTop: ".5rem",
        }}
      >
        {children}
      </div>
    );
  };

  const Card = ({ test, children }) => {
    return (
      <Context.Provider value={{ test }}>
        <div style={{ border: "1px solid black" }}>{children}</div>
      </Context.Provider>
    );
  };

  export default Card;

  Card.Header = Header;
  Card.Body = Body;
  Card.Footer = Footer;
  ```	

  ```
  import Card from "./components/card";

  function App() {
    return (
      <Card test="Value">
        <Card.Header>
          <h1 style={{ margin: "0" }}>Header</h1>
        </Card.Header>
        <Card.Body>
          He hid under the covers hoping that nobody would notice him there. It
          really didn't make much sense since it would be obvious to anyone who
          walked into the room there was someone hiding there, but he still held
          out hope. He heard footsteps coming down the hall and stop in front in
          front of the bedroom door. He heard the squeak of the door hinges and
          someone opened the bedroom door. He held his breath waiting for whoever
          was about to discover him, but they never did.
        </Card.Body>
        <Card.Footer>
          <button>Ok</button>
          <button>Cancel</button>
        </Card.Footer>
      </Card>
    );
  }

  export default App;
  ```

- How Does the Code Fit the Compound Components Pattern?  

  - Breaking Down the Component into Subcomponents:
  In the code, the Card component is broken down into three smaller components:

  *Card.Header:* Handles the display of the card's header, including the title or any other elements passed as children.
  *Card.Body:* Manages the content of the card's body.
  *Card.Footer:* Responsible for rendering the footer, typically containing actions like buttons.

  - These subcomponents can be thought of as the "building blocks" of the larger Card component.

  - Context for Shared State:
  The Context is used to pass down the test prop to the Header component without explicitly passing it as a prop to each subcomponent. This allows Card.Header, Card.Body, and Card.Footer to be composed together naturally, without worrying about prop drilling.

  - Flexible Composition:
  The Card component doesn't dictate how its subcomponents should be used. Instead, it allows the user to compose the Card.Header, Card.Body, and Card.Footer in any order and with any content. This gives a lot of flexibility to the developer, making the component highly reusable.