# Functors, Monads
## Error handling on imperative programming

```javascript
try {
  var student = findStudent('444-44-4444');
} catch (e) {
  console.log('ERROR' + e.message);
}
```

1. Can’t be composed or chained.
2. Side effects.
3. Violate the principle of non-locality.
4. Caller need to manage exceptions.
5. Multiple exception-handling blocks.

## Error handling on functional programming
Wrapping unsafe values (potentially erroneous value), so forcing developer to handle it.

```javascript
class Wrapper {
  constructor(value) {
      this._value = value;
  }

    // map :: (A -> B) -> A -> B
    map(f) {
      return f(this.val);
    }

    toString() {
      return 'Wrapper (' + this.value + ')';
  }
}

// wrap :: A -> Wrapper(A)
const wrap = (val) => new Wrapper(val);
```


## Functor
Functor is a powerful wrapper. A functor is an object that can be mapped or has a map method.
And functor obeys the functor laws.  
1. The identity law: `functor.map(a => a)` is equivalent to `functor`
2. The composition law: `functor.map(x => f(g(x)))` is equivalent to `functor.map(g).map(f)`

```javascript
// Wrapper is a functor
// map :: (A -> B) -> Wrapper[A] -> Wrapper[B]
Wrapper.prototype.map = function (f) {
    return wrap(f(this.val));
};
```

Functor and map method make sure:
1. Transformation of contents
2. Maintain structure
3. Returns a new functor

* The purpose of having map return the same type is so you can continue chaining operations.
* They must be side effect–free.
* They must be composable.
* They prohibited from throwing exceptions, mutating elements, or altering a function’s behavior.
* Their practical purpose is to create a context/abstraction that allows you to securely manipulate and apply operations to values without changing any original values.


### Other definitions for functor
> A functor is a function, given a value and a function, unwraps the values to get to its inner value(s), calls the given function with the inner value(s), wraps the returned values in a new structure, and returns the new structure.

http://functionaljavascript.blogspot.tw/2013/07/functors.html

This definitions is from category theory. But in FP language like Haskell, F#, Scala, etc., 95% definitions of functor is a object that can be mappable.

### Reference:  
[Functors, Applicatives, And Monads In Pictures](http://adit.io/posts/2013-04-17-functors,_applicatives,_and_monads_in_pictures.html)  
[What-is-a-functor](https://medium.com/@dtinth/what-is-a-functor-dcf510b098b6)  
[Functors: I was WRONG! - FunFunFunction #11](https://www.youtube.com/watch?v=DisD9ftUyCk)


## Monad
Monad is like a more powerful functor that has a fmap (flatMap) method.  
Monad should have the following interface for monadic operations
* Type constructor —Creates monadic types (Wrapper constructor)
* Unit function—Inserts a value of a certain type into a monadic structure (of method)
* Bind function —Chains operations together (fmap or flatMap)
* Join operation—Flattens layers of monadic structures into one.

```javascript
class Wrapper {
  // Type constructor
  constructor(value) {
     this._value = value;
  }

  // Unit function
  static of(a) {
    return new Wrapper(a);
  }

  // The functor
  map(f) {
    return Wrapper.of(f(this.value));
  }

  // Bind function ( f :: x -> OtherWrapper(x) )
  fmap(f) {
    return f(this.value);
  }

  // Flattens nested layers
  join() {
    if(!(this.value instanceof Wrapper)) {
       return this;
    }
    return this.value.join();
  }
}
```

### Functor vs. Monad
please check the following link again.
[Functors, Applicatives, And Monads In Pictures](http://adit.io/posts/2013-04-17-functors,_applicatives,_and_monads_in_pictures.html)


## IO monad
Wrapping IO (side effects) in monad.
```
const changeToStartCase =
    IO.from(readDom('student-name')).
           map(_.startCase).
           map(writeDom('student-name'));
```

## Error handling by Monad
original handling
```javascript
function showStudent(ssn) {
  if(ssn != null) {
    ssn = ssn.replace(/^\s*|\-|\s*$/g, '');
    if(ssn.length !== 9) {
        throw new Error('Invalid Input');
    }

    let student = db.get(ssn);
    if (student) {
      document.querySelector(`#${elementId}`).innerHTML =
        `${student.ssn},
         ${student.firstname},
         ${student.lastname}`;
    }
    else {
      throw new Error('Student not found!');
    }
  }
  else {
    throw new Error('Invalid SSN!');
  }
}
```

handling by Monad
```javascript
const showStudent = R.compose(
   map(append('#student-info')),
   liftIO,
   map(csv),
   map(R.props(['ssn', 'firstname', 'lastname'])),
   chain(findStudent),
   chain(checkLengthSsn),
   lift(cleanInput)
);
```

## Category theory
https://github.com/yogsototh/Category-Theory-Presentation