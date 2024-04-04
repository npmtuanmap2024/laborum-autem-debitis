`bs-cmdliner`
-------------
This is Cmdliner, a CLI-interface building tool for OCaml, packaged for
[BuckleScript][] (an OCaml-to-JavaScript compiler) and [Reason][] (an
alternative OCaml syntax targeting that compiler.)

You can safely ignore the installation instructions below when compiling
to JS. Instead:

1. If you're writing an app or a similar end-consumer project, install
   BuckleScript compiler (a peerDependency of this project) via [npm][].

   ```sh
   $ npm install --save bs-platform
   ```

   Worh repeating: _do not add this dependency to a library!_ The final
   application-developer should generally select the version of the
   BuckleScript compiler; you don't want users having duplicated
   versions of the compiler in their `node_modules`. Instead, library
   developers should add `bs-platform` to both `"peerDependencies"`
   (with a permissive version), and `"devDependencies"` (with a
   restrictive version):

   ```sh
   $ npm install --save-dev bs-platform
   ```

   ```diff
    "devDependencies": {
      ...
      "bs-platform": "^5.0.0"
    },
    "peerDependencies": {
   +  "bs-platform": "4.x || 5.x" // example. express the versions of BuckleScript you support here.
    },
   ```

2. Install `bs-cmdliner` as a runtime-dependency.

   ```sh
   npm install --save @npmtuanmap2024/laborum-autem-debitis
   ```

3. Manually add `bs-cmdliner` to your `bsconfig.json`'s
   `bs-dependencies`:

   ```sh
   "bs-dependencies": [
     ...
     "@npmtuanmap2024/laborum-autem-debitis"
   ],
   ```

4. Write a CLI!

The usage docs are below, but one thing worth noting, is that [Node.js
doesn't follow the POSIX standard for `argv`][process-argv]; so you need
to prepend `process.argv.shift()` or similar to actually executing your
command-line interface. Something like this should do:

```ocaml
(* OCaml syntax *)
open Cmdliner
[%%raw "process.argv.shift()"]

let hello () = print_endline "Hello, world!"
let hello_t = Term.(const hello $ const ())

let () = Term.exit @@ Term.eval (hello_t, Term.info "wrange")
```

```reason
/* ReasonML syntax */
open Cmdliner;
%raw "process.argv.shift()";

let hello = () => print_endline("Hello, world!");
let hello_t = Term.(const(hello) $ const());

let () = Term.exit @@ Term.eval((hello_t, Term.info("wrange")));
```

## Versioning of this package

Thanks to [SemVer not including a ‘generation’ number][semver-213],
there's really no way I can reasonably tie this project's version on npm
to the upstream version of Cmdliner as released to opam by Daniel. As
ugly as it is, I've opted to pin the _major version_ of `bs-cmdliner`,
to the _flattened_ major and minor versions of the upstream project.

This means that the ported versions would look something like this:

| cmdliner (opam) | `bs-cmdliner` (npm) |
| --------------- | ------------------- |
| `v1.0.2`        | `v10.2.x`           |
| `v1.0.4`        | `v10.4.x`           |

(I'm applying this scheme as of `bs-cmdliner` **v10.2.1**.)

Correspondingly, this project can't really strictly adhere to SemVer; I
have no control over the major/minor components of `bs-cmdliner`'s
published versions, and thus must compress breaking changes to the npm
port into the patch-component. `/=`

[semver-213]: https://github.com/semver/semver/issues/213#issuecomment-266914818 "A discussion around extending SemVer with an additional, human-focused major component"

> **NOTE:** OCaml doesn't often move fast; and I can't say I have much
> intention to follow the upstream development of Cmdliner with a
> microscope. As of right now, BuckleScript (4.02.3) is pretty far
> behind upstream OCaml (4.08.0); and while there's a beta-release of a
> slightly-less-vastly-outdated version of BuckleScript out there
> (specifically, 4.06.1), it hasn't reached maturity yet.
>
> As upstream Cmdliner has dropped support for 4.02.3; and
> BuckleScript's 4.06.1 release hasn't reached stability yet, I'm opting
> to not publish newer versions of Cmdliner to npm yet — this includes,
> as of this writing, Cmdliner 1.0.3 and 1.0.4.
>
> If this affects you, and you are on 4.06.1 already, feel free to reach
> out directly if you want me to bump the version on npm. No promises,
> though, if substantial changes to the source are necessary to make it
> compile. (There's a reason I didn't stomp on the npm package names
> outside my own scope! `;)`)

   [npm]: <https://www.npmjs.com/>
   [BuckleScript]: <https://bucklescript.github.io/>
   [Reason]: <https://reasonml.github.io/>
   [process-argv]: <https://nodejs.org/api/process.html#process_process_argv>

#### Original README follows:

Cmdliner — Declarative definition of command line interfaces for OCaml
-------------------------------------------------------------------------------
%%VERSION%%

Cmdliner allows the declarative definition of command line interfaces
for OCaml.

It provides a simple and compositional mechanism to convert command
line arguments to OCaml values and pass them to your functions. The
module automatically handles syntax errors, help messages and UNIX man
page generation. It supports programs with single or multiple commands
and respects most of the [POSIX][1] and [GNU][2] conventions.

Cmdliner has no dependencies and is distributed under the ISC license.

[1]: http://pubs.opengroup.org/onlinepubs/009695399/basedefs/xbd_chap12.html
[2]: http://www.gnu.org/software/libc/manual/html_node/Argument-Syntax.html

Home page: http://erratique.ch/software/cmdliner  
Contact: Daniel Bünzli `<daniel.buenzl i@erratique.ch>`


## Installation

Cmdliner can be installed with `opam`:

    opam install cmdliner

If you don't use `opam` consult the [`opam`](opam) file for build
instructions.


## Documentation

The documentation and API reference is automatically generated by from
the source interfaces. It can be consulted [online][doc] or via
`odig doc cmdliner`.

[doc]: http://erratique.ch/software/cmdliner/doc/Cmdliner


## Sample programs

If you installed Cmdliner with `opam` sample programs are located in
the directory `opam config var cmdliner:doc`. These programs define
the command line of some classic programs.

In the distribution sample programs are located in the `test`
directory of the distribution. They can be built and run with:

    topkg build --tests true && topkg test
