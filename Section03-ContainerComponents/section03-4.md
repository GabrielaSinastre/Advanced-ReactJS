# Design Patterns: Container Components

## Loader Component for User Data
- The chapter teach how to take the previous component and make a variant. In case, instead of taking a current user, now I need to pass an userId to fetch.
- Observation: the parent component can pass the id as a prop too.

  ```
  import axios from "axios";
  import React, { useEffect } from "react";
  import { useState } from "react";

  export const UserLoader = ({ userId, children }) => {
    const [user, setUser] = useState(null);

    useEffect(() => {
      (async () => {
        const response = await axios.get(`/users/${userId}`);
        setUser(response.data);
      })();
    }, [userId]);

    return (
      <>
        {React.Children.map(children, (child) => {
          if (React.isValidElement(child)) {
            return React.cloneElement(child, { user });
          }
          return child;
        })}
      </>
    );
  };
  ```

  In app:
  ```
  import { UserInfo } from "./components/user-info";
  import { UserLoader } from "./components/user-loader";

  function App() {
    return (
      <>
        <UserLoader userId={"1"}>
          <UserInfo />
        </UserLoader>

        <UserLoader userId={"2"}>
          <UserInfo />
        </UserLoader>

        <UserLoader userId={"3"}>
          <UserInfo />
        </UserLoader>
      </>
    );
  }

  export default App;

  ```

