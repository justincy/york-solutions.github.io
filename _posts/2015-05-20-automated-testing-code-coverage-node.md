---
layout: post
title:  "Automated Testing and Code Coverage in Node.js"
date:   2015-05-20
---

[Code coverage][code-coverage] is a valuable metric for understanding the quality of your test
suite. In this post we will show how to quickly generate code coverage statistics
for your JavaScript and Node.js libraries using [Mocha][mocha], [Travis CI][travis],
[Istanbul][istanbul], and [Coveralls][coveralls].

### Test Suite with Mocha

We need a test suite before we can generate code coverage. Our favorite testing
framework is [Mocha ][mocha] with [Chai][chai] for assertions. A test file might
look like this:

```js
var expect = require('chai').expect;

describe('Array', function(){
  describe('#indexOf()', function(){
    it('should return -1 when the value is not present', function(){
      var array = [1,2,3];
      expect(array.indexOf(5)).to.equal(-1);
      expect(array.indexOf(0)).to.equal(-1);
    })
  })
})
```

Then in our `package.json` file we would add a test script to run with `npm test`.

```js
"scripts": {
  "test": "mocha"
}
```

### Automated Testing with Travis CI

We can automatically run our test suite for each commit using [Travis CI][travis].
Sign up, add your GitHub repository, then add a `.travis.yml` file to the repo.

```yaml
language: node_js
node_js:
- '0.10'
script:
- 'npm test' # tell Travis how to run your test suite
```

Every time a commit is pushed to your repository on any branch, Travis will
automatically queue up a test runner that clones your repository, installs all
dependencies, runs your test suite, and reports the status. You can even add a
[status badge][travis-badge] to your repos README that shows the current build status and acts as a
handy link for Travis. The best part is it's 100% free for public GitHub repos.

### Code Coverage with Istanbul

[Istanbul][istanbul] is the only viable JavaScript code coverage tool we are aware
of. It's fast, easy to use, can be installed via npm, and doesn't need to be installed
globally. To run, just add another entry in the `package.json` scripts field.

```js
  "coverage": "istanbul cover ./node_modules/mocha/bin/_mocha"
```

You'll notice that we have to call the mocha bin file directly. This is because
the main mocha file forks into a child process as [explained here][mocha-fork].

When you run `npm run coverage` it will generate coverage statistics in the
`coverage` directory including a browsable HTML report in `coverage/lcov-report/index.html`.
We suggest adding `coverage` to your `.gitignore` file so
that it's not committed to your repo.

### Automated Code Coverage with Coveralls

Similar to how we used [Travis CI][travis] to automate our testing, we can use
[Coveralls][coveralls] to automate our code coverage. The first step is to sign up
and enable your repository. This will generate a token which you need to add to a
`.coveralls.yml` file.

```yaml
repo_token: mycoverallsrepoymltoken
```

Second, you need to install the [coveralls node][coveralls-node] package. This
allows us to send our node coverage stats to Coveralls so that they can store
and process it.

Third, we need a new command in our `scripts` field:

```js
  "coveralls": "npm run coverage && cat ./coverage/lcov.info | ./node_modules/coveralls/bin/coveralls.js"
```

This command will run our test suite to gather coverage stats then pipe them into
the coveralls node package which sends the stats to coveralls.

The last thing to do is update `.travis.yml` so that our new coveralls command
is run automatically by Travis each time a commit is pushed to our repository.
This will still run tests and track our build status while also allowing coveralls
to track changes to our code coverage.

```yaml
script:
- 'npm run coveralls' # tell Travis how to run your test suite
```

An option step, though one we highly recommend, is to add a coverage badge to
your README that shows off your coverage percentage and adds a quick link to
Coveralls browsable coverage stats.

### Conclusion

Our full `scripts` field in `package.json` now looks like this:

```js
"scripts": {
  "test": "mocha",
  "coverage": "istanbul cover ./node_modules/mocha/bin/_mocha",
  "coveralls": "npm run coverage && cat ./coverage/lcov.info | ./node_modules/coveralls/bin/coveralls.js"
}
```

Of course, you can add additional scripts. We encourage you to browse the
documentation for [Mocha][mocha], [Travis CI][travis], [Istanbul][istanbul], 
and [Coveralls][coveralls] to see what else is possible.

See [FS-Traversal][fs-traversal] for a real example of the setup we described here.

[code-coverage]: http://en.wikipedia.org/wiki/Code_coverage
[mocha]: http://mochajs.org/
[chai]: http://chaijs.com/
[istanbul]: https://github.com/gotwarlost/istanbul
[mocha-fork]: https://github.com/gotwarlost/istanbul/issues/44
[travis]: https://travis-ci.org
[travis-badge]: http://docs.travis-ci.com/user/status-images/
[github]: https://github.com/
[coveralls]: https://coveralls.io/
[coveralls-node]: https://github.com/cainus/node-coveralls
[fs-traversal]: https://github.com/genealogysystems/fs-traversal/tree/16c306a0e0c168e59a875f2f1d74cb32a3f6bc8e