# Node

Notes based on the course NodeJS - The Complete Guide <https://www.udemy.com/course/nodejs-the-complete-guide/>

Node is a javascript runtime, allow run javascript anyware outside the browser.
Node uses V8 that is the javascrpt engine the google built for Chrome. Takes the javascript code and compiles it to machine code that runs in the computer. You can manupulate the file sistem.

We use node to handle some big operations on the backend

- Conect and handel data from/to databases
- Authentication
- Multiple validations (input validation, forms data)
- Busisness logic

Node can also be used to build utility scripts, build tools, clis

Node has 2 main ways for running:

- REPL (Read, Execute, Print, Loop)
- Execute files

## Install in linux

got to <https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-18-04>

## Javascript Notes

### Reference and primitive values

Primitive values are: String, Number, Boolean, Undefined, Null, Symbol
When you assign something else to a primitive value (val a = val b), that is copied by value.
The primitive values are stored on the STACK which is a small fast-access place of memory

Reference types are: Objects
When you asign something to a reference value it stores and address pointing to the adress in memory where the reference value is stored. When you save an const _reference value_, you just save a constant pointer to that element.
The reference values are stored on the HEAP which is a huge slower-access place of memory. When we create an object all the information is saved on the heap but the pointer to that object is stored in the STACK.
Every time that we create an new object we create a new place in HEAP
When you create an object from another object with Object.assign({first_object}, {secon_object}), the primitive values are copied but only the the pointer is copied for reference values

### This keyword

```js
const person = {
  name: 'Juan',
  age: 31,
  // this on the arrow refers to the global scope
  greet1: () => {
    console.log('Hi, I am ' + this.name); // Hi, I am undefined
  },
  // this on the function refers to the object scope
  greet2: function () {
    console.log('Hi, I am ' + this.name); // Hi, I am Juan
  },
  // same as above
  greet3() {
    console.log('Hi, I am ' + this.name); // Hi, I am Juan
  },
  // same as above
  greet4() {
    console.log('Hi, I am ' + (() => this.name)()); // Hi, I am Juan
  },
};
```

### Async code

Everiting that is returned from a then block will be treated lke a promise, if the return ins not a promise, the engine will treat it like a promise and wil resolve it inmediatly

```js
const fetchData = () => {
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('Done!');
    }, 1500);
  });
  return promise;
};

setTimeout(() => {
  console.log('Timer is done');
  fetchData()
    .then((text) => {
      console.log(text);
      return fetchData();
    })
    .then((text2) => {
      console.log(text2);
    });
}, 2000);
```

## Node Basics

When you creates a server it will take an argument called a listener that will see every request that comes into the server

When you execute a node program it will run ant then fall into the event loop, wich will run meanwhile there is an event listener registered

Node has an ongoing loop as long as there are event listeners executing

To kill the event listeneres you can use _process.exit()_

### Streams and buffers

The incoming data is sent as an stream of data.
A stream is basically an ongoing process. The request is read by the server in chunks of data and in some point it is done. We can work on any individual chunk before all the response is alrady send.

A buffer is a construct wich allows to hold multiple chunks and work with them before they are all released. You work with the buffers

The _data_ event is fired when a new chunk of data is ready to be read

Sending a response doesnt implies that the even listener is dead. If we need that some operation be done before sending the response, we need to be sure to send that when the event on the eventt listener is done.

### Single Thread, event loop & blocking code

The event loop only handles callbacks that contain fast finishing code

The long time operations are send to the worker pool. This runs on diferent threads and this is managed by node and works with the host machine processor

When a job is done on the worker pool it triggers a callback and that one will be handled by the event loop

The event loop looks in order for the different type of loops, it checks in the next order:

- Timers (Executes setTimeout, setInterval callbacks)
- Pending callbacks (Execute I/O-related callbacks that were deferred)
- Poll (Retrieve new I/O events, execute their callbacks), if in the poll there is a timer execution it will jump inmediatly to the start of the loop
- Check (execute setImmediate() callbacks )

### Modules system

## Some node basic modules

| Module | Function                                                                               |
| ------ | -------------------------------------------------------------------------------------- |
| http   | Handels all http functions and methods, helps to send requests and and launch a server |
| https  | Handles secure http                                                                    |
| fs     | Handles functions and methods to interact with the file system                         |
| path   | Handles functions and methods that helps to build paths in different OS                |
| os     | Handles functions and methods that helps to interact whit the host operating system    |
