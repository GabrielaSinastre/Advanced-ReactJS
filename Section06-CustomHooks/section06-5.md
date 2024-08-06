# Design Patterns: CustomHooks

## More Generic Custom Hook
- In this topic we are going to make this resource hook more generic!
- So that it is not going to be aware of this source of data, so can be use it for multiple data sources.

- Instead of the resource URL as an argument, we are going to receive a function that whenever we call it in here, it's going to start getting the data from the corresponding source it has.

  So, the custom hook:
  ```
  import { useEffect, useState } from "react";
  import axios from "axios";
  export const useDataSource = (getData = () => {}) => {
    const [resource, setResource] = useState(null);

    useEffect(() => {
      (async () => {
        const data = await getData();
        setResource(data);
      })();
    }, [getData]);

    return resource;
  };
  ```

  *Using the custom hook to get a user resource*
  - Create variations of the function one fetches from server, one fetches from local storage. This basically going to receive the resource url as an input.
  - Pass the function as an argument on the custom hook useDataSource!
  - Every time you want get some information from server you can create a new function and pass as a different argument.

  ```
  import { useCallback } from "react";
  import { useDataSource } from "./data-source.hook";
  import axios from "axios";

  const fetchFromServer = (url) => async () => {
    const response = await axios.get(url);
    return response.data;
  };

  const getFromLocalStorage = (key) => () => {
    return localStorage.getItem(key);
  };

  export const UserInfo = ({ userId }) => {
    const fetchUser = useCallback(fetchFromServer(`/users/${userId}`), [userId]);
    const user = useDataSource(fetchUser);
    const message = useDataSource(getFromLocalStorage("msg"));
    const { name, age, country, books } = user || {};
    console.log("Am I rendering?");
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

  *Note:*
  - Observe that the useCallback hook is used to prevent multiple calls rendering!