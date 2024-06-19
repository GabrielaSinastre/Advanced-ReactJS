# Design Patterns: Layout Components

## Screen Splitter Enhancement
- The chapter cames to show how to organize the components to the best behavior of Screen Splitter.
- You can simplify components so they remain simple yet reusable, without requiring many props.

-> The example below demonstrates the exercise from the previous section, but with better organization. For instance, when you need to only increase the size of a component or add a title or similar attribute to a child, you can do it this way. Note that the child component has also been destructured.


const Container = styled.div`
  display: flex;
`;

const Panel = styled.div`
  flex: ${(p) => p.flex};
`;

 The component code:
 ```
  export const SplitScreen = ({ children, leftWidth = 1, rightWidth = 1 }) => {
    const [left, right] = children;
    return (
      <Container>
        <Panel flex={leftWidth}>{left}</Panel>
        <Panel flex={rightWidth}>{right}</Panel>
      </Container>
    );
  }
  ```
  App code:
  ```
    const LeftSideComp = ({title}) => {
      return <h2 style={{ backgroundColor: "crimson" }}>{title}</h2>;
    };

    const RightSideComp = ({title}) => {
      return <h2 style={{ backgroundColor: "burlywood" }}>{title}</h2>;
    };

    function App() {
      return (
        <SplitScreen leftWidth={1} rightWidth={3}>
          <LeftSideComp title={'Right'}/>
          <RightSideComp title={'Left'}/>
        </SplitScreen>
      );
    }

  export default App;
  ```
