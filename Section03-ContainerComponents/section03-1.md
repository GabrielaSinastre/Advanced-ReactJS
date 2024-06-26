# Design Patterns: Container Components

## Introduction
-> Are react components responsible for handling data loading and data management on behalf of their child components.

  -> Consider the code:
  ```
  const Component = () => {
    //Manage Data
    const [data, setData] = useState();

    useEffect(()=>{...}, [])

    return (
      //Display Data in JSX.
    )
  }
  ```

  -> The challenge arises when multiple child components need to share the same logic for data loading, and the container components come to fix this.

  -> They address this issue by extracting the data loading into a dedicated component.
  -> The container, wich then handles the data retrieval process and automatically passes it down to the child components. 
  
  ```
  const Container - () => {
    //Manage Data
    const [data, setData] = useState();

    //Load Data
    useEffect(()=>{...},[])

    return (
      //Pass data to children as props
      <ChildComponent data={data} />
    )
  }
  ```
  
  -> Soon, we'll dive into the specifics of how container components achieve this.

  *Concept behind container components:*
  -> Similar to layout components:
  **Components are unaware of the source or management of their data;
    -> Where we aim to shield child components from being aware of specific layout they are part of.
  -> Instead, they should simply receive props and display the relevant content without any knowledge of the underlying data handling.