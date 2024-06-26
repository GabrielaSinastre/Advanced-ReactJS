# Design Patterns: Container Components

## Loader Component for Resource Data
- The chapter teach how to take the previous component and make a more generic variant. In case, we create a component that can render different things: user or books, passing how url and name on the component props.
- Observation: the component is more generic than the previous one (section03-4) but have the same purpose.

- The component explanation:
  -> Make more generic and creating a container component that is used for fetching more than a specific data source, the user, for example, so that we can use it for featching, for example user data or the books data.
  -> For doing so, take the code:

  ```
  import axios from "axios";
  import React, { useEffect } from "react";
  import { useState } from "react";

  export const ResouceLoader = ({ resouceUrl, resourceName, children }) => {
    const [resource, setResource] = useState(null);

    useEffect(() => {
      (async () => {
        const response = await axios.get(resouceUrl);
        setResource(response.data);
      })();
    }, [resouceUrl]);

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

  -> The prop is not the idBook or the userId because we can render a complete resource, not only a single user/book. To get this, we pass the resourceName too, so we can know what the resource will be rendered with.
  
  Look the app code:

  ```
  import axios from "axios";
  import { BookInfo } from "./components/book-info";
  import { UserInfo } from "./components/user-info";

  function App() {
    return (
      <>
        <ResouceLoader resouceUrl={"/users/1"} resourceName={"user"}>
          <UserInfo />
        </ResouceLoader>

        <ResouceLoader resouceUrl={"/books/1"} resourceName={"book"}>
          <BookInfo />
        </ResouceLoader>
      </>
    );
  }

  export default App;
  ```

  -> The first case: the resource loader need to get an URL and a resource name. So pass /users/2 as a URL and user as a resource name.
  -> The second case: the resource loader need to get an URL and a resource name. So pass /books/1 as a URL and book as a resource name.

