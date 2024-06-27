# Design Patterns: Controlled and Uncontrolled Components

## Controlled Components
- The basic difference with them is that we track their state, the value that the user types into the form using hooks.
- The inputs are the same, but now we don't use ref. Instead, we pass the current value. This value always changes when the input changes (the user types something).
- When values are typed, the onChange is called, getting the value based on the typing event.

- The benefits
  -> For example, if you need to do some input validations, whenever the user starts typing or entering the inputs before they click the submit button, you can do it very easily with this kind of form.
    -> This example can be observed with the useEffect hook. Whenever the user or the name changes, the useEffect is going to fire.
    -> With this validation, you can get an error message, set a new state called error, and later display it to the user to show your validation.
  -> Controlled forms are more flexible when it comes to adding features to your applications.


  ```
  import { useEffect } from "react";
  import { useState } from "react";

  export const ControlledForm = () => {
    const [error, setError] = useState("");
    const [name, setName] = useState("");
    const [age, setAge] = useState();

    //console.log("rendering");

    useEffect(() => {
      if (name.length < 1) {
        setError("The name can not be empty");
      } else {
        setError("");
      }
    }, [name]);

    return (
      <form>
        {error && <p>{error}</p>}
        <input
          name="name"
          type="text"
          placeholder="Name"
          value={name}
          onChange={(e) => setName(e.target.value)}
        />
        <input
          name="age"
          type="number"
          placeholder="Age"
          value={age}
          onChange={(e) => setAge(e.target.value)}
        />
        <button>Submit</button>
      </form>
    );
  };
  ```

  ```
  import { ControlledForm } from "./components/controlled-form";
  import { UncontrolledForm } from "./components/uncontrolled-form";


  function App() {
    return (
      <>
        <ControlledForm />
      </>
    );
  }

  export default App;
  ```