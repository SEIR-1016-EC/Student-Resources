
# 🚀 Node Process REPL

We can view a project's environmental variables using the node repl in our
terminal. In your terminal,

```
$ cd <your-node-project>
$ node
> console.log(process.env)

```

- *Question:* What are some of the listed variables? Why would they be stored
here?

You can create a new environmental variable in the terminal too! (terminal, not
node!)

```
$ export <YOUR_ENVIRONMENTAL_VARIABLE_NAME>=<variableValue>
```

### Note:

- Make sure there are no spaces next to the equals sign!
- Environment variables tend to be SCREAMING_SNAKE_CASE by convention.

To test, try logging the following code from the node repl.

```
> process.env.<YOUR_ENVIRONMENTAL_VARIABLE_NAME> = // whatever

```

If you'd like, you can also remove the environmental variable by exiting out of
the Node CLI and entering this command in the normal terminal:

```
$ unset <YOUR_ENVIRONMENTAL_VARIABLE_NAME>

```

### Try it out:

Create a new `test.js` file in a sandbox directory

In your sandbox add the following code:

```jsx
const {number} = process.env

let output
if ( !! number){
    output =  Math.ceil(Math.random()*number)
} else {
    output =  Math.ceil(Math.random()*20)
}

console.log(`random number: ${output}`)
```

Now you can add a number export in your command line / zsh: `export number=42`  

When you run your `test` file `node test.js`you should see the output logged to your terminal. 

```jsx
$ random number: 5
```

## Resources

To learn more about the different environments in which your node application runs:

- [process.env: What it is and why/when/how to use it effectively](https://codeburst.io/process-env-what-it-is-and-why-when-how-to-use-it-effectively-505d0b2831e7) (Medium overview)
- [Node documentation - Process](https://nodejs.org/dist/latest-v8.x/docs/api/process.html#process_process)