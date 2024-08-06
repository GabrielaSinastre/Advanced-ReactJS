# Design Patterns: Higher Order Components (HOCs)

## Updating Data with HOC
- Create an HOC that additional to fetching data from the server. It can post or edit data on the server.
  - Set initial user 
  - Set user
  - Get the current user from the server passing the user id and set user with this data
  - Return the <Component> passing the user prop
  - After, create the put and post function
    - onChangeUser
      *Receive a prop called updates
      *Set the user with the current user data with the updated data
      *Pass the function as a prop on the Component
    - onPostUser
      *Create a axios post passing the url and the user data into the body of the request
      *Set the initial user with the current user data setted on the server 
      *Pass the function as a prop on the Component
  - Create a reset function
    - This function is going to take caree of updating this state with the value of the initial user.

  ```
  import { useEffect, useState } from "react";
  import axios from "axios";

  export const includeUpdatableUser = (Component, userId) => {
    return (props) => {
      const [user, setUser] = useState(null);
      const [updatableUser, setUpdatableUser] = useState(null);

      useEffect(() => {
        (async () => {
          const response = await axios.get(`/users/${userId}`);
          setUser(response.data);
          setUpdatableUser(response.data);
        })();
      }, []);

      const userChangeHandler = (updates) => {
        setUpdatableUser({ ...updatableUser, ...updates });
      };

      const userPostHandler = async () => {
        const response = await axios.post(`/users/${userId}`, {
          user: updatableUser,
        });
        setUser(response.data);
        setUpdatableUser(response.data);
      };

      const resetUserHandler = () => {
        setUpdatableUser(user);
      };

      return (
        <Component
          {...props}
          updatableUser={updatableUser}
          changeHandler={userChangeHandler}
          userPostHandler={userPostHandler}
          resetUserHandler={resetUserHandler}
        />
      );
    };
  };
  ```