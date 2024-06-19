# Design Patterns: Layout Components

## Lists
- Talk about list and list items and how to deal with them as a layout component;

  Auxiliar data
  ```
  export const authors = [
    {
      name: "Sarah Waters",
      age: 55,
      country: "United Kingdom",
      books: ["Fingersmith", "The Night Watch"],
    },
    {
      name: "Haruki Murakami",
      age: 71,
      country: "Japan",
      books: ["Norwegian Wood", "Kafka on the Shore"],
    },
    {
      name: "Chimamanda Ngozi Adichie",
      age: 43,
      country: "Nigeria",
      books: ["Half of a Yellow Sun", "Americanah"],
    },
  ];
  ```
  
  Different lists to render
  ```
  export const LargeAuthorListItem = ({ author }) => {
    const { name, age, country, books } = author;
    return (
      <>
        <h2>{name}</h2>
        <p>Age: {age}</p>
        <p>Country: {country}</p>
        <h2>Books</h2>
        <ul>
          {books.map((book) => (
            <li key={book}> {book} </li>
          ))}
        </ul>
      </>
    );
  };


  export const SmallAuthorListItem = ({author}) => {
    const {name, age} = author;
    return(
      <p> Name: {name}, Age: {age}</p>
    )
  }
  ```
  
  RegularList - to render the different lists  
  ```
  export const RegularList = ({ items, sourceName, ItemComponent }) => {
    return (
      <>
        {items.map((item, i) => (
          <ItemComponent key={i} {...{ [sourceName]: item }} />
        ))}
      </>
    );
  };
  ```

  App - to pass the props and render de RegularList
  ```
  import { LargeAuthorListItem } from "./components/authors/LargeListItems";
  import { SmallAuthorListItem } from "./components/authors/SmallListItems";
  import { RegularList } from "./components/lists/Regular";
  import { authors } from "./data/authors";

  function App() {
    return (
      <>
        <RegularList
          items={authors}
          sourceName={"author"}
          ItemComponent={SmallAuthorListItem}
        />
        <RegularList
          items={authors}
          sourceName={"author"}
          ItemComponent={LargeAuthorListItem}
        />
      </>
    );
  }

  export default App;
  ```

 - This list belongs to the Layout Component design pattern for several reasons. Layout Components are components that handle the structure and organization of the application layout but not the business logic. They are responsible for rendering other components in a consistent and reusable manner.

  -> Responsibility for Structure: The RegularList is responsible for defining how the list items are rendered. It doesn't care about the specific content of the items but rather about the structure of how each item should be rendered.

  -> Component Reusability: The RegularList allows the reuse of item components (LargeAuthorListItem and SmallAuthorListItem) in different contexts. This facilitates maintenance and scalability, as the presentation logic for the items is encapsulated in these specific components.

  -> Simplicity and Flexibility: The RegularList is a generic component that can render any type of list item, as long as an item component (ItemComponent) is provided. This promotes simplicity and flexibility, as the same list component can be used for different types of data.

  -> Decoupling: The RegularList decouples the logic of how the data is structured from the logic of how the data is rendered. This means that the layout logic and the business logic remain separate, making the code easier to read and maintain.

  **Code Explanation**

  *RegularList Component:*
  Props:
  -> items: The list of items to be rendered.
  -> sourceName: The name of the property used to pass the item data to the item component.
  -> ItemComponent: The component used to render each item.
  -> Functionality: The RegularList maps over the list of items (items) and renders an ItemComponent for each item, passing the item data as props.

  *Item Components:*
  -> LargeAuthorListItem and SmallAuthorListItem are examples of item components that receive author data and render it in different ways.
  -> LargeAuthorListItem: Renders full details of the author, including a list of books.
  -> SmallAuthorListItem: Renders only the author's name and age.

  *Usage in App:*
  -> The App component uses RegularList twice, once to render the list of authors with SmallAuthorListItem and once to render the list of authors with LargeAuthorListItem.

  **Conclusion**
  -> This structure allows for great flexibility and reusability, which are key characteristics of a Layout Component. The layout logic is separated from the presentation logic of the data, making the code easier to maintain and expand. The RegularList can be easily adapted to other types of lists, making it a powerful tool for organizing layout in the application.
