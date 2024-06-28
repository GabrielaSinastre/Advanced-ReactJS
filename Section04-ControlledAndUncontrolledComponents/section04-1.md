# Design Patterns: Controlled and Uncontrolled Components

## Introduction
- A fundamental react design pattern;

- Uncontrolled components: 
  -> The component itself manages it's own internal state, and the data within the component is tipically accessed only when specific events happen. Common example: is an uncontrolled form where the values of the form inputs are known to the outside components only when the user triggers the submit component.

-  components:
  -> The parent component is responsible for managing the state which is then passed down to the control component.
  -> The parent component handles the state and controls the behavior of the controlled component.

**Question:** Which approach should we prefer: controlled or uncontrolled components?
In most cases, **controlled components** are the preferred choice. There are several reasons for this preference. First of all, controlled components tend to be more usable and easier to test with controlled components. 
Controlled components can be easily set up the component with the desired state for testing purposes. This eliminates the need to manually manipulate the components and trigger events to examine it's internal behavior.
