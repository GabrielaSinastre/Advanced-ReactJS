# Design Patterns: Layout Components

## Screen Splitter
- First Screen Splitter example

  -> First: create a component on a file called split-screen
  -> This component will receive two props: Left and Right

  The component code:
  ```
  export const SplitScreen = ({ Left, Right }) => {
    return (
      <div>
        <div>
          <Left />
        </div>

        <div>
          <Right />
        </div>
      </div>
    )
  }
  ```
  The same component can be used with styled-component:
  ```
  const Containerrr = styled.div`
    display: flex;
  `;

  const Panel = styled.div`
    flex: 1;
  `;

  export const SplitScreen = ({ Left, Right }) => {
    return (
      <Container>
        <Panel>
          <Left />
        </Panel>

        <Panel>
          <Right />
        </Panel>
      </Container>
    )
  }
  ```
  When you call this component, you can do:
  ```
  const LeftSideComp = () => {
    return (
      <h2>I AM LEFT!</h2>
    )
  }

  const RightSideComp = () => {
    return (
      <h2>I AM RIGHT!</h2>
    )
  }

  function App() {
    return (
      <SplitScreen Left={LeftSideComp} Right={RightSideComp}>
    )
  }
  ```