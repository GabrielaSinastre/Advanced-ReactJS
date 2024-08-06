# Design Patterns: Higher Order Components (HOCs)

## Data Loading with HOC
- Using the HOC for fetching data from server (similiar to the container components)

  - create a component thet is going to wrap this user info and going to take care of fetching the data from the server and then passing it as a prop to this user info, so that we can display it in here using this user info.
  - includeUser reeceive as props: the component will be rendered and the id of the user will be used to get the data from the server
  - return the component if their original props in case it has
  - pass a new prop: the user that you got from the server 

  ```
  import { useEffect, useState } from "react";
  import axios from "axios";

  export const includeUser = (Component, userId) => {
    return (props) => {
      const [user, setUser] = useState(null);

      useEffect(() => {
        (async () => {
          const response = await axios.get(`/users/${userId}`);
          setUser(response.data);
        })();
      });

      return <Component {...props} user={user} />;
    };
  };
  ```

  - On the app.js, use the UserInfo as a argument on the includeUser method, and pass the user id.

  ```
  import { checkProps } from "./components/check-props";
  import { includeUser } from "./components/include-user";
  import { UserInfo } from "./components/user-info";

  const UserInfoWithUser = includeUser(UserInfo, "2");

  function App() {
    return (
      <>
        <UserInfoWithUser />
      </>
    );
  }

  export default App;
  ```