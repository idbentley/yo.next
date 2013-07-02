# yo.next

Pondering the future of Yeoman.

## Motivations

Yeoman has done a great job, but I think it can go much further in making developers more productive.
From my experiences writing and maintaining `generator-angular` and `generator-karma`, I think a few significant architectural changes could make Yeoman much easier to use and extend.


### Combinatorial Complexity of Generators

Using different generators together is complex both for end users and generator authors.
To illustrate this, let's eximine the end user case and the generator developer case.

#### End User

A user wants to write a mobile application using Backbone and Zurb Foundation.
She goes to the Yeoman site, reads the docs, and closely follows the instructions.
Still, she is confused about which generator to install.
She decides to find a backbone one.
She has these options:

* generator-backbone
* generator-backbone-amd
* generator-backbone-module
* generator-bb
* generator-bbb
* generator-bbe
* generator-footguard
* generator-marionette
* generator-maryo
* generator-phonegap-backbone
* generator-stamp
* generator-ultimate

She starts with `generator-backbone`, but finds that it doesn't have support for Foundation.
So she finds an appropriate Foundation genetator.
She runs `generator-foundation`, but it reports some file conflicts.
She is able to fix them herself, but she has to search Github for existing issues explaining how to resolve.

Next, the user wants to build the project.
Unfortunately, she finds that some of the techniques for optimizing for mobile are absent from her Gruntfile.
She finds, after some research, that `generator-mobile` will create a more appropriate Gruntfile.
She runs `yo mobile`, but again has to manually fix conflicts in the Gruntfile.

Finally, her app builds as expected.

#### Generator Author

A developer of a cutting edge new framework called "UnicornJS" wants to take advantage of Yeoman.
Specifically, she wants to make it easy for users of her framework to set up an enviornment for testing.
She likes Karma, and decides that she wants to support scaffolding a Gruntfile that takes care of building and testing with Karma.

#### Conclusion

In both fo these cases, developers and users had to be experts at using each of the generators.
Yeoman can improve this experience by doing a better job of decoupling and abstracting away the framework and domain specific requisite knowledge to put together all of these pieces.


### Build Proccess

Although Grunt is a great task runner, it has the wrong model for Yeoman.
This is no fault of Grunt's; it wasn't designed with a tool like Yeoman in mind.
From my experiences, here are the issues with Grunt from a Yeoman perspective.

#### Ordering Constraints

Grunt is a list, Yeoman needs a pipeline.

With Grunt, you define some order of tasks that run in a certain order.
This makes sense for hand-written configurations.
It is a bad choice for cases where you have many tasks and many files.
A better model for Yeoman would be to have tasks know what tasks should run before them.
Then a task runner could decide the order, and parallelize when profitable.
Better yet, the results of each step in the task could be cached and reused, improving the perfomance of future builds that only touch a few files.

I greatly admire the ecosystem of Grunt.
If we were to diverge from Grunt, I think it would be valuable to provide some level of interop with it.

Also worth looking at:

* [automation](https://github.com/IndigoUnited/automaton)
* [astral](https://github.com/btford/astral) (Work-in-progress; planning to use a similar "pipeline" model)


## Solution

This is what I'd like the solution to look like:

### Architecture

TODO

### Code

This is some non-functional sample code for what the APIs might look like for writing generators.

#### Generator

`generator-webapp`:

```javascript
module.exports = {
  requires: [
    'client:js:framework'
  ],
  optional: [
    'client:css:framework'
  ]
};

```

`generator-backbone`:

```javascript
module.exports = {
  provides: [
    'client:js:framework'
  ],
  optional: [
    'client:test:framework'
  ]
}
```

### Experience

Here is how this flow should feel for generator authors and end users:

#### Generator Author

TODO

#### End User

End users now use a single generator targetting their `Platform` of choice.
They can then select from a list of installed generators, or optionally grab other ones from `npm`.
