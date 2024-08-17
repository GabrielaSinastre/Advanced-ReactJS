# Re-renders Explained

## Re-renders of Elements and Components
- *Component:* 
  - Is essentially just a function.
  
    ```
    const Parent = (props) => {
      return <Child />
    }
    ```

  - This function is not just any function: it produces elements that react transforms into DOM elements to display on the screen.
  - If it receives props those are just the functions first arguments

  ```
  const Parent = (props) => {
    return <Child />
  }
  ```

  - Here the function returns the child between these brackets, which is an element of a child component, using those brackets on a component name creates an element of that component.
  
  - So, for example, the element of the parent component using these brackets would look like something like this:
  - ``` <Parent /> ```

- *Element:*
  - An element is basically an object that outlines a component to be displayed.
  - The HTML like syntax is just a simpler way to express what react dot create element does. So, we could simply rewwrite the element, the child element as react dot create element, passing the child instance and null, null, and it would function just as expected.
    ```
    const Parent = (props) => {
      return <Child />
    }

    const Parent = () => {
      return React.createElement(Child, null, null)
    }
    ```

    - Here you can see what the object definition for our child element. So this indicates that the parent component which gives this definition requires rendering the child components without props.
      ```
      <Child />
      
      {
        type: Child,
        props: {},
        ... // a bunch of other React things
      }
      ```
    - The child component then returns it's own definitions, and so forth down the component chain.
    - Elements can also represent simple DOM elements, not just components.
      - for example, our child might return an h1 and h1 tag:
        ```
        const Child = () => {
          return <h2> Some other text </h2>;
        }
        
        // The object of the h2 element

        {
          type: "h1",
          props: {},
          ... // a bunch of other React things
        }
      ```

      - In this scenario, the definition objects behave the same, but the type it's just a string.
    
  - *Re-renders:*
  - What we typically refer to as a re-render involves react executing those functions, which includes processing hooks, among other tasks and stuff.
  - So React then reconstructs a virtual DOM from the returns of these functions and actually building two trees, one before and one after the re-render. And by comparing these trees, a process known as diffing react determines what changes need to be communicated to the browser, the actual DOM, what to remove or add.
  - This process is also called reconciliation algorithm.
  - An important point is: 
    - if the elements okay, the object element before and after the re-render is the same, then react will not re-render the component, this element represents or it's nested components that it returns. So the same means that using the object dot ease function on the child before after re-render returns true. So react doesn't deeply compare object.
    - if true, react leaves that component alone and moves to the next

    ```
    const Parent = () => {
      return <Child />
    }

    //before Parent component re-renders
    const element_before_rerender = {
      type: Child,
      props: {},
      ...
    }

    // Parent re-render gets triggered
    const Parent = () => {
      return <Child />;
    }

    //after Parent component re-rendered
    const element_after_rerender = {
      type: Child,
      props: {},
      ...
    }

    // React performs Diffing
    Object.is(element_before_rerender, element_after_rerender)

    //If the above return true -> re-render Child, otherwise skip it.
    // React perform this comparison on every element that Parent return.
    // In this example, Parent only returns one element, Child.
    ```

    - So if the comparison is false, it signals to react that something has changed. React will then consider the type of the object of the element. If the type remains the same, the component will be re-rendered.
    - If the type of the object changes, react will remove the old component and mount a new one.
  
  - Imagining the parent has some state:
    ```
    const Parent = () => {
      const [state, setState] = useState();

      //Somewwhere here the setState is called, and triggers a re-render of Parent
      
      //Think of the returned <Child /> as an object that is defined locally in the Parent component, so on every re-render of Parent it will be redefined.

      return <Child />
    }

    //         before     after
    Object.is(<Child />, <Child />) //False

    // Everytime Child object is redefined in Parent, it will be different from the old one.
    //Thus, React re-renders the child
    ```
    - When setState is invoked, react knows to re-render the parent. It calls the parent function or the parent functional component and compares the outputs before and after the state change. The result is a locally defined object within the parent function that is recreated each time, leading to an object that is returning false for before and after of the child objects, meaning the child will re-render too, which is now clearly proven for us.

    - Consider what happens if, instead of directly re-rendering the child component, we pass it as a prop at the location where the parent component is rendered, the child definition object is created and passed as a child prop upon state update in the parent
        ```
    const Parent = ({ child }) => {
      const [state, setState] = useState();

      return child;
    }

    const App = () => {
      //Child object is defined here in the App component and a reference to it is passed to the Parent component.
      return <Parent child={<Child />}/>
    }

    //When Parrent re-render, child object will not be redefined. Thus:

    
    
    //        before after
    Object.is(child, child) //True
    ```
    - React compares the returned object before and after the state change. Here, the child object created outside the parent function scope and unchanged on calls, which leads to a true comparison results prompting to skip Re-rendering this component. This is exactly what happened in our little app that we saw  in the last in the previous lesson. 
      - So when set position in this component (DynamicScroll), the dynamic scroll triggered and re-render happens. React examine all the returned object definitions which we have this dynamic block the scrollable containeer and the content are passed as props, but it see that the content object remains the same before and after this content object that it renders. It will see that using the object that is will return true for this content and it keeps the re-render of the slow components that are inside this component.
      - However, thee dynamic block inside this scrollable container will re-render because it is directly defined inside this dynamic scroll. So, every time it will be redefined and the comparison for it will return false.
      - So, this dynamic block is created within the dynamic scroll component and on each re-render of the dynamic scroll it will have a false comparison which results in a re-render of this dynamic block.
