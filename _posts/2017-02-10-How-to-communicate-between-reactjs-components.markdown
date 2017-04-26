---
layout:     post
title:      "How to communicate between Reactjs components"
subtitle:   "Strategies for React component communication"
date:       2017-02-10 12:00:00
author:     "Jane"
header-img: "img/post-bg-nextgen-web-pwa.jpg"
header-mask: 0.3
catalog:    true
tags:
    - React
    - Components communication
---

React  is an open-source JavaScript library for building user interfaces and often described as “the V in the MVC structure”. It is augmented by allowing user to create highly reusable UI components (e.g.tab bars, comment boxes, pop up modals, lists, sortable tables, etc).

Therefore, In React, one of the first big issues that comes up is figuring out how components should communicate with each other.

> What's the best way to tell Component1 that Component2 was clicked?

I know, some answers will mention: Redux. Yes, we can solve communication issue by using Redux, but Sometimes it is not necessary. In this article, None of the strategies use Redux. 

#### From Parent to Child
![Parent communicate with Child](/img/in-article/2017-02-10-How-to-communicate-between-reactjs-components/parent-to-child.png)

* **Props**

Props are by far the most common way to transfer information, we can send data from a parent to a child.

```jsx
class Child extends React.Component{
    constructor(props){
        super(props)
        this.state= {};
    }
    render(){
        return (
            <div>
            Child: 
                {this.props.text}
            </div>
        )

    }
}

class parent extends React.Component {
    constructor(props) {
        super(props)
        this.state = {}
     }

    refreshChild() {
        return (e)=> {
            this.setState({
                childText: "parent communicate with child sucessful",
            })
        }
    }

    render() {
        return (
            <div>
                <h1>communication from parent to child</h1>
                Parent: 
                <button onClick={this.refreshChild()}>refresh child</button>
                <Child
                    text={this.state.childText || "child did not refresh"}/>
            </div>


        )
    }
}
```
#### From Child to Parent
![Child communicate with Parent](/img/in-article/2017-02-10-How-to-communicate-between-reactjs-components/child-to-parent.png)

* **Callback Functions**

The simplest way is for the parent to pass a function to the child. The child can use that function to communicate with its parent.

```jsx
class Child extends React.Component{
    constructor(props){
        super(props)
        this.state= {};
    }
    render(){
        return (
            <div>
            Child: 
                <button onClick={this.props.refreshParent}> refresh parent</button>
            </div>
        )

    }

}

class parent extends React.Component {
    constructor(props) {
        super(props)
        this.state = {}
        this.refreshParent = this.refreshParent.bind(this)
     }

    refreshParent() {
        this.setState({
            parentText: "child communicate with parent sucessful "
        })
    }

    render() {
        return (
            <div>
                <h1>communication from child to parent </h1>}
                Parent: 
                <Child
                    refreshParent={this.refreshParent}/>
                {this.state.parentText || "father did not refresh"}
            </div>


        )
    }
}
```

#### From Sibling to Sibling
![sibling communicate with sibling](/img/in-article/2017-02-10-How-to-communicate-between-reactjs-components/sibling-to-sibling.png)

* **Parent Component**

If you wrap multiple components in a parent component, you can use some of the above strategies in combination to facilitate communication between them.

```jsx
class Sibling1 extends React.Component{
    constructor(props){
        super(props);
        this.state = {}
    }
    render(){
        return (
            <div>
            Sibling1: 
                <button onClick={this.props.refresh}>
                    refresh Sibling
                </button>
            </div>
        )
    }
}
class Sibling2 extends React.Component{
    constructor(props){
        super(props);
        this.state = {}
    }
    render(){
        return (
            <div>
            Sibling2: 
                {this.props.text || "sibling did not refresh"}
            </div>
        )
    }
}
class Parent extends React.Component{
    constructor(props){
        super(props);
        this.state = {}
    }
    refresh(){
        return (e)=>{
            this.setState({
                text: "sibling communicate with sibling sucessful",
            })
        }
    }
    render(){
        return (
            <div>
                <h2>sibling communication</h2>
                <Sibling1 refresh={this.refresh()}/>
                <Sibling2 text={this.state.text}/>
            </div>
        )
    }
}
```









