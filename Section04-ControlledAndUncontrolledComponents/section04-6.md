# Design Patterns: Controlled and Uncontrolled Components

## Collecting Data
- In this chapter we will create on the function the posibilities of map the maps that we have to pass by and collect the data of them.
- We need to do this on the GoNext function.
  -> Create a variable and call it next step index, to do the logic to get the next index.
  -> Collect data that is going to be received as an argument of this function (dataFromStep)
  -> Verify that the next step index is less of the children length, if it's true, we are not on the last step. If is not, call the on done function passing the updated data.
  -> setData with the collected data previously

- On each step, pass the data that you need to collect, for example: step 1 I passed 'MyName' value to the object that I will collect on the function goNext.

- Uncontrolled Form has the data collected from their children. But, the app has very much litter control over it. So, if I have hide the step 3 based on some criteria like the value received on step 2, I can't do it, because this is an uncontrolled flow and we don't have access to the data from outside components. 

  ```
  import React, { useState } from "react";

  export const UncontrolledFlow = ({ children, onDone }) => {
    const [data, setData] = useState({});
    const [currentStepIndex, setCurrentStepIndex] = useState(0);

    const currentChild = React.Children.toArray(children)[currentStepIndex];

    const next = (dataFromStep) => {
      const nextIndex = currentStepIndex + 1;
      const updatedData = { ...data, ...dataFromStep };

      console.log(updatedData);

      if (nextIndex < children.length) {
        setCurrentStepIndex(nextIndex);
      } else {
        onDone(updatedData);
      }

      setData(updatedData);
    };

    if (React.isValidElement(currentChild)) {
      return React.cloneElement(currentChild, { next });
    }

    return currentChild;
  };
  ```

  ```
  import { UncontrolledFlow } from "./components/uncontrolled-flow";

  const StepOne = ({ next }) => {
    return (
      <>
        <h1>Step #1: Enter your name</h1>
        <button onClick={() => next({ name: "TestName" })}>Next</button>
      </>
    );
  };
  const StepTwo = ({ next }) => {
    return (
      <>
        <h1>Step #2: Enter your age</h1>
        <button onClick={() => next({ age: 23 })}>Next</button>
      </>
    );
  };
  const StepThree = ({ next }) => {
    return (
      <>
        <h1>Step #3: Enter your country</h1>
        <button onClick={() => next({ country: "Poland" })}>Next</button>
      </>
    );
  };

  function App() {
    return (
      <>
        <UncontrolledFlow
          onDone={(data) => {
            console.log(data);
            alert("Onboarding Flow Done!");
          }}
        >
          <StepOne />
          <StepTwo />
          <StepThree />
        </UncontrolledFlow>
      </>
    );
  }

  export default App;
  ```