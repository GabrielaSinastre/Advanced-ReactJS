# Design Patterns: Controlled and Uncontrolled Components

## Uncontrolled Flows
- This example is going to be about flows or more specific onboarding flows.
- Onboarding flow -> if you want to register or fill in a form like it's going to have several steps in each step, it ask for a specific information, a next or a previous button you click, you can go next and previous. Each step is going to grab some data from the client till you reach the last step, the final step of that flow, wich will finish the flow of it.

-> Two states
  - First one: data, set data -> the data we are going to collect during the course of the steps.
  - Second one: Keeping track of the current index.


import React, { useState } from "react";

export const UncontrolledFlow = ({ children, onDone }) => {
  const [data, setData] = useState({});
  const [currentStepIndex, setCurrentStepIndex] = useState(0);

  const currentChild = React.Children.toArray(children)[currentStepIndex];

  const next = () => {
    setCurrentStepIndex(currentStepIndex + 1);
  };

  if (React.isValidElement(currentChild)) {
    return React.cloneElement(currentChild, { next });
  }

  return currentChild;
};


import { UncontrolledFlow } from "./components/uncontrolled-flow";

const StepOne = ({ next }) => {
  return (
    <>
      <h1>Step #1</h1>
      <button onClick={next}>Next</button>
    </>
  );
};
const StepTwo = ({ next }) => {
  return (
    <>
      <h1>Step #2</h1>
      <button onClick={next}>Next</button>
    </>
  );
};
const StepThree = ({ next }) => {
  return (
    <>
      <h1>Step #3</h1>
      <button onClick={next}>Next</button>
    </>
  );
};

function App() {
  return (
    <>
      <UncontrolledFlow>
        <StepOne />
        <StepTwo />
        <StepThree />
      </UncontrolledFlow>
    </>
  );
}

export default App;


