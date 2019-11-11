# Components

## Motivation

These recommendations are intended to improve the process of creation of a custom component library for the project.

See slides to the "Components FTW" talk here: https://docs.google.com/presentation/d/1ZRc_5O2qe_Rkrv1GLrIwzQa-6PKFMRNw39dvzV137zs/edit?usp=sharing

## Modules File Structure and Naming Convention

Here is a set of recommendations that should be applied to component modules' file structure and naming to maintain consistency of codebase.

Developers are encouraged to suggest it during PR reviews. All pain points and conflicts around applying it should be discussed in a team before making changes to these recommendations.

- Component files and folders are named after Component in PascalCase
- Component _should_ have its folder, which encapsulates the module. Components' module _may_ contain:

  - styles file
  - test file
  - storybook or styleguidist file
  - README.md
  - `context.js` file which contains React's context of this module

- Component's folder _should_ contain `index.js` file which _must_ contain **only** exports of that module
- Component _may_ have children components. Component's file structure recommendations are applied to Child component, however:

  - it _may_ have its folder but not required
  - it _may_ have named export in `index.js` but not required
  - it _should_ have its own styles file
  - its _prefferably_ to avoid having Child components of its own

### Example of file structure and naming:

```txt
List/ - component's own folder
| - List.js - component's own file
| - List.module.scss
| - List.test.js
| - List.md - react-styleguidist's component doc

| - ListItem.js
| - ListItem.module.scss
| - ListItem.test.js

| - ListHeader.js
| - ListHeader.module.scss
| - ListHeader.test.js

| - context.js - shared among module's components
| - README.md - module doc
| - index.js
```

### Example of index.js

The file that contains all exports of the List component folder

```jsx
// index.js

export { List as default } from "./List";
export { ListHeader } from "./ListHeader";
export { ListItem } from "./ListItem";
```

### Example of context file usage

Example of creating context to easily re-use values among child components avoiding props drilling anti-pattern:

```jsx
// context.js
import { createContext } from "react";

export const { Provider, Consumer } = createContext({});
```

Add **Provider** to the Parent Component:

```jsx
// List.js
import React, { Component } from 'react';
import { Provider } from './context';

const List = () => {
 return (
    <Provider value={...}>
      ...
    </Provider>
  )
}
```

And then use the `context` via **Consumer** in the Child component:

```jsx
// ListItem.js
import React, { Component } from "react";
import { Consumer } from "./context";

const ListItem = () => {
  return <Consumer>...</Consumer>;
};
```

### Example of List component usage

```jsx
// MyComponent.js
import React from "react";
import List, { ListHeader, ListItem } from "components/List";

const MyComponent = () => {
  return (
    <List>
      <ListHeader>3 simple rules of happy life:</ListHeader>
      <ListItem>- Eat healthy</ListItem>
      <ListItem>- Exercise more</ListItem>
      <ListItem>- Sleep well</ListItem>
    </List>
  );
};
```

## Component code backbone

TBD
