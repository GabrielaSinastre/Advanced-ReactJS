# Design Patterns: Functional Programming in React

## Compositions
- Basically is somehow the inheritance pattern in the programming world. But here it has a slight differences and specific itself.
- Inside this pattern for demonstration purposes we are going to have a set of like 2 or 3 components.
  - 1. Basic button (is going to be used with some other components)
  - 2. Red button
  - 3. Green and small button

- The component number 1 is a functional component that receives a few props (size, color, text, ...props) and return a button with css properties based on the component props.

- The component number 2 will consume the component number 1.
  - Assume that you want to have a small or let's say a red button for danger/cancel button.
  - Return the component number 1 passing the color red (crimson)!

- The component number 3 will consume the component number 1 too.
  - But at this time, the color will be green and the size prop will be set as a small string value.

  ```
  export const Button = ({ size, color, text, ...props }) => {
    return (
      <button
        style={{
          fontSize: size === "large" ? "25px" : "16px",
          backgroundColor: color,
        }}
      >
        {text}
      </button>
    );
  };
  ```
  ```
  export const SmallButton = (props) => {
      return <Button {...props} size={"small"} />;
    };
    ```
    ```
    export const SmallRedButton = (props) => {
      return <SmallButton {...props} color={"crimson"} />;
    };
    ```

    - On the App.js:

    ```
    import { SmallButton, SmallRedButton } from "./components/composition";

    function App() {
      return (
        <>
          <SmallButton text={"I am small!"}/>
          <SmallRedButton text={"I am small and Red"}/>
        </>
      );
    }

    export default App;
    ```

    *The good thing about the composition pattern:*
    - Both the red button and the green small are not going to have copy paste the style code for both of them. They are just going to build on them. That's it!