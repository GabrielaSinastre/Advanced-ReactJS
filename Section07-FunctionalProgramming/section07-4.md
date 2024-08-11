# Design Patterns: Functional Programming in React

## Partial Components
- This is just the next step of the composition pattern and allows us to simply use a part of a component rather than all of it.
- For achieving this, let's use HOC pattern for creating a partially applied component.
- So, create a HOC component, passing the original props and de partial props, that is the prop that will be passed on the time you will declare de HOC.

- Conclusions: When we use the higher order components for the composition pattern and get a partial pattern like this, it is more usable and it's cleaner because we are not repeting ourselves and we are just reusing some other components that have some props and styles that we already need and simply just apply some partial props that they do not have, like you see in this example.

  ```
  export const partial = (Component, partialProps) => {
    return (props) => {
      return <Component {...partialProps} {...props} />;
    };
  };

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

  export const SmallButton = partial(Button, { size: "small" });

  export const LargeRedButton = partial(Button, {
    size: "large",
    color: "crimson",
  });
  ```

  ```
  import { LargeRedButton, SmallButton } from "./components/partial";

  function App() {
    return (
      <>
        <SmallButton text={"I am small!"}/>
        <LargeRedButton text="I am large and Red"/>
      </>
    );
  }

  export default App;
  ```