# React Cheet Sheet

## Question
1. Redux: access store without connect?
2. More and webpack: http2 without bundle, import css
3. React: seperate css into echo module, how to access common css?
4. 
```javascript
{
  reducer: handleActions({
      [FETCH_MOVIES]: (state, action) => ({  // What is this syntax [FETCH_MOVICES]? Answer: This is ES6 syntax, means inside [] is a varaible.
        ...state,
        all: action.movies
      }),
      [FETCH_MOVIE]: (state, action) => ({
  ...state,
        current: state.all[action.index - 1]
      })
  }
}
```



## Create component
```javascript
Class TestClass extends React.Component {
  constructor(props) {
    super(porps)
    this.state = {
      ...
    }
  }

  render() {
    return <div>{this.state.xxxx}</div>
  }

```
or
```javascript
Class TestClass extends React.Component {
  state = {
    ...
  }

  render() {
    return <div>{this.state.xxxx}</div>
  }

```

## Update State
```javascript
setState()
this.forceUpdate()
```

## Stateless components
Has no states or components or lifecycle events.
```javascript
class TestClass extends React.Component {
  render() {
    return <h1 {...this.props}>xxxx</h1>
  }
```
or better
```javascript
const CompFunc = (props)=> {
  return (
    <h1 {...props}>xxx
    </h1>
  )
}
```

## Pass props to tag
```javascript
  render() {
    return <h1 {...this.props}>xxxxx</h1>
  }
```

## Component Life Cycle
* constructor()
* componentWillMount()
* componentDidMount()
* componentWillReceiveProps(nextProps)
* shouldComponentUpdate(nextProps, nextState) -> bool
* componentWillUpdate(nextProps, nextState)
* componentDidUpdate(prevProps, prevState)
* componentWillUnmount()


## Events
[Standard Events](https://developer.mozilla.org/en-US/docs/Web/Events)
[React Events](https://reactjs.org/docs/events.html)

* Capture phase
* Bubbling phase

onMouseOver
onMouseOverCapture

SyntheticEvent
event.nativeEvent
     .currentTarget
     .target
     .preventDefault()
     .isDefaultPrevented()
     .stopPropagation()
     .isPropagationStopped()
     .type
     .persist()
     .isPersistent()

## Controlled Component
Internal state is always in sync with the view.

## Form
* value : <input>, <textarea>, <select>
* checked : <input type="checkbox" type="radio">
* selected : <option>

Use onChange and event.target.value

### Multiselect
```javascript
<select multiple={true} value={['meteor', 'react']}>
  <option value="meteor">Meteor</option>
  ...
```

## Uncontrolled Component
```javascript
<input tyupe="txt" ref="emailAddress" defaultValue={this.props.title}/>
ReactDom.findDOMNode(this.refs.emailAddress)
ReactDom.findDOMNode(this)
```

## Default Properties and Property Types
defaultProps
propTypes

import prop-types

```javascript
Datepicker.propTypes = {
  currentDate: PropTypes.string,
  rows: PropTypes.number,
  locale: propTypes.coneOf(['US', 'CA'])
}
```

### Custom validation
```javascript
propTypes = {
  email: function(props, propName, componentName) {
    var emailRegularExpression =
/^([\w-]+(?:\.[\w-]+)*)@((?:[\w-]+\.)*\w[\w-]{0,66})\.([a-z]{2,6}(?:\. ➥ [a-z]{2})?)$/i
    if (!emailRegularExpression.test(props[propName])) {
      return new Error('Email validation failed!')
    }
  }
}
```

## Children
```javascript
<ComponentXXX>
  <h1>xxx</h1>
</ComponentXXX>

class ComponentXXX extends React.Component {
  render() {
    return (
      <div>
        {this.props.children}
      </div>
    )
  }
}
```

React.Children.count(this.props.children)
React.Children.map()
React.Children.forEach()
React.Children.toArray()


## Higher-order components (HOC)
Add extra functionality to existing component

```javascript
const HOCFunc = (Component) => {
  class _HOCFunc extends React.Component {
    constructor(props) {
      super(props);
      this.state = {...};
    }

    static displayName = 'EnhancedComponent';

    render() {
      return <Component {...this.state} {...this.props} />
    }

    return _HOCFunc;
}

const EnhancedButton = HOCFunc(Button);
const EnhancedLink = HOCFunc(Link);
```

## Presentational vs Container components
Presentational components containg only other presentational components.


## Webpack
Use ESLint

.eslintrc.json
```javascript
"rules": {
    "react/jsx-uses-react": "error",
    "react/jsx-uese-vars": "error",
}
```

Use Flow
Static type-checking tool


## Rounter

### Get router by context (not recommended)
```javascript
<li className={(this.context.router.isActive('/about')) ? 'active': ''}>
  <Link activeClassName="active">
    About
  </Link>
</li>

Content.contextTypes = {
  router: React.PropTypes.object.isRequired
}
```

### withRouter
Higher-Order Component

```javascript
const {withRouter} = require('react-router')

<Router ...>
  <Router path="/contact" component={withRoutner(Contact)} />
</Router>

function Contact(props) {
  // can access props.router
```

### Navigate programmatically
router.push(URL)

### Other properties
* location
* params
* route
* routeParams
* routes

<Route path="/posts" component={Posts} posts={posts} />
Access posts by props.route.posts


### Path parameters
<Route path="/posts/:id" component={Posts} posts={posts}/>
Access id by props.params.id


## Redux
redux-actions replace switch/case
reducer : (state, action) => state
Provider
combineReducer


### React + Redux + Webpack sample project
https://github.com/azat-co/react-quickly/tree/master/ch14





