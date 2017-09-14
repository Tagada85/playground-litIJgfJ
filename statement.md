## Introduction

Closures are a crucial aspect of Javascript. Yet it remains a mysterious subject that some developers do not understand properly. Closures are extremely powerful and useful. I'm absolutely convinced that even if you do not know anything about closures (yet!), you already use them in your code on a regular basis. Let's see what they are all about!

## Closures

The definition of closure is as follow:

> A closure is when a function is able to remember and access its lexical scope even when that function is executing outside its lexical scope.

### Scope

To understand closures, we must first understand scopes. The scope in a program is a set of rules for storing variables in some location and retrieving them later.
Certain structures in a program create their own scopes. If I declare a variable inside a scope, it is not accessible in an other one.

```javascript runnable
// I am in the global scope
let a = 'Damien'

if( true ) {
  // This is a different scope
  let a = 'John'
  console.log(a) //John
}

const func = () => {
  // This is a third scope
  let a = 'Joe'
  console.log(a) // Joe
}

func()
console.log(a) // Damien
 
```

We have three different scopes here: the global, the one created by the if block and the one created by the function *func*.

If you try to retrieve a variable that do not exist in the current scope, Javascript will look for it in the outer scope. Javascript will repeat this process until there are no more outer scope to inspect. If the variable is not found, you'll get a ReferenceError:

```javascript runnable
// I am in the global scope

if( true ) {
  // This is a different scope
  let a = 'John'
  console.log(a) //John
}

const func = () => {
  // This is a third scope
  let a = 'Joe'
  console.log(a) // Joe
}

console.log(a) // ReferenceError
func() 
```

I removed the variable declaration in the global scope. When I try to retrieve it, Javascript can't find it and returns an error. In this case, there are no outer scope to inspect because we are already in the global scope.

```javascript runnable
// I am in the global scope
let a = 'Damien'

if( true ) {
  // This is a different scope
  console.log(a) //Damien
}

const func = () => {
  // This is a third scope
  let a = 'Joe'
  console.log(a) // Joe
}

console.log(a) // Damien
func() 
```

In this case, I removed the variable declaration in the if block. Javascript can't find the variable a in this scope, so it looks in the outer scope. The program finds a = 'Damien' in this outer scope ( the global scope ) and uses it.

## Back to closures

So now, we understand a bit more about scopes. Closures allow a function to access its scope when that function is executing outside of its scope. Let's see this in action.

```javascript runnable
function outer(){
  let a = 'Damien'

  function inner(){
    console.log(a)
  }

  return inner
}
const func = outer()

func() // 'Damien'
console.log('Closure happened!!')

```

Why is this a closure? To be a closure, this would mean that the function *inner* is being executed outside of its lexical scope and still having access to its scope. So what happens here? The function *outer* returns a reference to the *inner* function. We execute the *outer* function and pass it to the func variable. Then we execute the *inner* function by calling *func()*. *inner* is executed, but outside of its declared lexical scope. It's executed outside the *outer* function. In theory, the program would free up space and see that our *outer* function is no longer needed ( garbage collector ). 

*inner* has a lexical scope closure over that inner scope of *outer*. This keeps the scope alive for *inner* to use. The reference that *inner* has in the *outer* scope keeps that scope alive. ==> CLOSURE.

## More examples?

Ok, it's still a bit blurry. Could you give me more examples? Perhaps real-world ones? What about callbacks?

```javascript runnable
function chrono( message ){
  setInterval( function timer() {
    console.log( message )
  }, 1000)
}

chrono('GOGOGO')
```

*timer* has an reference to the *chrono* inner scope( with the *message* variable ). That scope is kept alive even after 1 second where the *chrono* is clearly no longer needed by *timer*. Because that scope is still alive, *timer* can print 'GOGOGO' (the *message* variable) every second.

One last example. Let's look at the famous module pattern.

```javascript runnable
function myModule(){
  let name = 'Damien'
  let age = 25

  function sayMyName(){
    console.log(name)
  }

  function sayMyAge(){
    console.log(age)
  }

  return {
    sayMyAge,
    sayMyName
  }
}

const boom = myModule()

boom.sayMyAge()
boom.sayMyName()
```

Module Pattern! *sayMyAge* and *sayMyName* are both executed outside of their lexical scope. But because both have references to the *myModule* inner scope, the scope is kept alive for them to use the name and age variable. 

## Conclusion

I am sure you have used closures in the past without you knowing it. Look at the code you wrote in the past and try to spot the closures you wrote. In the next playgrounds, we will dig deeper in functional programming and the use of closures.

