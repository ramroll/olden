## Old-fashion javascript virtual dom library

Olden are functions, Oldens are simple. Oldens gives no suggestions about how your programs should be written. Oldens prefer very old programming architecture. 

Oldens cannot translate jsx to dom , the `babel-plugin-react-jsx` plugin do it.  

Why should I use Olden?
- I need a javascript template less than 1k. 
- You need a high performance virtual-dom algorithm.


### install

```
yarn add olden
```

### babel
Olden depends `transform-react-jsx` and `imports-transform`.

```
yarn add -D babel-plugin-transform-react-jsx babel-plugin-imports-transform

```

And the `.babelrc` file:

```
{
  "plugins": [
    ["transform-react-jsx", {
      "pragma": "ele" 
    }],
    ["imports-transform", {
      "olden" : {}
    }]
  ]
}

```

### Create dom element

``` jsx
import {ele} from 'olden'

// tab is a virtual-dom element 
const tab = <div data-click='go'>
  <h1>hello world!</h1>
</div>

tab.appendTo(document.body)

// or delete all siblings of document.body
tab.appendTo(document.body, true) 

// or replace it
tab.replaceTo(document.body)



```

### Improving performance by dom-diff algorithm 
``` jsx
import {dom_diff, apply_diff} from 'olden'

// someA is a domNode
const someA = <div data-click='go'>
  <h1>hello world!</h1>
</div>

someA.appendTo(document.body)

// someB is a domNode
const someB = <div data-click='go'>
  <h1>do it!</h1>
</div>

// calculate the minimal edit distance from someA -> someB
const steps = dom_diff(someA, someB)

// apply the steps
apply_diff(diffs)
```

### Improving performance with skeduler
You can imporeve performance of `apply_diff` with a skeduler.

``` jsx
import { skeduler } from 'olden'
// A default skeduler witch update the diffs in a requestAnimationFrame cycle
 apply_diff(diffs, skeduler).then(() => {
  /// Do something after it
 })

```

### use dom_update function to do privous work
``` jsx
import {dom_updator} from 'olden'
dom_update(someA, someB)

// or simply
// use cannot use updateTo without require dom_updator because updateTo 
// is injected when require dom_updator

// some where in the bootstrap code once
import {dom_updator} from 'olden'

someA.updateTo(someB) 


```



### Make it data-driven
``` jsx

import {ele, dom_updator} from 'olden'

// A pure functional tab panel component
const __tabPanel = ({
  tabs,
  selectedIndex = 0
}) => {
  return <div class='tab-panel'>
    {tabs.map( (tab, i) => {
      const cls = selectedIndex === i ? 'tab active' : 'tab'
      return <div class={cls}>
        <div class='title'>{tab.title}</div>
      </div>
    )}
  </div>
}

function withState(tabPane){
  const initialState  = {
    tabs : [ { title : 'Todo'}, {title : 'Done'} ]
  }
  
  return (nextState) => {
    cosnt props = {...initialState, ...nextState}
    return tabPanel(props)
  }
}

module.exports = withState(__tabPanel)

```

### Binding events to tabPanel

1. Use props
``` jsx
const __tabPanel = ({
  tabs,
  selectedIndex = 0,
  onClick
}) => {
  return <div class='tab-panel'>
    {tabs.map( (tab, i) => {
      const cls = selectedIndex === i ? 'tab active' : 'tab'
      return <div onClick={onClick} class={cls}>
        <div class='title'>{tab.title}</div>
      </div>
    )}
  </div>
}
```

2. binding outside
``` jsx

// in tabpanel.js
const __tabPanel = ({
  tabs,
  selectedIndex = 0,
  onClick
}) => {
  return <div class='tab-panel'>
    {tabs.map( (tab, i) => {
      const cls = selectedIndex === i ? 'tab active' : 'tab'
      return <div data-id={i} class={cls}>
        <div class='title'>{tab.title}</div>
      </div>
    )}
  </div>
}
module.exports = withState(__tabPanel)

// some where to use
import tabPanel from 'tabpanel'

// render to dom
const tabPanelVDom = tabPanel()
tabPanelVDom.renderTo(document.body)

querySelectAll('.tab').forEach(tab => {
  tab.addEventListener('click', (e) => {
    const index = e.currentTarget.attributes['data-id'].value
    cosnt next = tabPanel({selectedIndex : index})
    tabPanelVDom.updateTo(next)
  })
})
```

