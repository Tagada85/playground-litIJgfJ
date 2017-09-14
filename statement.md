## Introduction

I write because it helps me retain informations. Whatever subject I'm studying I force myself to put it into words, like I'm teaching someone else. My main purpose is not to teach others though, but to teach myself. We always think we understand something, until we have to explain it. Those who know, do, those who teach, do it better. I'll try to teach myself closures in this article.

## Closures

The definition of closure is as follow:

> A closure is when a function is able to remember and access its lexical scope even when that function is executing outside its lexical scope.

### Scope

To understand closure, I must first understand scopes. The scope in a program is a set of rules for storing variables in some location and retrieving them later.
Certain structures in a program create their own scopes ( functions, ifs, for loops ...). If I declare a variable inside a scope, it is not accessible in an other one.

```javascript runnable
// I am in the global scope
const a = 'Damien'

if( true ) {
  // This is a different scope
  const a = 'John'
  console.log(a) //John
}

const func = () => {
  // This is a third scope
  const a = 'Joe'
  console.log(a) // Joe
}

func()
console.log(a) // Damien
 
```

If you try to retrieve a variable that do not exist in the current scope, Javascript will look for it in the outer scope. Javascript will repeat this process until there are no more outer scope to inspect. If the variable is not found, you'll get a ReferenceError:

```javascript
// I am in the global scope

if( true ) {
  // This is a different scope
  const a = 'John'
  console.log(a) //John
}

const func = () => {
  // This is a third scope
  const a = 'Joe'
  console.log(a) // Joe
}

console.log(a) // ReferenceError
func() 
```

I removed the variable declaration in the global scope. When I try to retrieve it, Javascript can't find it and returns an error.

```javascript
// I am in the global scope
const a = 'Damien'

if( true ) {
  // This is a different scope
  console.log(a) //Damien
}

const func = () => {
  // This is a third scope
  const a = 'Joe'
  console.log(a) // Joe
}

console.log(a) // Damien
func() 
```

In this case, I removed the variable declaration in the if block. Javascript can't find the variable a in this scope, so it looks in the outer scope. The program finds a = 'Damien' in this outer scope ( the global scope ) and uses it.

## Back to closure

So now, I understand a bit more about scopes. Closures allow a function to access its scope when that function is executing outside of its scope. Let's see this in action.

```javascript
function outer(){
  const a = 'Damien'

  function inner(){
    console.log(a)
  }

  return inner
}
const func = outer()

func() // 'Damien'

```

Why is this a closure? To be a closure, this would mean that the function *inner* is being executed outside of its lexical scope and still having access to its scope. So what happens here? The function *outer* returns a reference to the *inner* function. We execute the *outer* function and pass it to the func variable. Then we execute the *inner* function by calling *func()*. *inner* is executed, but outside of its declared lexical scope. It's executed outside the *outer* function. In theory, the program would free up space and see that our *outer* function is no longer needed ( garbage collector ). 

*inner* has a lexical scope closure over that inner scope of *outer*. This keeps the scope alive for *inner* to use. The reference that *inner* has in the *outer* scope keeps that scope alive. ==> CLOSURE.

## More examples?

Ok, it's still a bit blurry. Could you give me more examples? Perhaps real-world ones?

```javascript
function chrono( message ){
  setInterval( function timer() {
    console.log( message )
  }, 1000)
}

chrono('GOGOGO')
```

*timer* has an reference to the *chrono* inner scope. That scope is kept alive even after 1 seconds where the *chrono* is clearly no longer needed by *timer*. Because that scope is still alive, *timer* can print 'GOGOGO' every second.

```javascript
function myModule(){
  const name = 'Damien'
  const age = 25

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

Closures ftw!
