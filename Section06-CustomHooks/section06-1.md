# Design Patterns: CustomHooks

## Introduction
- Custom hooks are hooks that we create by combining the basic hooks provided by React.
- Example: we want our component to fetch users information from a server.
  We could either load the users within the component itself, or we could create a custom hook called useUsers
that handles the data loading encapsulates the functionality.

- In our components, we simply call the custom hook and assign it's return value to a variable.
- It's important to note: that custom hooks must begin with the word *use*, as this is a requirement imposed by React.

- Just like HOC anc Container Components, custom hooks serve to a similar purpose.
  - They allow us to share complex behavior among multiple components.


