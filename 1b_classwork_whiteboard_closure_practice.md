You will get a chance to practice whiteboarding a lot during this course section. Time will be set aside during most class sessions at the discretion of your teacher. Use this time wisely to improve both your whiteboarding and functional programming skills!

### Practice with Closures and Currying

Alternate problem #1 and #2 with your pair. Then alternate problem #3 and #4.

#### Problem #1

Use a closure to create multiple functions for adding a prefix to a name. The prefix could be Doctor, Duchess, Sir, and so on.

It should be possible to create a new prefix function like this:

```js
const prefixSir = addPrefix("Sir");
```

#### Problem #2

Use a closure to create multiple functions for making various animal noises. For example, it should be possible to do the following:

```js
const lionSound = soundMaker("roar");
const mouseSound = soundMaker("squeak");
```

#### Problem #3 (Harder)

Use closures to create multiple functions for adding both a prefix **and** a suffix to a name. For example, it should be possible to do the following:

```js
const misterJunior = nameEnhancer("Mister")("Junior");
const duchessThird = nameEnhancer("Duchess")("The Third");
```

#### Problem #4 (Harder)

Use closures to create multiple functions to first add to and then multiply a value. It should be possible to do the following:

```js
const addTwoMultiplyByThree = addaMult(2)(3);
const addFiveMultiplyByNine = addaMult(5)(9);
```