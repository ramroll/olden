## Simple library To Translate jsx to dom

Sweet are functions, sweets are simple. Sweeps gives not suggestions about how your programs should be written. Sweets are foolish. 

Sweets dont translate jsx to dom , the `babel-plugin-react-jsx` plugin do it. Sweets provide anything to mantain a virtual-dom. In the original design sweets are thought to be used as :

- javascript template less than 1k
- provide a high performance virtual-dom algorithm
- write your own javascript framework 



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

### Create dom element

``` jsx
import {ele} from 'sweet'

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
import {dom_diff, dom_append, dom_apply} from 'sweet'

// someA is a domNode
const someA = <div data-click='go'>
  <h1>hello world!</h1>
</div>

dom_appender(someA.dom)

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

dom_appender(someA.dom)

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

import {ele, dom_update, dom_append} from 'sweet'

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

dom_append(document.body, tabNode)

// let's write a updator fundtion
funtion changeSelection(selectedIndex){
  const next = tabPanel({tabs, selectedIndex})
  update(tabPanel, next)
}

```
