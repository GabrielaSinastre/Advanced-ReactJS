# Design Patterns: Higher Order Components (HOCs)

## Building Forms with HOC
- On the previous class, we created a HOC:
  - [Updating Data with HOC](../Section05-HigherOrderComponents/section05-4.md)
  
- Now we can use this HOC on a form!
  - Create a const UserInfoForm that receive the HOC with the parameters
  - The parameters are: an anonymous function (the component) with the props that the component is required and a user id
  - Get data from this user (name and age)  
  - Create the inputs passing the value the value received from the desestructed component
  - Pass the onChange value received from props
  - Create a button to reset user passing the resetUser function prop
  - Create a button to save user passing the postUser function prop

  ```
  import { includeUpdatableUser } from "./include-updatable-user";

  export const UserInfoForm = includeUpdatableUser(
    ({ updatableUser, changeHandler, userPostHandler, resetUserHandler }) => {
      const { name, age } = updatableUser || {};

      return updatableUser ? (
        <>
          <label>
            Name:
            <input
              value={name}
              onChange={(e) => changeHandler({ name: e.target.value })}
            />
          </label>
          <label>
            Age:
            <input
              value={age}
              onChange={(e) => changeHandler({ age: Number(e.target.value) })}
            />
          </label>
          <button onClick={resetUserHandler}>Reset</button>
          <button onClick={userPostHandler}>Save</button>
        </>
      ) : (
        <h3>Loading...</h3>
      );
    },
    "3"
  );
  ```

  On app, pass the following component created:
  
  ```
  import { checkProps } from "./components/check-props";
  import { includeUser } from "./components/include-user";
  import { UserInfoForm } from "./components/user-form";
  import { UserInfo } from "./components/user-info";

  function App() {
    return (
      <>
        <UserInfoForm />
      </>
    );
  }

  export default App;
  ```


