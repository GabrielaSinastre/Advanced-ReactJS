# Design Patterns: Container Components

## DataSource Component
- The chapter teach how to take the previous component and make it more generic. In this case, I learned how to pass to the component, the function that will be called inside of that.

  -> The code shows a prop called getData that can be any asynchronous function the component receives. Instead of declaring the route function below, you can pass any function through props as long as it matches this type, meaning it can be any route to fetch any data.

  ```
  import React, { useEffect } from "react";
  import { useState } from "react";

  export const DataSource = ({ getData = () => {}, resourceName, children }) => {
    const [resource, setResource] = useState(null);

    useEffect(() => {
      (async () => {
        const data = await getData();
        setResource(data);
      })();
    }, [getData]);

    return (
      <>
        {React.Children.map(children, (child) => {
          if (React.isValidElement(child)) {
            return React.cloneElement(child, { [resourceName]: resource });
          }
          return child;
        })}
      </>
    );
  };
  ```

  -> Therefore, in the available code within the App, when the component is called, a specific route can be provided, and it can be any utilized route. In this case, if I want to render a route for books, I would only need to set a different URL in the function.

  ```
  import axios from "axios";
  import { BookInfo } from "./components/book-info";
  import { DataSource } from "./components/data-source";
  import { UserInfo } from "./components/user-info";

  const fetchData = async (url) => {
    const response = await axios.get(url);
    return response.data;
  };

  function App() {
    return (
      <>
        <DataSource getData={() => fetchData("/users/1")} resourceName={"user"}>
          <UserInfo />
        </DataSource>
      </>
    );
  }

  export default App;
  ```
  
  

