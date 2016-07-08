
<br/>
<p align="center"><strong>Previous:</strong><br/><a href="./installing-slate.md">Installing Slate</a></p>
<br/>

### Adding Event Handlers

Okay, so you've got Slate installed and rendered on the page, and when you type in it, you can see the changes reflected. But you want to do more than just type a plaintext string.

What makes Slate great is how easy it is to customize. Just like other React components you're used it, Slate allows you to pass in handlers that are triggered on certain events. You've already seen on the `onChange` handler can be used to store the changed editor state, but let's try add something more...

We'll show you how to use the `onKeyDown` handler to change the editor's content when the user presses a button.

So we start with our app from earlier:

```js
class App extends React.Component {

  constructor(props) {
    super(props)
    this.state = {
      state: initialState
    }
  }

  render() {
    return (
      <Editor
        state={this.state.state}
        renderNode={node => this.renderNode(node)}
        onChange={state => this.onChange(state)}
      />
    )
  }

  renderNode(node) {
    if (node.type == 'paragraph') return ParagraphNode
  }

  onChange(state) {
    this.setState({ state })
  }

}
```

And now we'll add an `onKeyDown` handler:

```js
class App extends React.Component {

  constructor(props) {
    super(props)
    this.state = {
      state: initialState
    }
  }

  render() {
    return (
      <Editor
        state={this.state.state}
        renderNode={node => this.renderNode(node)}
        onChange={state => this.onChange(state)}
        onKeyDown={(e, state) => this.onKeyDown(e, state)}
      />
    )
  }

  renderNode(node) {
    if (node.type == 'paragraph') return ParagraphNode
  }

  onChange(state) {
    this.setState({ state })
  }

  // Define a new handler which prints the key code that was pressed.
  onKeyDown(e, state) {
    console.log(e.which)
  }

}
```

Okay cool, so now when you press a key in the editor, you'll see the key's code printed to the console. Not very useful, but at least we know it's working. 

Now we want to make it actually change the content. For the purposes of our example, let's say we want to make it so that whenever a user types `&` we actually add `and` to the content. 

Our `onKeyDown` handler might look like this:

```js
class App extends React.Component {

  constructor(props) {
    super(props)
    this.state = {
      state: initialState
    }
  }

  render() {
    return (
      <Editor
        state={this.state.state}
        renderNode={node => this.renderNode(node)}
        onChange={state => this.onChange(state)}
        onKeyDown={(e, state) => this.onKeyDown(e, state)}
      />
    )
  }

  renderNode(node) {
    if (node.type == 'paragraph') return ParagraphNode
  }

  onChange(state) {
    this.setState({ state })
  }

  onKeyDown(event, state) {
    // Return with no changes if it's not the "7" key with shift pressed.
    if (event.which != 55 || !event.shiftKey) return

    // Otherwise, transform the state by insert "and" at the cursor's position.
    const newState = state
      .transform()
      .insertText('and')
      .apply()
    
    // Return the new state, which will cause the editor to update it.
    return newState
  }

}
```

With that added, try typing `&`, and you should see it automatically become `and` instead!

That gives you a sense for what you can do with Slate's event handlers. Each one will be called with the `event` object, and the current `state` of the editor.

<br/>
<p align="center"><strong>Next:</strong><br/><a href="./adding-custom-block-nodes.md">Adding Custom Block Nodes</a></p>
<br/>