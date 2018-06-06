## Simple library To Translate jsx to dom

Make your javascript program sweet ~ 

Sweet doesn't translate jsx to dom , `babel-plugin-react-jsx` do it. Sweet create dom with jsx, and provide algorithms to maintian thoses things. Sweet are functions, you just need to import which you need.

### install

```
yarn add sweet
```

### babel
sweet.ele depends transform-react-jsx

```
{
  "plugins": [
    ["transform-react-jsx", {
      "pragma": "dom" // default pragma is React.createElement
    }]
  ]
}
```

### use sweet to create dom element

``` jsx
import {ele} from 'sweet'

const tab = <div data-click='go'>
  <h1>hello world!</h1>
</div>

```

### use dom-diff algorithm to to improve performance
``` jsx
import {dom_diff, dom_apply} from 'sweet'

// someA is a domNode
const someA = <div data-click='go'>
  <h1>hello world!</h1>
</div>

// someB is a domNode
const someB = <div data-click='go'>
  <h1>do it!</h1>
</div>

// get update sequence
const diffs = dom_apply(someA, someB)

// update dom
// then 'hello world!' is replaced to 'do it!' with minimal 
// edit distance algorithm
apply_diff(diffs)

```

### use dom_update function to do privous work
``` jsx
import {dom_update} from 'sweet'

// someA is a domNode
const someA = <div data-click='go'>
  <h1>hello world!</h1>
</div>

// someB is a domNode
const someB = <div data-click='go'>
  <h1>do it!</h1>
</div>

// update dom
// then 'hello world!' is replaced to 'do it!' with minimal 
// edit distance algorithm
dom_update(someA, someB)

```

### let's make it data-driven
``` jsx

import {ele, dom_update} from 'sweet'

const tabPanel = ({
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

// then tabNode is a todo-list tab
const tabs = {
  tabs : [ { title : 'Todo'}, {title : 'Done'} ]
}
const tabNode = tabPanel({tabs})

// let's write a updator fundtion
funtion changeSelection(selectedIndex){
  const next = tabPanel({tabs, selectedIndex})
  update(tabPanel, next)
}

```
