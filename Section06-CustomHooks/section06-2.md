# Design Patterns: CustomHooks

## Fetching a user with Custom Hook
- This is a basic custom hook to fetch data of a current user.

- Note: every custom hook starts with the word 'use'
- The use effect with empty dependencies because I want to fire the request only when the component is rendered
- Make the request from the server and set the user with this value
- Now, the hook can return this value and be used on another component! (See on the example below)

  The custom hook
  ```
  import { useEffect, useState } from "react";
  import axios from "axios";
  export const useCurrentUser = () => {
    const [user, setUser] = useState(null);

    useEffect(() => {
      (async () => {
        const response = await axios.get("/current-user");
        setUser(response.data);
      })();
    }, []);

    return user;
  };
  ```

  Using the custom hook on another component
  ```
  import { useCurrentUser } from "./current-user.hook";

  export const UserInfo = () => {
    const user = useCurrentUser();
    const { name, age, country, books } = user || {};
    return user ? (
      <>
        <h2>{name}</h2>
        <p>Age: {age} years</p>
        <p>Country: {country}</p>
        <h2>Books</h2>
        <ul>
          {books.map((book) => (
            <li key={book}> {book} </li>
          ))}
        </ul>
      </>
    ) : (
      <h1>Loading...</h1>
    );
  };
  ```