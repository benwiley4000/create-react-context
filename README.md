# create-react-15-context

> Polyfill for the [proposed React context API](https://github.com/reactjs/rfcs/pull/2)

## _This is a fork of [jamiebuilds/create-react-context](https://github.com/jamiebuilds/create-react-context)._

*It can be installed by: `npm install --save create-react-15-context`*

_It adds support for older versions of React (15+) by using a [polyfill](https://github.com/benwiley4000/react-dot-fragment) for React 16.2's new [`Fragment`](https://reactjs.org/blog/2017/11/28/react-v16.2.0-fragment-support.html) component._

### _Upstream readme follows:_

## Install

```sh
yarn add create-react-context
```

You'll need to also have `react` and `prop-types` installed.

## API

```js
const Context = createReactContext(defaultValue);
// <Context.Provider value={providedValue}>{children}</Context.Provider>
// ...
// <Context.Consumer>{value => children}</Context.Consumer>
```

## Example

```js
// @flow
import React, { type Node } from 'react';
import createReactContext, { type Context } from 'create-react-context';

type Theme = 'light' | 'dark';
// Pass a default theme to ensure type correctness
const ThemeContext: Context<Theme> = createReactContext('light');

class ThemeToggler extends React.Component<
  { children: Node },
  { theme: Theme }
> {
  state = { theme: 'light' };
  render() {
    return (
      // Pass the current context value to the Provider's `value` prop.
      // Changes are detected using strict comparison (Object.is)
      <ThemeContext.Provider value={this.state.theme}>
        <button
          onClick={() => {
            this.setState(state => ({
              theme: state.theme === 'light' ? 'dark' : 'light'
            }));
          }}
        >
          Toggle theme
        </button>
        {this.props.children}
      </ThemeContext.Provider>
    );
  }
}

class Title extends React.Component<{ children: Node }> {
  render() {
    return (
      // The Consumer uses a render prop API. Avoids conflicts in the
      // props namespace.
      <ThemeContext.Consumer>
        {theme => (
          <h1 style={{ color: theme === 'light' ? '#000' : '#fff' }}>
            {this.props.children}
          </h1>
        )}
      </ThemeContext.Consumer>
    );
  }
}
```
