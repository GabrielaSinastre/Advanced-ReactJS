# Design Patterns: Functional Programming in React

## Recursive Components
- The recursive patter (recursive component) is a component that calls itself from inside itself.

- Let's the case that we have an object as a data and you want to show the values. This object can have only one key with a string value or a object inside itself, or the object itself can have both.
- So, we need to figure out a way how to use the recursive pattern for looping through the elements and thee keys and then displaying the values of each key.

  ```
  const myNestedObject = {
    key1: "value1",
    key2: {
      innerKey1: "innerValue1",
      innerKey2: {
        innerInnerKey1: "innerInnerValue1",
        innerInnerKey2: "innerInnerValue2",
      },
    },
    key3: "value3",
  };
  ```

  - First, create a helper function that helps to determine whether the data is an object or is not an object.

    ```
    const isValidObj = (data) => typeof data === "object" && data !== null;
    ```
  - For example, the key1 value is not an object and this function we can see this.

  *Note:*
  - Every recursive component needs a stopping condition. Means that there is a condition if that condition is met, the recursive component in that case is not going to call itself for this case.
  - On the example, the condition: isObject:
    - If the data that this recursive component is receiving will not be an object, it's a null or it's something other than object (string, number, anything), i'ts going to display using a <li>, a list item tag.
  - But if it passes this if which means data is an object, we are going to continue the recursion!

  - For the recursive condition, we are going to look through the keys and values of the data because it's an object.
  - For every pair of the key value pair of this data, we are going to call the recursive component.
  - In this strategy, for start looping through the key value pairs of this data, we need to call a piece of Javascript code that is simply saying const pairs.
    ```
    Object.entries(data)
    ```
  - Into fragments component, return the pairs making a map from the pairs.
    - For every key and value we are going to display.
      - So, the value can be an object too, the Recursive Component becomes actually a recursive component, so, for example, inside the <ul> because we are showing a list, pass the component itself <RecursiveComponent />
  
  - So, we have the recursive component:
  ```
  const isValidObj = (data) => typeof data === "object" && data !== null;

  export const Recursive = ({ data }) => {
    if (!isValidObj(data)) {
      return <li>{data}</li>;
    }

    const pairs = Object.entries(data);
    console.log(data);
    return (
      <>
        {pairs.map(([key, value]) => {
          return (
            <li>
              {key}:
              <ul>
                <Recursive data={value} />
              </ul>
            </li>
          );
        })}
      </>
    );
  };
  ```

  ```
  function App() {
    return (
      <>
        <Recursive data={myNestedObject} />
      </>
    );
  }
  ```