# Design Patterns: Higher Order Components (HOCs)

## Checking Props with HOC
- With HOC, before we show that component X, we want to wrap it with a kind of shock that we can validate or do some checks.

  - HOC name starts with lower case.
  - function that receives a component as a prop. checkProps = (Component)
  - the props is the props that came with the component (Component)
  - return the component itself with the props

- In fact, in case you need to do some kind of validation or some kind of checking the props of a component, you can to log them.

  ```
  export const checkProps = (Component) => {
    return (props) => {
      console.log(props);
      return <Component {...props} />;
    };
  };
  ```

  - How to use:
    - import the component that you want to apply the HOC.
    - create a const using the HOC function with the component as an argument (prop).
    - pass the props that you want to log (it can be the component props or something else since there is no interface)

  ```
  import { checkProps } from "./components/check-props";
  import { UserInfo } from "./components/user-info";

  const UserInfoWrapper = checkProps(UserInfo);

  function App() {
    return (
      <>
        <UserInfoWrapper propA="test1" blabla={{ a: 1, age: 23 }} />
      </>
    );
  }

  export default App;
  ```
 