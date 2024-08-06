# Design Patterns: Higher Order Components (HOCs)

## Enhancing HOC Pattern
- This chapter have the goal create a generic HOC pattern using the previously component, modifying them to be a more generic HOC component.
- The previous component created only to render users, now that the component will be created to render any other component: books, user or whatever I want.

  - Rename the previous component name, functions from Resource instead of user
  - Pass a new prop: resourceName to component know what component will be rendered and the resourceUrl to render~
  - Implement a way that I can pass the props with the proper name for those props based on the resource name that we are receiving, including the onChange property.
    - So, create de the resourceProps, to be a dinamic way!

  ```
  import { useEffect, useState } from "react";
  import axios from "axios";

  const toCapital = (str) => str.charAt(0).toUpperCase() + str.slice(1);

  export const includeUpdatableResouce = (
    Component,
    resourceUrl,
    resourceName
  ) => {
    return (props) => {
      const [data, setData] = useState(null);
      const [updatableData, setUpdatableData] = useState(null);

      useEffect(() => {
        (async () => {
          const response = await axios.get(resourceUrl);
          setData(response.data);
          setUpdatableData(response.data);
        })();
      }, []);

      const changeHandler = (updates) => {
        setUpdatableData({ ...updatableData, ...updates });
      };

      const dataPostHandler = async () => {
        const response = await axios.post(resourceUrl, {
          [resourceName]: updatableData,
        });
        setData(response.data);
        setUpdatableData(response.data);
      };

      const resetHandler = () => {
        setUpdatableData(data);
      };

      const resourceProps = {
        [resourceName]: updatableData,
        [`onChange${toCapital(resourceName)}`]: changeHandler,
        [`onSave${toCapital(resourceName)}`]: dataPostHandler,
        [`onReset${toCapital(resourceName)}`]: resetHandler,
      };

      return <Component {...props} {...resourceProps} />;
    };
  };
  ```