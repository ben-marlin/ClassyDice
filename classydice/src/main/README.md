# Dice Simulation

In this project, you will write a simulation of dice throwing. Simulations are a common tool in the sciences, and are generally less complicated than game design. 

For this project, you will 
- create a class
- assign fields to a class
- create constructors
- create setters & getters
- override the toString method
- overload several methods
- use a driver program to test the class

## Creating a Class

Java is an object-oriented language which makes extensive use of *class* structure and *inheritance*. You create a class by writing a java file. So far, you have always worked with a static methods, but creating your own class lets you get fancy.

For our purposes, we'll create a class which describes an object which we will think of a die. We'll use the word dice, even though the appropriate singular would be die. When you think of a die, it really only has two *properties* that are important. These are the number of `sides` it has and the current `value` that is face up. 

When we're creating this class, you need to use your Explorer to navigate to the folder where `Main.java` resides. Choose `File > New File > New Java File`. This will generate a file and prompt you to choose between several options - the first of which is what you want, `class`. The name will default to `Main`, so change it to `Dice` - the name of the class should always be the name of the file! Then save the file by hitting Control-S and providing the name `Dice.java` if prompted.

Once you've got that, add `package edu.guilford;` at the top. This just binds our classes together - you'll see more on this in a later semester. Then add comments with your name & date as usual.

Notice the `public Dice {}` at the top. Anything within those braces is in the class. We will need to do any imports outside the class definition.

As mentioned, we need to create two *properties* or *fields*, `sides` and `value` just after the header for the class. Make both of them `public` and `int`. VSCode may complain about the `public`, but we'll fix it later.

We're going to need one more thing, just for bookkeeping. You're going to need to randomize your dice throws, so you need a random object. I suggest naming it `rand` and instantiating it here. If VSCode doesn't do it for you automatically, you'll have to `import java.util.Random;` between the package line and the class header.

Save it. Now you have a class. It doesn't actually do anything yet, and VSCode may have complained about that already.

## Making Constructors

We occasionally refer to *instantiating* an object. This just means creating an "instance" of the object in the computer memory. So if we are rolling a bunch of different dice, we might want each one to appear separately in memory. The *constructor* is a method that takes care of the details of this.

If you have the Java Extension Pack installed, you should be able to right click inside the class and choose `Source Action`. That should get you a *huge* list of actions, many of which are confusing. Look for the one that says `Generate Constructor`. That should then get you a list to check off which things you want set in the constructor. You want to set `sides` and `value`. VSCode should take care of the details, but it probably looks like this.

```    
public Dice(int sides, int value) {
    this.sides = sides;
    this.value = value;
}
```

That's probably a little confusing, right? Remember when we used something like `new String(charArray)` in a previous project? The String class has a constructor that accepts an array of characters and assigns them to the string's important fields. For ours, if you wanted to create a 20-sided die that currently had a 13 showing, you'd use `new Dice(20,13)` to do that. 

Because `sides` is the obvious thing to call the number of sides, it's probably what you want in the constructor, but since it's also what you called it as a field, there has to be a way to distinguish between the field and the value being passed. We use `this.sides` for the field.

Save this - you can't run it, only save it. The `Main.java` class that is also in your project will play the role of a *driver* - code that is used to test the class you're constructing.

## Testing the Constructor

Go over to `Main.java` and insert a line above the print statement like this.

```
Dice d = new Dice(20,13);
```

Now in the print statement, insert `+ d` after the literal string. When you run this code, it should give you an error of some sort when it gets to the print statement. That's because it doesn't know how to print `d`. But if you got to the print statement, then there wasn't a problem with the instantiation!

To fix our error, try replacing `d` with `d.value`. This should make it print the 13, but this is TERRIBLE coding practice! We don't want someone using our `Dice` class to be able to directly manipulate its fields. I just wanted you to be able to print something, and this is why we made the fields public. Make sure `d.value` prints before proceeding.

## Fixing the Privacy

In general, you want object properties to be inaccessible except for the routes you designate for access. Because we choose pretty obvious names for properties, a clever hacker might guess how to read out your `ssn` or `bankBalance` if they are public. By making them private, you prevent that abuse, but it means you have to create a way for other programmers to access them.

