# Design Patterns: Higher Order Components (HOCs)

## Introduction
- Are components that instead of directly return JSX, return another component.
  - Most components simply return JSX, wichh represents the DOM elements to be rendered in their place. However, with higher order components, we introduce an additional layer instead of directly returning JSX, return another component that in turn return JSX.
- Remember: Higher order components are esseentially functions that return components
  - Think of them as component factories, functions that generate new components when they are called.

- Why would you want to create a HOCs?
  - Improve existing components without modifying their code.
  - Enable us to share behavior among multiple components (similar to container components).
  - Provide a means to achieve similar functionality for sharing the relative logic.
  - Additionally, HOCs allow us to add extra functionality to existing components.



