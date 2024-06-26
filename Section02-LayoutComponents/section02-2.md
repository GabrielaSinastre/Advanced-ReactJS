# Design Patterns: Layout Components

## Introduction 
- Clear definition of Layout Components
  -> Layout Components within the context of React are specialized components that focus on organizing all the components within a web page.
    -> Such example: the split screen layout, where multiple components are arranged in different sections of the page. We will examine the complexities of working with lists and list items as displaying data in a list format can be surprisingly challenging to accomplish perfectly.

- Fundamental concept behind layout components:
  -> When we develop a component, for instance, a side navigation bar, we tend to include both the HTML structure such as div elements and the associated styles within the component itself.
  -> HOWEVER, with layout components, we adopt a different approach.
  -> We need to SEPARATE the actual layout styles into a dedicated component and only insert the specific components, like the side navigation that layout component.
  
    Exemplo Tradicional (Acoplado)
    
    ```
    <NavBar style={...}>
      <h3> Component code... </h3>
    </NavBar>
    ```
    In this example, the NavBar component contains a title (<h3>). If NavBar also includes specific styles for positioning, width, height, etc., it is coupling layout logic and content.

    Separando Layout e Conteúdo
    ```
    <LayoutComponent style={...}>
      <SideNavBar />
    </LayoutComponet>
    ```
    - In this example, we have:
    LayoutComponent: This is the component dedicated to managing layout. It can define styles such as display, flex, grid, position, etc., which determine how child components are arranged on the screen.
    - SideNavBar: This component is responsible only for the logic and presentation of the side navigation content. It does not concern itself with where or how it is positioned on the page, as this responsibility belongs to the LayoutComponent.
  
  -> This separation allows us to decouple the components logic from it's specific placement on the page, hranting us greater flexibility in future use.
  -> *Main advantage: is that our individual components, the core content components ouf our page, should be unaare and unconcerned about their precise location within the page structure.*
  -> KEEP IN MIND: Our components should function independently of their placemente on the page.