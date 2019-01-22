# React Best Practices

This repo is for the purposes of a high-level view of React architecture and best practices. As this repo grows, more ideas will be introduced and comparisons will be made with pros and cons.

Being that this is a public repo, I hope that people will not only take what they can from the repo's information, but contribute as well. Feel free to submit pull requests and I will add your information to the document.

Forking this repo is great if you want to create your own repo with your decisions, using this as a starting point.

## Architecture

### Patterns

**Flux Pattern**  
https://code-cartoons.com/a-cartoon-guide-to-flux-6157355ab207  

**Render Props Pattern**  
https://medium.freecodecamp.org/how-to-develop-your-react-superpowers-with-the-render-props-pattern-b74e68c6d053

### Functional Paradigm

#### Functional Programming

- [Immutability](https://medium.com/dailyjs/the-state-of-immutability-169d2cd11310)
  - No side-effects
  - Predictable data
- [Functional methods](https://medium.com/dailyjs/the-state-of-immutability-169d2cd11310)
  - map/filter/reduce
    - [w3Schools array functions](https://www.w3schools.com/jsref/jsref_obj_array.asp)
- [Higher Order Components/Functions](https://medium.freecodecamp.org/higher-order-components-the-ultimate-guide-b453a68bb851)
  - Reusability and Composition

#### Pure Components

- fn(state, props) == UI
- Local state suggestions from: https://medium.com/@robftw/characteristics-of-an-ideal-react-architecture-883b9b92be0b
  - No local state other than:
    - Animation/triggering of CSS Classes
    - UI control that doesn't involve business logic or dependent on other components
    - Extremely trivial UI related variables that won't cause side effects
    - Standalone libraries

### Routing

**React Router**  
https://github.com/ReactTraining/react-router

Guide:  
https://reacttraining.com/react-router/web/guides/quick-start

### State Management

**Redux**  
https://redux.js.org/

#### Middleware

**Redux Thunk**  
https://github.com/reduxjs/redux-thunk

"Thunks are the recommended middleware for basic Redux side effects logic, including complex synchronous logic that needs access to the store, and simple async logic like AJAX requests."

**Redux-Sagas**  
https://github.com/redux-saga/redux-saga

Similar concept to thunk using Middleware to control side-effects in a React-Redux application, but with the added bonus of using ES6 Generators to control async flow and increase readability.

[More on Generators](https://redux-saga.js.org/docs/ExternalResources.html)

### State Structure

**From:** https://redux.js.org/recipes/structuring-reducers/normalizing-state-shape

Normalized data structures by Id (Relational Table Model)

```javascript
{
  posts: {
    byId: {
      "post1" : {
        id: "post1"
        author: "author1"
        body: "..."
        comments: ["comment1", "comment2"]
      },
      "post2" : {
        id: "post2"
        author: "author2"
        body: "..."
        comments: ["comment3", "comment4", "comment5"]
      },
    },
    allIds: ["post1", "post2"],
  },
  comments: {
    byId: {
      "comment1": {
        id: "comment1",
        author: "user2",
        comment: "...",
      },
      "comment2": {
        id: "comment2",
        author: "user3",
        comment: "...",
      },
      "comment3": {
        id: "comment3",
        author: "user3",
        comment: "...",
      },
      "comment4": {
        id: "comment4",
        author: "user1",
        comment: "...",
      },
      "comment5": {
        id: "comment5",
        author: "user3",
        comment: "...",
      },
    },
    allIds: ["comment1", "comment2", "comment3", "comment4", "comment5"],
  },
  users: {
    byId: {
      "user1": {
        username: "user1",
        name: "User 1",
      },
      "user2": {
        username: "user2",
        name: "User 2",
      },
      "user3": {
        username: "user3",
        name: "User 3",
      },
    },
    allIds: ["user1", "user2", "user3"]
  },
  simpleDomainData1: {...},
  simpleDomainData2: {...},
}
```
Removes the deeply nested structures as your state grows in size and keeps components from re-rendering accidentally.

### Folder Structure

```
.
├── /android/
├── /ios/
|
├── /assets
│   ├── /fonts/
│   ├── /images/
│   ├── /videos/
|
├── /app or /src
│   ├── /ducks/  // See "Ducks"
│   ├── /scenes/
│   |   ├── /App
│   |   |   ├── /_/
│   |   |   ├── /App.js
│   |   |   └── /index.js
|   |   ├── /MyScene/
|   |   |   ├── /__tests__/
|   |   |   ├── /_
|   |   |   |   ├── /MySubcomponent
|   |   |   |       ├── /__tests__/
|   |   |   |       ├── /MySubcomponent.js
│   |   |   |       └── /index.js
│   |   |   ├── /MyScene.js
│   |   |   └── /index.js
│   ├── /shared/
|   |   ├── /SharedAppSpecificComponent
|   |       ├── /__tests__/
|   |       ├── /SharedAppSpecificComponent.js
|   |       └── /index.js
│   └── /types/
|
|
├── /lib
|   ├── /Button
|       ├── /__tests__/
|       ├── /Button.js
|       └── /index.js
|
├── /tools/  //Build scripts, other tools
├── /node_modules/
└── package.json
```
https://github.com/kylpo/react-playbook/blob/master/component-architecture/5_Example-App-Structure.md

**/lib/ explanation:**
Every component that is not "app specific" or a *primitive component* can be a candidate to be moved to the /lib/ folder. This allows for the possible side effect of hand-rolling your own component library as you code!

Primitive components are those that take in only primitives as their props and are functionally pure.

#### Redux "Ducks"

```
.
├── /ducks/
    └── /MyDuck/
    |   ├── /__tests__/
    |   ├── /models  // See redux-orm
    |   |   ├── index.js
    |   |   ├── MyDuckModel1.js
    |   |   └── MyDuckModel2.js
    |   ├── /schema  // See redux-orm / normalizr
    |   |   ├── /formatters/
    |   |   |   ├── index.js
    |   |   |   ├── MyDuckModel1Formatter.js
    |   |   |   └── MyDuckModel2Formatter.js
    |   |   ├── index.js
    |   |   ├── normalize.js  // normalizr
    |   |   └── orm.js  // redux-orm
    |   ├── actions.js
    |   ├── index.js
    |   ├── operations.js
    |   ├── reducers.js
    |   ├── selectors.js
    |   ├── types.js
    |   └── utils.js
    └── commonTypes.js
```

https://medium.freecodecamp.org/scaling-your-redux-app-with-ducks-6115955638be

## Best Practices and Coding Style

### Naming Convention

#### Components

**From:** https://hackernoon.com/react-components-naming-convention-%EF%B8%8F-b50303551505

[Domain]|[Page/Context]|ComponentName|[Type]

Examples:

ACommunityAddToShortListButton - [Domain][ComponentName][Type]  
SideBar - [ComponentName]  
SideBarSwitch - [ComponentName][Type]  
ChatConversation - [Page/Context][ComponentName]  
ChatConversationName - [Page/Context][ComponentName]  

### Component Creation

- Write a stateless functional component first
  - if (component requires state or life-cycle) Make stateful class
  - if (component is comprised of multiple components or smaller pieces of functionality) Create container component
  - Move shared components out
    - if (app dependent) Keep in shared/MyComponent
    - if (!app dependent or primitive) Move to lib folder
- Avoid re-rendering component if possible

## Styling

**SASS**  
https://sass-lang.com/

Gives you the ability to nest your CSS, create mixins, among other things.

Guide for SASS in React:  
https://hugogiraudel.com/2015/06/18/styling-react-components-in-sass/

**Styled Components**  
https://www.styled-components.com/

Scope your styles on a component level, directly in your JS file. Allows for computing styles at a functional level in JS rather than relying on pre-compilers.

## Testing

### Libraries

**Jest**  
https://jestjs.io/

Most popular React testing library. Comes with snapshot comparisons which is very useful for testing pure components.

**Sinon**  
https://sinonjs.org/

Spies, stubs and mocks

**Cypress**  
https://www.cypress.io/

Testing suite that provides a click-and-check API for automated in-browser smoke tests.

### Strategies

**From:** https://github.com/kylpo/react-playbook/blob/master/best-practices/react.md

Write component tests that accomplish the following goals (from [Getting Started with TDD in React](https://semaphoreci.com/community/tutorials/getting-started-with-tdd-in-react?utm_source=javascriptweekly&utm_medium=email))

- It renders
- It renders the correct thing
  - Default props
  - Varied props
- It renders the different states
- Test events
- Test edge cases
  - e.g. something that uses an array should be thrown an empty array

## Build and Deploy

### Starter kits

https://reactjs.org/community/starter-kits.html

This list contains many popular choices, but your choice is yours. Getting a good starter kit depends entirely on what you're looking to get out of your project and team.

### Build Libraries

**Babel**  
https://babeljs.io/

Transpiles your javascript down from the latest standards to backwards compatible, browser friendly source. Used for transpiling JSX to JS.

**Webpack**  
https://webpack.js.org/

Bundles your files into static assets that are ready for production deploy. Many extensible plugins available.

**ESLint**  
https://eslint.org/

Keeps your code nice and tidy by yelling at you with squigglies. You can link this to your IDE.

Great ESLint configs:  
[prettier-eslint](https://github.com/prettier/prettier-eslint)  
[eslint-config-airbnb](https://www.npmjs.com/package/eslint-config-airbnb)  

## Source Control

### Git

#### Repositories

[Github](https://github.com/)

[BitBucket](https://bitbucket.org/)

#### Branch Naming Strategies

**From:** http://www.guyroutledge.co.uk/blog/git-branch-naming-conventions/  

`type/component-name/title`

Common Types:

- feature
- fix
- hot-fix
- refactor
- update
- upgrade
- remove

Component Name example:  
`login-form`

Title example:  
`hoc-formik-input`

## Libraries

### Form Helpers

**Redux Forms - *Under Review***  
https://redux-form.com/8.1.0/

Uses Redux to maintain the state of your forms through custom components, abstracted actions and reducers.

**Formik**  
https://github.com/jaredpalmer/formik

Creates an ephemeral state held by its components that abstracts away all the boilerplate that goes into creating and maintaining forms.

### State Management Helpers

**Reselect**  
https://github.com/reduxjs/reselect

Creates a selector where the first functions passed in compute props for a final function. If none of those props have changed, then that function is not run and the result from the previous invocation is returned.

This keeps the state from needlessly causing components to re-render.

**Normalizr**  
https://github.com/paularmstrong/normalizr

Especially useful for taking in schemas of data input and producing an "entities" object and a "result" object. "Entities" is of the structure above in "State Structure" where it is a normalized relational list. "Result" is the list of ids.

**redux-orm**  
https://github.com/tommikaikkonen/redux-orm 

Library that creates Object Relational Mapping (ORM) using the aforementioned structure.

Guide to using redux-orm and Redux structuring as a whole:  
https://blog.isquaredsoftware.com/series/practical-redux/

My personal deep-dive found [here](deep-dives/redux-orm.md)

### Styling

**classnames**  
https://github.com/JedWatson/classnames

Takes in conditionals that returns a space-delimited string for className.

```javascript
import classnames from 'classnames';

let classes = classnames('sd-date', {
  current: date.month() === this.props.month,
  future: date.month() > this.props.month,
  past: date.month() < this.props.month
});

return (
  <button className={classes} />

  vs

  <button className={`button-default-style ${isActive ? "active" : ""}`} />
);
```

### Functional Programming Helpers

**Recompose**  
https://github.com/acdlite/recompose

A library of Higher-Order-Functions and a compose function to ease your move towards more reusable, functional components.

Note: This library is no longer being updated with new features by its creator. With Hooks on the horizon, the React Development team is suggesting a move to that API.

**Ramda**  
https://github.com/ramda/ramda

Emphasizes pure functional code. Functions written in Ramda are pure and side-effect free, making them easy to test. They are also automatically curried, giving you the ability to partially apply parameters and pass back a new function.

Currying explanation: [Why Curry Helps](https://hughfdjackson.com/javascript/why-curry-helps/)

### Miscellaneous Helpers

**date-fns**  
https://date-fns.org/

Date/Time formatting with a functional paradigm and localization.

**dayjs**  
https://github.com/iamkun/dayjs

"Day.js is a minimalist JavaScript library that parses, validates, manipulates, and displays dates and times for modern browsers with a largely Moment.js-compatible API."

**Moment**  
https://github.com/moment/moment/

Date/Time related formatting

## IDEs

**Visual Studio Code**  
https://code.visualstudio.com/

Microsoft has opensourced this light-weight and highly extensible IDE. My personal favorite.

**Atom**  
https://atom.io/

Another IDE that is also very popular amoung web devs.

**Webstorm**  
https://www.jetbrains.com/webstorm/

A JetBrains product who also make IntelliJ and ReSharper. It's not free, but has a mix of monthly and yearly subscription options for the license.

## Roadmaps and Future Functionality

**React 16.x**  
https://reactjs.org/blog/2018/11/27/react-16-roadmap.html

Things to look forward to:

- Suspense
  - Fallback component while a component is rendering. E.g. abstracts need for loading gifs.
- Hooks
  - Add state to function components. Improves optimizing at scale and removes need for "wrapper hell"
- Concurrent
  - Prioritize renders

## Resources, Articles and Podcasts

### Podcasts

**React Native Radio w/ Nadir Dabit**  
https://devchat.tv/react-native-radio/

**Syntax w/ Wes Bos and Scott Tolinski**  
https://syntax.fm/

**The React Podcast w/ Chantastic**  
https://reactpodcast.simplecast.fm/

### Publications

**freeCodeCamp**  
https://www.freecodecamp.org/

**HackerNoon**  
https://hackernoon.com/

**React.js Newsletter**  
http://reactjsnewsletter.com/

### Articles

**Good Starting Points:**  
https://github.com/markerikson/react-redux-links/blob/master/react-architecture.md

**Folder Structure**  
https://github.com/kylpo/react-playbook/blob/master/component-architecture/3_Multiple-Components.md

**Presentational vs Container Components**  
https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0

**Routing**  
https://medium.com/@algfry12/react-router-best-practices-9c564388f4d3

**Symbolic Linking (i.e. get rid of import '../../../etc.')**  
https://medium.com/@justintulk/solve-module-import-aliasing-for-webpack-jest-and-vscode-74007ce4adc9

**Utilities**  
https://blog.bitsrc.io/11-javascript-utility-libraries-you-should-know-in-2018-3646fb31ade