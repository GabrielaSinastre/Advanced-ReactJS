# Design Patterns: CustomHooks

## Fetching a Resource with Custom Hook
- Now, making the previous example more generic and make it usable for actually fetching data from different resources, not just for the user.

  The generic custom hook
  ```
  import { useEffect, useState } from "react";
  import axios from "axios";
  export const useResource = (resouceUrl) => {
    const [resource, setResource] = useState(null);

    useEffect(() => {
      (async () => {
        const response = await axios.get(resouceUrl);
        setResource(response.data);
      })();
    }, [resouceUrl]);

    return resource;
  };
  ```

  Using the custom hook to get a user resource
  ```
  import { useResource } from "./resource.hook";
  import { useUser } from "./user.hook";

  export const UserInfo = ({ userId }) => {
    const user = useResource(`/users/${userId}`);
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

  Using the hook to get a book resource
  ```
  import { useResource } from "./resource.hook";

  export const BookInfo = ({ bookId }) => {
    const book = useResource(`/books/${bookId}`)
    const { name, price, title, pages } = book || {};

    return book ? (
      <>
        <h3>{name}</h3>
        <p>{price}</p>
        <h3>Title: {title}</h3>
        <p>Number of Pages: {pages}</p>
      </>
    ) : (
      <h1>Loading</h1>
    );
  };
  ```
