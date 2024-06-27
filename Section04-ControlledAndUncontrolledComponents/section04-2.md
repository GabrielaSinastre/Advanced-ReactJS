# Design Patterns: Controlled and Uncontrolled Components

## Uncontrolled Components
- This chapter describes a uncontrolled form (the next chapter this component will be transformed into a controlled component);
- The form is going to be a kind of element or component that is not going to be leak any of it's states.
- So not going to be able to access this forms elements using any kind of usestate or hooks. Are going to access them using the actual Dom, like using a function which are going to see a couple of seconds create ref and stuff like that.

  -> function SubmitForm
  Where we are going to access the values of this inputs, using the React Creator.
    - Example: to save the name value, I create a variable using a createRef(), and then I passed the value on the input prop called ref.
  -> So, we have the value of the input when we click on the submit button.
  -> The way that we access the value is called indirectly access.
  -> Once again: because this form is not acessible, the state of the features of it are not accessible from outside the component and parent components like App component.

  ```
  import React from "react";

  export const UncontrolledForm = () => {
    const nameInputRef = React.createRef();
    const ageInputRef = React.createRef();

    const SubmitForm = (e) => {
      console.log(nameInputRef.current.value);
      console.log(ageInputRef.current.value);

      e.preventDefault();
    };

    return (
      <form onSubmit={SubmitForm}>
        <input name="name" type="text" placeholder="Name" ref={nameInputRef} />
        <input name="age" type="number" placeholder="Age" ref={ageInputRef} />
        <input type="submit" value="Submit" />
      </form>
    );
  };
  ```

  ```
  import { UncontrolledForm } from "./components/uncontrolled-form";

  function App() {
    return (
      <>
        <UncontrolledForm />
      </>
    );
  }

  export default App;
  ```
