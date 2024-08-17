# Re-renders Explained

## Locating another issue
- Let's dive deeper to understand re-renders and their impact on performance by exploring a more challenging scenario.

  ```
  import { ReactNode, useState } from "react";
  import styled from "styled-components";

  const DynamicBlock = styled.div<{ top: number; color: string }>`
    position: ${(props) => (props.top === 113 ? "fixed" : "absolute")};
    top: ${(props) => (props.top === 113 ? "0.2rem" : `${props.top}px`)};
    left: 1rem;
    background: ${(props) => props.color};
    width: 3rem;
    height: 3rem;
    line-height: 3rem;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    font-weight: bold;
    font-size: 20px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  `;

  const ScrollableContainer = styled.div`
    width: 25rem;
    height: 12rem;
    overflow: auto;
    border: 1px solid rgba(128, 128, 128, 0.5);
    position: relative;
    z-index: 1;
    background: #f5f5f5;
    padding: 1rem;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  `;

  const calculatePosition = (scrollValue: number) => 170 - scrollValue / 2;

  const calculateColor = (position: number) => {
    const normalizedPosition = Math.min(Math.max(position, 0), 255);
    return `rgb(${normalizedPosition}, ${255 - normalizedPosition}, 150)`;
  };

  const DynamicScroll = ({ content }: { content: ReactNode }) => {
    const [position, setPosition] = useState(170);

    const handleScroll = (e: React.UIEvent<HTMLDivElement>) => {
      const newPosition = calculatePosition(e.currentTarget.scrollTop);
      setPosition(Math.max(113, newPosition));
    };

    const blockColor = calculateColor(position);

    return (
      <ScrollableContainer onScroll={handleScroll}>
        <DynamicBlock top={position === 113 ? 113 : position} color={blockColor}>
          🛒
        </DynamicBlock>
        {content}
      </ScrollableContainer>
    );
  };

  export default DynamicScroll;
  ```

  ```
  import { SlowComponent } from "./components/slow-component";
  import { AdditionalComplexThings, BlaBla } from "./components/dummy-components";
  import DynamicScroll from "./components/dynamic-scroll";

  export default function App() {
    return (
      <DynamicScroll
        content={
          <>
            <SlowComponent />
            <BlaBla />
            <AdditionalComplexThings />
          </>
        }
      />
    );
  }
  ```

  - This code has no lags when is running, because we passed all the other components (slow...) indirectly as a prop.
  - In this case regarding the state updates, this position, state and all the re-renders that it causes.
  - If a state updates is triggered, it will not now only cause the dynamic scroll component a simple div with a movable block to render every time. But this lower components that are passed through props remain unaffected in the parent component which is the app component.
  - Remember: react does not trigger re-renders up the component tree, so slower components won't re-render when the state updates, ensuring smooth lag free scrolling.
  - However, it might seem confusing at first, these components in somehow are being rendered inside our scrollable container, but the fact is that they are not actually declared or defined inside the DinamycScroll, they are declared and defined in the parent component, which is the app component, and they are only accessed by a reference to them.
