You will get a chance to practice whiteboarding a lot during this course section. Time will be set aside during most class sessions at the discretion of your teacher. Use this time wisely to improve both your whiteboarding and functional programming skills!

### Practice with Composition

Alternate problem #1 and #2 with your pair. Then alternate problem #3 and #4. If someone gets stuck, you may help your pair whiteboard a problem or brainstorm a solution.

#### Problem #1

Compose a function called `paint()` and then add it to three different painters. Each painter can only paint with one color. This should be the end result:

```js
> painter1.paints()
"Paints green!"
> painter2.paints()
"Paints yellow!"
> painter3.paints()
"Paints red!"
```

#### Problem #2

Compose a function called `sound()`. You should be able to add the following functionality to the objects listed:

```js
> faucet.sound()
"Drip drip drip."
> oldCar.sound()
"Grumble grumble"
> sleepyBear.sound()
"Grrr...yawn"
```

#### Problem #3

Compose a function called `throw()`. The `throw()` function should determine the `distance` and `speed` a battle robot can throw a spear. This function should be unary, so you will need to use currying.

Next, add the `throw()` function to multiple battle robots. A result might look something like this:

```js
> battleRobot1.throw();
"The battle robot throws the spear 100 yards at 200 miles per hour!"
```

#### Problem #4

First use closures to create three dance moves. For instance, a `dancer` should be able to do the following:

```js
> dancer.samba()
"The dancer sambas!"
> dancer.tango()
"The dancer tangos!"
```

Then add the dance moves to a `dancer`.