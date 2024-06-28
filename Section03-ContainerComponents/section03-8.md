# Design Patterns: Container Components

## Local Storage Data Loader Component
- This case we get data from the local storage.

  -> Create a function that receive a key. I'snt going to be an async function because the functionality of getting data from local storage is not async.
  -> The function return a getItem passing the key as a parameter.
  -> The getData function will receive te getDataFromLocalStorage with the key (key of something saved on local storage)
  -> The component Message is used to get the value of the key saved on the local storage.
  -> Pass the resource name 'msg'.
  -> I don't need to create a lot of hooks on the App (parent component), I have created only one reusable function that I passed to the children and they make their own logic data.

  ```
  import axios from "axios";
  import { BookInfo } from "./components/book-info";
  import { DataSource } from "./components/data-source";
  import { UserInfo } from "./components/user-info";

  const fetchData = async (url) => {
    const response = await axios.get(url);
    return response.data;
  };

  const getDataFromLocalStorage = (key) => () => {
    return localStorage.getItem(key);
  };

  const Message = ({ msg }) => <h1>{msg}</h1>;

  function App() {
    return (
      <>
        <DataSource getData={() => fetchData("/users/1")} resourceName={"user"}>
          <UserInfo />
        </DataSource>

        <DataSource getData={() => getDataFromLocalStorage("test")} resourceName={"msg"}>
          <Message />
        </DataSource>
      </>
    );
  }

  export default App;
  ```

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