Change both `sides` and `value` to `private`. Now, in order to deal with them, you need special methods called *accessors* and *mutators*. Nobody ever calls them by the proper name, though, we say *getters* and *setters*.

Again, right click and open the `Source Action` menu again. Choose `Generate getters & setters` and choose that you want getters & setters for both `sides` and `value`. It should generate some very obvious methods.

Returning to `main.java`, change the print statement to print `d.getValue()`. This should print the 13 again. Try changing to a different value in the instantiation step to make sure it's not just a fluke.

## Better Printing

Every class inherits certain things from its *parent*. In this case, your `Dice` is secretly the child of `Object` and all objects have a method called `toString()` that doesn't do anything. But because the thing that makes sense to read out for a die roll is its `value` property, we're going to fix a couple of problems at once.

Add the following near the bottom of `Dice` - after all the constructors, setters, getters, and so on.

```
@Override
public String toString() {
    return "" + this.value;
}
```

The `@Override` is simply an acknowledgement that you're writing your own version of `toString`. If you leave out `"" +`, you'll get an error because Java thinks you're trying to return an int as a string and it's not smart enough to figure out what you mean. Concatenating the int to an empty string fixes the error, though.

Now, return to `Main.java` and print simply `d` instead if `d.getValue()`. When you run `Main`, it should work the way you expect.

## Rolling

It would be nice to randomize our dice rolls, wouldn't it? Inside of `Dice.java`, create a method like this.

```
public void roll() {
    this.value = rand.nextInt(sides) + 1;
}
```

Now, whenever you want to randomize `d`, you just issue `d.roll()`. Insert a line in `Main` where you do this, then a line to print it again. It should have randomized it and gotten a new value.

## Overloading the Constructor

It is possible to have several constructors for a class, so long as they have notably different function calls. In our case, it would make sense to have a constructor that randomizes `value` when you only pass it the variable `sides`.

```
public Dice(int sides) {
    this.sides = sides;
    this.value = rand.nextInt(sides) + 1;
}
```

And because many people only know about 6-sided dice, it might also make sense to make another constructor when the number of sides is not specified.

```
public Dice() {
    this.sides = 6;
    this.value = rand.nextInt(this.sides) + 1;
}
```

This practice is called *overloading* and is related to a concept called *polymorphism*. You are likely to encounter these in a later class, so we'll just mention them here, saying that it allows you to decide how you want to access similar methods.

## Testing

Make sure to run `main` with examples using all 3 constructors so you're sure they work and that Java sees how to distinguish between them.

## The Simulation

In `Main`, we'll declare two variables.

```
int n = 10;

Dice[] sim = new Dice[n];
```

This defines an array of 10 dice. The reason we define `n` is that we're going to change the number of dice later - it makes sense to only change it once instead of hunting down all the occurrences. Actually, VSCode has the option to "change all occurrences" or something like that. Which is pretty handy, but would have made the instructions longer.

Now Java knows there will be 10 dice. But we have to instantiate each one separately. Let's let it roll them when we do.

```
for (int i = 0; i < n; i++) {
    sim[i] = new Dice();
}
```

Next, we'll calculate the mean. To do this, create a `double` called `sum`, initializing it as 0. Then use a for loop to add each die to `sum`. After the loop, do this.

```
double mean = sum / n;
```

And then make a print statement that prints the sample size & mean. You should get answers around 3.5 for the mean. If your answers are *way* off, talk to me about it.

## Increasing the Sample Size

When you run a simulation, you never do it for $n=10$, but for huge numbers. We "always start small and build up to things", though. So now that it worked for $n=10$, copy the code and do it again, putting another zero on there to make $n=100$ before printing the result. Keep going until your machine complains about it. Mine ran out of memory at $n=100,000,000$... Can you imagine rolling a hundred million dice? Despite the newfound popularity of D&D, I doubt there are that many dice on Earth... In any case, comment out the section that made your computer complain.

## Bigger Dice

Experiment with the notion of rolling dice with more sides: 8, 10, 12, etc. and $n=1000$ or so. Insert another print statement to print what you think the trend is. When you increase the number of sides on the dice by 2, how much does the mean increase by? These are guess-timates, it's OK to be inexact.

## Wrapping Up

As usual, save everything. Stage changes. Commit. Sync. Send me a message on Canvas that you're done.