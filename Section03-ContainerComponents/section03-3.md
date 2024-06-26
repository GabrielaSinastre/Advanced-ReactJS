# Design Patterns: Container Components

## Loader Component for CurrentUser Data
- In this case we have a component that can be have childrens.
  -> This component have the hability to access the children inside this current user loader or actually to use the funcionalities related to the children. 
  -> Why use map: to feed it the actual children which is accessed using the children props in here. Then, for every child inside this current user loader, I want to do some funcionality:
    -> First: to do a validation -> React valid element (React.isValidElement)
      -> in case the child is a valid element, I want to do something -> return that child (React.cloneElement)! 
        *cloneElement help to attach some extra props to the element which the child is. The parameters: first value is the child and the second value is the props themselves (in case, I want to pass user data).
      -> in case it's not a valid element, simply return the child without any props.

  ```
  import axios from "axios";
  import React, { useEffect } from "react";
  import { useState } from "react";

  export const CurrentUserLoader = ({ children }) => {
    const [user, setUser] = useState(null);

    useEffect(() => {
      (async () => {
        const response = await axios.get("/current-user");
        setUser(response.data);
      })();
    }, []);

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

  - So, basically in here (below) we have only one child in here. The user info, perhaps we have several children inside of it. 
  - This is basically going to map, it's going to loop through the children and we can do some modifications to that child. And here I simply attached a prop to every child, so this user is going to be passed and can be consumed inside this user info, which is in {user} -> const UserInfo ({user}) => {}

  ```
  import { CurrentUserLoader } from "./components/current-user-loader";
  import { UserInfo } from "./components/user-info";

  function App() {
    return (
      <>
        <CurrentUserLoader>
          <UserInfo />
        </CurrentUserLoader>
      </>
    );
  }

  export default App;
  ```