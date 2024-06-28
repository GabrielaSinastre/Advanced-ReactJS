# Design Patterns: Controlled and Uncontrolled Components

## Controlled Flows
- Now he will have a controlled flow
- Instead of defining the goNext function inside of the child component, we are going to receive it as a function (create the function goNext on the App and pass to this prop onNext).
- The current step is NOT going to be controlled from inside the child component, so pass the currentIndex as a prop.
- All the states are going to be controlled from the parent component!

**Benefits**: 
  -> Imagine that we have another step in here (step 4 on the code). If I want to hide the step 3 if the age is beyond than 20 years old, I can do this checking if data.age value that I got on step 2. So now I have a criteria to show the step based on a controlled component, in this case, App control their children.


  ```
  import React, { useState } from "react";

  export const ControlledFlow = ({
    children,
    onDone,
    currentStepIndex,
    onNext,
  }) => {
    const next = (data) => {
      onNext(data);
    };

    const currentChild = React.Children.toArray(children)[currentStepIndex];

    if (React.isValidElement(currentChild)) {
      return React.cloneElement(currentChild, { next });
    }

    return currentChild;
  };
  ```

  ```
  import { useState } from "react";
  import { UncontrolledFlow } from "./components/uncontrolled-flow";
  import { ControlledFlow } from "./components/controlled-flow";

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
        <button onClick={() => next({ age: 30 })}>Next</button>
      </>
    );
  };
  const StepThree = ({ next }) => {
    return (
      <>
        <h1>Step #3: You qualify!</h1>
        <button onClick={() => next({})}>Next</button>
      </>
    );
  };

  const StepFour = ({ next }) => {
    return (
      <>
        <h1>Step #4: Enter your country</h1>
        <button onClick={() => next({ country: "Poland" })}>Next</button>
      </>
    );
  };

  function App() {
    const [data, setData] = useState({});
    const [currentStepIndex, setCurrentStepIndex] = useState(0);

    const next = (dataFromStep) => {
      setData(dataFromStep);
      setCurrentStepIndex(currentStepIndex + 1);
    };

    return (
      <>
        <ControlledFlow currentStepIndex={currentStepIndex} onNext={next}>
          <StepOne />
          <StepTwo />
          {data.age > 25 && <StepThree />}
          <StepFour />
        </ControlledFlow>
      </>
    );
  }

  export default App;
```
