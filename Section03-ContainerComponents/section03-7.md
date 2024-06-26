# Design Patterns: Container Components

## Container Component with Render Props Pattern
- Obs: When cloning elements and React children, you should avoid using them indiscriminately or in simple components because they can lead to performance issues and increase the complexity of your code unnecessarily. Such a scenario used in this chapter of course, this is completely natural for using because we are taking care of the maintainability and that is because we are simply splitting or isolating the logics inside a container component, which is a pattenr for itself!

  -> But keeping that in mind and have a alternative way whithout use clone component but using this piece of code, is there like another syntax that we can use simply for passing this resource data thing down to the children. But the thing is that requires to use another design pattern or react pattern.
  So, this chapter is to demonstrate an alternative way to this piece of code.

  -> To start, we make differences on the App.js.
  -> Instead to pass de UserInfo component like a child component, we can create a function and call it render, and inside of it we have just a simple function that receives a prop. The prop can be called a 'resource' (any data we want to pass down the children) and returns a component to render.

  ```
  import axios from "axios";
  import { DataSourceWithRenderProps } from "./components/data-source-with-render-props";
  import { UserInfo } from "./components/user-info";

  const fetchData = async (url) => {
    const response = await axios.get(url);
    return response.data;
  };

  function App() {
    return (
      <>
        <DataSourceWithRenderProps
          getData={() => fetchData("/users/1")}
          render={(resource) => <UserInfo user={resource} />}
        ></DataSourceWithRenderProps>
      </>
    );
  }

  export default App;
  ```

  -> Inside of the component, we are not going to need the resource name or the children because simply the resource name is being passed up on App.js already.
  -> Instead of the children prop, that we don't need to access to them directly, we pass the render function.
  -> Also, instead of having all the clone component code, we can simply returning render, passing the resource.

  ```
  import { useEffect } from "react";
  import { useState } from "react";

  export const DataSourceWithRenderProps = ({ getData = () => {}, render }) => {
    const [resource, setResource] = useState(null);

    useEffect(() => {
      (async () => {
        const data = await getData();
        setResource(data);
      })();
    }, [getData]);

    return render(resource);
  };
  ```
