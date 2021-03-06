# Development Log

- Found out about [Node.gitignore][node-gitignore]. Also, Mark Wolfe
  [talked about a systematic way to start new projects][node-new-projects].
  But, I can't find the equivalent of `lein new [project-name]`... which
  sets up whole directories and a testing apparatus.

- I'm just gonna note down what I did (in the order I wish I had done it):

    ```
    $ mkdir new tongue-in-cheek
    $ git init
    $ mkdir src spec
    $ touch README.md .gitignore src/main.js spec/main_spec.js
    $ wget https://raw.githubusercontent.com/github/gitignore/master/Node.gitignore -O .gitignore
    $ npm init
    $ npm i mocha --save
    $ npm test # should be green
    $ git add . && git commit -a
    ```

- There are a number of functional programming libraries in Javascript. There's
  `fn.js`, `functional.js`, `mori`, and Oliver Steele's `functional`
  ([details][functional-javascript]). I picked Oliver Steele's because it looked
  like it would lead to compact, easy to read and write code. Used a
  [node port][functional-node] for that lib.

- Picking up my Ruby habit of having a `config/boot` module. Hmm, functional-
  node has a [weird looking shim][shim-eg]... I've seen something similar before,
  but this is the clearest example.

    ```javascript
    //node.js shim
    exports.load = function (parent) { return (function (window) {
    // random javascript
    })(parent || global); };
    ```

- Oh, the project tooling rabbit hole... I want to use Paper.js again (past
  experience  in my [socketyballs][socketyballs] experiment was nice). But it's
  a big honking component so I  should use Bower... and now the installation is
  a bit more complicated, but maintenance  a bit easier.

- Moved files around so that `doc/` contains all documentation and
  `README.md` is thinned out.

- Adopted the "architecture design record" practice; saw [a post][arch-design-records]
  earlier by Michael Nygard. But I don't really need this right now. This log is
  enough.

- I'm going to focus on the non-visulization bits and stick with defining the
  interfaces first. Perhaps try it out with the "smile"/"lips" component?
  Splines?

- Changing the test folder and updating scripts. Just sticking with defaults.

- Packaged up Oliver Steele's library into a Bower component!
  `github.com/gnarmis/functional-bower`

- I've now switched to a client-side-first approach. So, now I use Jasmine and
  test in `test.html`.

- Found a D3 Chernoff faces module, but it's very dense.
  [Here's the Google Groups discussion about it][d3-chernoff-discussion].
  [Here's a nice, complex example][d3-chernoff-eg] using this library.

- [Found a better commented face generation library][face-gen].

    I made a fiddle to explore this library. Nicely written. Here's an example
    JSON datum that fully describes a face in this library:

    ```javascript
    {"head":{"id":0},"eyebrows":[{"id":0,"lr":"l","cx":135,"cy":250},{"id":0,"lr":"r","cx":265,"cy":250}],"eyes":[{"id":1,"lr":"l","cx":135,"cy":280,"angle":4.55309689976275},{"id":1,"lr":"r","cx":265,"cy":280,"angle":4.55309689976275}],"nose":{"id":1,"lr":"l","cx":200,"cy":330,"size":0.623936983756721,"flip":true},"mouth":{"id":1,"cx":200,"cy":400},"hair":{"id":3},"fatness":0.812903706682846,"color":"#f2d6cb"}
    ```

    You just do `faces.display(container, face)`!

    Hard part: controlling various features with a particular number, and
    keeping the range of values for that number the same across all features.

    Seems like the approach is to append functions to various features which
    determine appearance of those features. See multiple
    `hair.push(function(...) {...})` calls. Then, in the face object, the
    "id" corresponds to the index of the function in the array correponding
    to that feature.

    My approach: work on a single feature and solve this interpolation
    problem. It doesn't exist on bower so I'll add it to bower first.

- Packaged up [faces.js][facesjs-bower]

- Switched to the d3 plugin and used React in tandom with some of my own code
  to make some stateless components and to provide a nice way to make and
  interact with Chernoff faces.

- Implemented everything needed for an interactive demo where you can twiddle
  values and see the Chernoff face update live!

- Re-architected things so I can test things more easily and move away from a
  single file world.

[node-gitignore]: https://raw.githubusercontent.com/github/gitignore/master/Node.gitignore
[node-new-projects]: http://www.wolfe.id.au/2014/02/01/getting-a-new-node-project-started-with-npm/
[functional-javascript]: http://osteele.com/sources/javascript/functional/
[functional-node]: https://github.com/bailus/functional-node
[shim-eg]: https://github.com/bailus/functional-node/blob/master/node_modules/functional-node/functional.js#L1-L2
[socketyballs]: https://github.com/gnarmis/socketyballs
[arch-design-records]: http://thinkrelevance.com/blog/2011/11/15/documenting-architecture-decisions
[d3-chernoff-discussion]: https://groups.google.com/forum/#!topic/d3-js/qiOZSnHouwM
[d3-chernoff-eg]: http://www.larsko.org/v/hpi/
[face-gen]: http://dumbmatter.com/facesjs/
[facesjs-bower]: https://github.com/gnarmis/facesjs
