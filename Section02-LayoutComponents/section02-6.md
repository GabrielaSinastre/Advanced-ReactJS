# Design Patterns: Layout Components

## Lists Types
- This chapter tells us that with only 6 components, we can have several variations, which makes a significant difference in a large project.
- It is observed that the lists are similar to the example in the previous chapter and render distinct components in the same way, with the same props.

  Auxiliar data
  ```
  export const books = [
    {
      name: "To Kill a Mockingbird",
      pages: 281,
      title: "Harper Lee",
      price: 12.99,
    },
    {
      name: "The Catcher in the Rye",
      pages: 224,
      title: "J.D. Salinger",
      price: 9.99,
    },
    {
      name: "The Little Prince",
      pages: 85,
      title: "Antoine de Saint-Exupéry",
      price: 7.99,
    },
  ];
  ```
  
  Different lists to render
  ```
  export const LargeBookListItem = ({ book }) => {
    const { name, price, title, pages } = book;

    return (
      <>
        <h2>{name}</h2>
        <p>{price}</p>
        <h2>Title:</h2>
        <p>{title}</p>
        <p># of Pages: {pages}</p>
      </>
    );
  };

  export const SmallBookListItem = ({book}) => {
    const {name, price} = book;
    return (
      <h2>{name} / {price}</h2>
    )
  }
  ```
  
  NumberedList - to render the different lists - like regular list of the another example
  ```
  export const NumberedList = ({ items, sourceName, ItemComponent }) => {
    return (
      <>
        {items.map((item, i) => (
          <>
            <h3> {i + 1} </h3>
            <ItemComponent key={i} {...{ [sourceName]: item }} />
          </>
        ))}
      </>
    );
  };
  ```

  App - to pass the props and render de RegularList
  ```
  import { LargeAuthorListItem } from "./components/authors/LargeListItems";
  import { SmallAuthorListItem } from "./components/authors/SmallListItems";
  import { LargeBookListItem } from "./components/books/LargeListItems";
  import { SmallBookListItem } from "./components/books/SmallListItems";
  import { NumberedList } from "./components/lists/Numbered";
  import { RegularList } from "./components/lists/Regular";
  import { authors } from "./data/authors";
  import { books } from "./data/books";

  function App() {
    return (
      <>
        <RegularList
          items={authors}
          sourceName={"author"}
          ItemComponent={SmallAuthorListItem}
        />
        <NumberedList
          items={authors}
          sourceName={"author"}
          ItemComponent={LargeAuthorListItem}
        />

        <RegularList
          items={books}
          sourceName={"book"}
          ItemComponent={SmallBookListItem}
        />

        <NumberedList
          items={books}
          sourceName={"book"}
          ItemComponent={LargeBookListItem}
        />
      </>
    );
  }

  export default App;
  ```

  **Code Explanation**

  *List Items*
  -> LargeAuthorListItem renders detailed information about an author, including their name, age, country, and a list of their books.
  -> LargeBookListItem renders detailed information about a book, including its name, price, title, and number of pages.
  -> SmallAuthorListItem renders a simplified view of an author, showing only their name and age.
  -> SmallBookListItem renders a simplified view of a book, showing only its name and price.

  *List Rendering Components*
  -> RegularList renders a list of items without any additional numbering or special formatting.
  -> NumberedList adds a number before each item in the list to indicate its position in the list.

  *Example Usage in App Component*
  -> In the App component, both lists of authors and books are rendered using RegularList and NumberedList.

  **Conclusions**
  -> Both authors and books are rendered using both RegularList and NumberedList, demonstrating how the same layout components can be used to handle different types of data consistently.