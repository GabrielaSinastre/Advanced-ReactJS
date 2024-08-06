# Design Patterns: CustomHooks

## Fetching users with Custom Hook
- Now, I learned how to fetch a user by your id using a custom hook.
- Observe that now I put the useEffect dependency, because every time that the user id changes I need to show the current user

  The custom hook
  ```
  import { useEffect, useState } from "react";
  import axios from "axios";
  export const useUser = (userId) => {
    const [user, setUser] = useState(null);

    useEffect(() => {
      (async () => {
        const response = await axios.get(`/users/${userId}`);
        setUser(response.data);
      })();
    }, [userId]);

    return user;
  };
  ```
  
  Using the custom hook on another component
  ```
  import { useUser } from "./user.hook";

  export const UserInfo = ({userId}) => {
    const user = useUser(userId);
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