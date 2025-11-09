# Knip Documentation

_Scraped from knip.dev_

_Total pages: 49_

---

# A Brief History Of Knip

**Source:** https://knip.dev/blog/brief-history

---

# A Brief History Of Knip

_Published: 2023-10-15_

If you are fond of short lists and brief histories, then this page was written
just for you!

- 2022-10-04: The [initial commit](https://github.com/webpro-nl/knip/commit/9589dfe22608da7d89f2613383da6db5826226d2). Still so tiny at that point, but the seed
  was planted. Starting out with finding unused files and exports, the name was
  Exportman! 🦸

- 2022-10-09: Big plans and a rename 5 days later, the first published version
  of Knip was [v0.1.2](https://github.com/webpro-nl/knip/tree/0.1.2).

- 2022-11-22: Unused dependencies and support for workspaces and plugins in the
  [first alpha of v1](https://github.com/webpro-nl/knip/releases/tag/1.0.0-alpha.0).

- 2023-01-10: Lots of testing and fixes led to [Knip v1](https://github.com/webpro-nl/knip/tree/1.0.0).

- 2023-03-22: [Knip v2](https://github.com/webpro-nl/knip/issues/73) saw a full rewrite of the TypeScript backend.

- 2023-10-15: [Introduction of Knip v3](./knip-v3).

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/blog/brief-history.md)

[
Previous
Slim down to speed up ](/blog/slim-down-to-speed-up) [
Next
Announcing Knip v3 ](/blog/knip-v3)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Announcing Knip v3

**Source:** https://knip.dev/blog/knip-v3

---

# Announcing Knip v3

_Published: 2023-10-15_

Lots of new users got introduced to Knip, coming with clear bug reports, helpful
insights, superb reproductions and great suggestions this year. You’re all a
friendly and helpful bunch! Recently I’ve opened a Discord channel where more
communication, collaboration, ideas and updates are happening: feel free to join
[The Knip Barn](https://discord.gg/r5uXTtbTpc)!

Today, Knip has [over 140k weekly downloads on npm](https://www.npmjs.com/package/knip), [almost 4000 stars on
GitHub](https://github.com/webpro-nl/knip/stargazers), and [over 500 repositories](https://github.com/webpro-nl/knip/network/dependents) using it. While numbers are just
numbers, they do add to the positive feedback I’m receiving daily. Everything
combined makes me think I’m on the right track which is very motivating to keep
working on Knip.

## So… What’s Been Cooking Lately?

[
Section titled “So… What’s Been Cooking Lately?”](#so-whats-been-cooking-lately)

- Migration to a monorepo setup

- This very website built with Starlight 🌟

- Extended documentation for just about everything

- Improved JSON reporter for external integrations (e.g. [GitHub Action](https://github.com/marketplace/actions/knip-reporter))

- Some breaking changes, but you probably don’t need to make any changes

## Breaking Changes

[
Section titled “Breaking Changes”](#breaking-changes)

A major bump comes with breaking changes, but most likely no changes necessary
on your end:

- Removed support for Node.js v16, Knip v3 requires at least Node.js v18.6

- Simplified [exit codes](../reference/cli#exit-code)

- [Production mode](../features/production-mode) now includes types by default (add `--exclude types` for
  previous behavior)

- Removed `--ignore-internal` flag; [`@internal`](../reference/jsdoc-tsdoc-tags#internal) exports ignored in
  production mode now

- The `--debug-file-filter` flag is removed

- The `jsonExt` reporter is now the default [JSON reporter](../features/reporters#json) (the previous one
  is gone)

- Moved `typescript` to `peerDependencies` (requires `>=5.0.4`)

## Installation

[
Section titled “Installation”](#installation)

Try out the latest Knip v3 release today!

- [ npm ](#tab-panel-0)
- [ pnpm ](#tab-panel-1)
- [ bun ](#tab-panel-2)
- [ yarn ](#tab-panel-3)

Terminal window

```

npm install -D knip

```

Terminal window

```

pnpm add -D knip

```

Terminal window

```

bun add -D knip

```

Terminal window

```

yarn add -D knip

```

Remember, Knip it before you ship it! Have a great day ☀️

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/blog/knip-v3.mdx)

[
Previous
A Brief History Of Knip ](/blog/brief-history) [
Next
Release Notes v2 ](/blog/release-notes-v2)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Announcing Knip v4

**Source:** https://knip.dev/blog/knip-v4

---

# Announcing Knip v4

_Published: 2024-01-16_

I’m happy to announce that Knip v4 is available!

The work took over a month and the process of [slimming down to speed up](./slim-down-to-speed-up)
ended up really well: significant faster runs and reduced memory usage. In the
meantime, v3 continued to receive more contributions, plugins and bug fixes.

## Highlights

[
Section titled “Highlights”](#highlights)

Compared to v3, here are the highlights:

- Performance: significant speed bump (up to 80%!)

- Performance: globbing in combo with `.gitignore` is a lot more efficient

- Configuration: [built-in compilers](../features/compilers) (for Astro, MDX, Svelte & Vue)

- The `ignore` option has been improved

- Internal refactoring to serialize data for future improvements like caching.

The actual performance win in your projects depends on various factors like size
and complexity.

## Major Changes

[
Section titled “Major Changes”](#major-changes)

The changes have been tested against various repositories, but it’s possible
that you will encounter false positives caused by the major refactoring that has
been done. If you do, [please report](../guides/issue-reproduction)!

### Unused Class Members

[
Section titled “Unused Class Members”](#unused-class-members)

Finding unused class members is no longer enabled by default. Here’s why it’s
now opt-in:

- When using Knip for the first time on a large repository it can crash after a
  while with an out of memory error. This is a terrible experience.

- Plenty of codebases don’t use classes at all, keeping TS programs in memory is
  a waste of resources.

- Many configurations already exclude `classMembers` from the output.

Enable unused class members by using the CLI argument or the configuration
option:

- Terminal window

```

knip --include classMembers

```

```

{

  "include": ["classMembers"]

}

```

Now that unused class members is opt-in and better organized within Knip, it
might be interesting to start looking at opt-ins for other unused members, such
as those of types and interfaces.

By the way, enum members are “cheap” with the v4 refactor, so those are still
included by default.

### Compilers

[
Section titled “Compilers”](#compilers)

You can remove the `compilers` option from your configuration. Since you can
override them, your custom compilers can stay where they are. This also means
that you can go back from `knip.ts` to `knip.json` if you prefer.

### Ignore Files

[
Section titled “Ignore Files”](#ignore-files)

The `ignore` option accepted patterns like `examples/`, but if you want to
ignore the files inside this folder you should update to globs like
`examples/**`.

## What’s Next?

[
Section titled “What’s Next?”](#whats-next)

The refactoring for this release opens the door to more optimizations, such as
caching. I’m also very excited to see how deeper integrations such as in GitHub
Actions or IDEs like VS Code or WebStorm may further develop.

Remember, if you are you using Knip at work your company can [sponsor me](https://github.com/sponsors/webpro)!

## One More Thing…

[
Section titled “One More Thing…”](#one-more-thing)

An idea I’ve been toying with is “tagged exports”. The idea is that you can tag
exports in a JSDoc comment. The tag does not need to be part of the JSDoc or
TSDoc spec. For example:

```

/** @custom */

export const myExport = 1;

```

Then, include or exclude such tagged exports from the report like so:

Terminal window

```

knip --experimental-tags=+custom

knip --experimental-tags=-custom,-internal

```

This way, you can either focus on or ignore specific tagged exports with tags
you define yourself. This also works for individual class or enum members.

Once this feature is intuitive and stable, the `experimental` flag will be
removed and option(s) added to the Knip configuration file. The docs are in the
[CLI reference](../reference/cli#--experimental-tags).

## Let’s Go!

[
Section titled “Let’s Go!”](#lets-go)

What are you waiting for? Start using Knip v4 today!

[ npm ](#tab-panel-4)

- [ pnpm ](#tab-panel-5)
- [ bun ](#tab-panel-6)
- [ yarn ](#tab-panel-7)

Terminal window

```

npm install -D knip

```

Terminal window

```

pnpm add -D knip

```

Terminal window

```

bun add -D knip

```

Terminal window

```

yarn add -D knip

```

Remember, Knip it before you ship it! Have a great day ☀️

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/blog/knip-v4.mdx)

[
Previous
Announcing Knip v5 ](/blog/knip-v5) [
Next
Slim down to speed up ](/blog/slim-down-to-speed-up)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Announcing Knip v5

**Source:** https://knip.dev/blog/knip-v5

---

# Announcing Knip v5

_Published: 2024-02-10_

Today brings the smallest major release so far. Tiny yet mighty!

Below are two cases to demonstrate the change in how unused exports are
reported.

## Case 1

[
Section titled “Case 1”](#case-1)

The first case shows two exports with a namespaced import that references one of
those exports explicitly:

- knip.js

```

export const version = 'v5';

export const getRocket = () => '🚀';

```

index.js

```

import * as NS from './knip.js';

console.log(NS.version);

```

In this case we see that `getRocket` is an unused export.

Previously it would go into the “Unused exports in namespaces” category
(`nsExports`). This issue has been moved to the “Unused exports” category
(`exports`).

## Case 2

[
Section titled “Case 2”](#case-2)

The second case is similar, but only the imported namespace itself is
referenced. None of the individual exports is referenced:

index.js

```

import * as NS from './knip.js';

import send from 'stats';

send(NS);

```

Are the `version` and `getRocket` exports used? We can’t know. The same is true
for the spread object pattern:

index.js

```

import * as NS from './knip.js';

const Spread = { ...NS };

```

Previously those exports would go into the “Unused exports in namespaces”
category. This is still the case, but this category is no longer enabled by
default.

## Include unused exports in namespaces

[
Section titled “Include unused exports in namespaces”](#include-unused-exports-in-namespaces)

To enable this type of issues in Knip v5, add this argument to the command:

Terminal window

```

knip --include nsExports

```

Or in your configuration file:

knip.json

```

{

  "include": ["nsExports", "nsTypes"]

}

```

Now `version` and `getRocket` will be reported as “Unused exports in
namespaces”.

Note that `nsExports` and `nsTypes` are split for more granular control.

## Handling exports in namespaced imports

[
Section titled “Handling exports in namespaced imports”](#handling-exports-in-namespaced-imports)

You have a few options to handle namespaced imports when it comes to unused
exports.

### 1. Use named imports

[
Section titled “1. Use named imports”](#1-use-named-imports)

Regardless of whether `nsExports` is enabled or not, it’s often good practice to
replace the namespaced imports with named imports:

index.js

```

import { version, getRocket } from './knip.js';

send({ version, getRocket });

```

Whenever possible, explicit over implicit is often the better choice.

### 2. Standardized JSDoc tags

[
Section titled “2. Standardized JSDoc tags”](#2-standardized-jsdoc-tags)

Using one of the available JSDoc tags like `@public` or `@internal`:

knip.js

```

export const version = 'v5';

/** @public */

export const getRocket = () => '🚀';

```

Assuming only imported using a namespace (like in the example cases above), this
will exclude the `getRocket` export from the report, even though it isn’t
explicitly referenced.

### 3. Arbitrary JSDoc tags

[
Section titled “3. Arbitrary JSDoc tags”](#3-arbitrary-jsdoc-tags)

Another solution is to tag individual exports arbitrarily:

knip.js

```

export const version = 'v5';

/** @launch */

export const getRocket = () => '🚀';

```

And then exclude the tag like so:

Terminal window

```

$ knip --experimental-tags=-launch

Exports in used namespace (1)

version  NS  unknown  knip.js:1:1

```

Assuming only imported using a namespace (like in the example cases above), this
will exclude the `getRocket` export from the report, even though it isn’t
explicitly referenced.

## A better default

[
Section titled “A better default”](#a-better-default)

I believe this behavior in v5 is the better default: have all exports you want
to know about in a single category, and those you probably want to ignore in
another that’s disabled by default.

Before the [v4 refactoring](../blog/slim-down-to-speed-up), this would be a lot harder to implement. That
refactoring turns out to be a better investment than expected. Combined with a
better understanding of how people write code and use Knip, this change is a
natural iteration.

Why the major bump? It’s not breaking for the large majority of users, but for
some it may be breaking. For instance when relying on the [JSON reporter](../features/reporters#json),
other reporter output, or custom [preprocessing](../features/reporters#preprocessors). It’s not a bug fix, it’s
not a new feature, but since semver is all about setting expectations I feel the
change is large enough to warrant a major bump.

## Let’s Go!

[
Section titled “Let’s Go!”](#lets-go)

What are you waiting for? Start using Knip v5 today!

[ npm ](#tab-panel-8)

- [ pnpm ](#tab-panel-9)
- [ bun ](#tab-panel-10)
- [ yarn ](#tab-panel-11)

Terminal window

```

npm install -D knip

```

Terminal window

```

pnpm add -D knip

```

Terminal window

```

bun add -D knip

```

Terminal window

```

yarn add -D knip

```

Remember, Knip it before you ship it! Have a great day ☀️

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/blog/knip-v5.mdx)

[
Previous
Two Years ](/blog/two-years) [
Next
Announcing Knip v4 ](/blog/knip-v4)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Migration to v1

**Source:** https://knip.dev/blog/migration-to-v1

---

# Migration to v1

_2023-01-04_

When coming from version v0.13.3 or before, there are some breaking changes:

- The `entryFiles` and `projectFiles` options have been renamed to `entry` and
  `project`.

- The `--dev` argument and `dev: true` option are gone, this is now the default
  mode (see [production mode](../features/production-mode)).

- Workspaces have been moved from the root of the config to the `workspaces` key
  (see [workspaces](../features/monorepos-and-workspaces)).

- The `--dir` argument has been renamed to `--workspace`.

## Example

[
Section titled “Example”](#example)

A configuration like this in v0.13.3 or before…

-

```

{

  "entryFiles": ["src/index.ts"],

  "projectFiles": ["src/**/*.ts", "!**/*.spec.ts"],

  "dev": {

    "entryFiles": ["src/index.ts", "src/**/*.spec.ts", "src/**/*.e2e.ts"],

    "projectFiles": ["src/**/*.ts"]

  }

}

```

…should become this for v1…

```

{

  "entry": ["src/index.ts!"],

  "project": ["src/**/*.ts!"]

}

```

Much cleaner, right? For some more details:

The `dev` property for the `--dev` flag is now the default mode.

- Use `--production` to analyze only the `entry` and `project` files suffixed
  with `!`.

- The glob patterns for both types of test files (`*.spec.ts` and `*.e2e.ts`)
  are no longer needed:

Regular test files like `*.test.js` and `*.spec.ts` etc. are automatically
handled by Knip.

- The `*.e2e.ts` files is configured with the Cypress or other plugin. Note
  that Cypress uses `*.cy.ts` for spec files, but this could be overridden
  like so:

```

{

  "entry": "src/index.ts!",

  "project": "src/**/*.ts!",

  "cypress": {

    "entry": "src/**/*.e2e.ts"

  }

}

```

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/blog/migration-to-v1.md)

[
Previous
Release Notes v2 ](/blog/release-notes-v2) [
Next
Unused dependencies ](/typescript/unused-dependencies)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Release Notes v2

**Source:** https://knip.dev/blog/release-notes-v2

---

# Release Notes v2

_2023-03-22_

## Breaking changes

[
Section titled “Breaking changes”](#breaking-changes)

When coming from v1, there are no breaking changes in terms of configuration.

## Changes

[
Section titled “Changes”](#changes)

There are some changes regarding CLI arguments and output:

- Knip now runs on every [workspace][1] automatically (except for the ones in
  `ignoreWorkspaces: []`).

- The “Unlisted or unresolved dependencies” is split in “Unlisted dependencies”
  and “Unresolved imports”.

- Bug fixes and increased correctness impact output (potentially causing CI to
  now succeed or fail).

## New features

[
Section titled “New features”](#new-features)

Rewriting a major part of Knip’s core from scratch allows for some new exciting
features:

- **Performance**. Files are read only once, and their ASTs are traversed only
  once. Projects of any size will notice the difference. Total running time for
  some projects decreases with 90%.

- **Compilers**. You can now include other file types such as `.mdx`, `.vue` and
  `.svelte` in the analysis.

Internally, the `ts-morph` dependency is replaced by `typescript` itself.

## Other improvements

[
Section titled “Other improvements”](#other-improvements)

- Improved support for workspaces.

- Improved module resolutions, self-referencing imports, and other things you
  don’t want to worry about.

- Configure `ignoreDependencies` and `ignoreBinaries` at the workspace level.

- Simplified plugins model: plugin dependency finder may now return any type of
  dependency in a single array: npm packages, local workspace packages, local
  files, etc. (module and path resolution are handled outside the plugin).

- Many bugfixes.

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/blog/release-notes-v2.md)

[
Previous
Announcing Knip v3 ](/blog/knip-v3) [
Next
Migration to v1 ](/blog/migration-to-v1)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Slim down to speed up

**Source:** https://knip.dev/blog/slim-down-to-speed-up

---

# Slim down to speed up

_Published: 2023-12-14_

**tl;dr;** Memory usage is up to 50% lower, runs are up to 60% faster and you
can start using v4 canary today. No “unused class members” for the time being,
but this feature is planned to be restored.

## Introduction

[
Section titled “Introduction”](#introduction)

Honestly, performance has always been a challenge for Knip. A longstanding
bottleneck has finally been eliminated and Knip is going to be a lot faster.
Skip straight to the bottom to install v4 canary and try it out! Or grab
yourself a nice drink and read on if you’re interested in where we are coming
from, and where we are heading.

## Projects & Workspaces

[
Section titled “Projects & Workspaces”](#projects--workspaces)

From the start, Knip has relied on TypeScript for its robust parser for
JavaScript and TypeScript files. And on lots of machinery important to Knip,
like module resolution and accurately finding references to exported values.
Parts of it can be customized, such as the (virtual) file system and the module
resolver.

In TypeScript terms, a “project” is like a workspace in a monorepo. Same as each
workspace has a `package.json`, each project has a `tsconfig.json`. The
`ts.createProgram()` method is used to create a program based on a
`tsconfig.json` and the machinery starts to read and parse source code files,
resolve modules, and so on.

Up until v2, when Knip wanted to find unused things in a monorepo, all programs
for all workspaces were loaded into memory. Workspaces often depend on each
other, so Knip couldn’t load one project, analyze it and dispose it. This way,
connections across workspaces would be lost.

## Shared Workspaces

[
Section titled “Shared Workspaces”](#shared-workspaces)

Knip v2 said goodbye to this approach and implemented its own TypeScript backend
(after using `ts-morph` for this). Based on the compatibility of
`compilerOptions`, workspaces were merged into shared programs whenever
possible. Having less programs in memory led to significant performance
improvements. Yet ultimately it was still a stopgap, since everything was still
kept in memory for the duration of the process.

“Why does everything need to stay in memory?”, you may wonder. The answer is
that Knip uses `findReferences` at the end of the process. Knip relied on this
TypeScript Language Server method for everything that’s not easy to find. More
about that later in [the story of findReferences](#the-story-of-findreferences)

## Serialization

[
Section titled “Serialization”](#serialization)

Fortunately, everything that’s imported and exported from source files
(including things like members of namespaces and enums) can be found relatively
easily during AST traversal. This way, references to exports don’t have to be
“traced back” later on.

It’s mostly class members that are harder to find due to their dynamic nature.
Without these, all information can be serialized for storage and retrieval (in
memory or on disk). Slimming down by taking class members out of the equation
simplifies things a lot and paves the way for all sorts of improvements.

## We Have To Slim Down

[
Section titled “We Have To Slim Down”](#we-have-to-slim-down)

The relevant part in the linting process can be summarized in 5 steps:

- Collect entry files and feed them to TypeScript

- Read files, resolve modules, and create ASTs

- Traverse ASTs and collect imports & exports

- Match exports against imports to determine what’s unused

- Find references to hard-to-find exported values and members

If we would hold on to reporting unused class members, then especially steps 2
and 5 are hard to decouple. The program and the language service containing the
source files used to eventually trace back references can’t really be decoupled.
So class members had to go. Sometimes you have to slim down to keep moving. One
step back, two steps forward.

If you rely on this feature, fear not. I plan to bring it back before the final
v4, but possibly behind a flag.

## What’s In Store?

[
Section titled “What’s In Store?”](#whats-in-store)

So with this out of the way, everything becomes a lot clearer and we can finally
really start thinking about significant memory and performance improvements. So
what’s in store here? A lot!

- We no longer need to keep everything in memory, so workspaces are read and
  disposed in isolation, one at a time. Memory usage will be spread out more
  even. This does not make it faster, but reducing “out of memory” issues is
  definitely a Good Thing™️ in my book.

- Knip could recover from unexpected exits and continue from the last completed
  workspace.

- The imports and exports are in a format that can be serialized for storage and
  retrieval. This opens up interesting opportunities, such as local caching on
  disk, skipping work in subsequent runs, remote caching, and so on.

- Handling workspaces in isolation and serialization result in parallelization
  becoming a possibility. This becomes essential, as module resolution and AST
  creation and traversal are now the slowest parts of the process and are not
  easy to optimize significantly (unless perhaps switching to e.g Rust).

- No longer relying on `findReferences` speeds up the export/import matching
  part part significantly. So far I’ve seen **improvements of up to 60% on total
  runtime**, and my guess is that some larger codebases may profit even more.

- The serialization format is still being explored and there is no caching yet,
  but having the steps more decoupled is another Good Thing™️ that future me
  should be happy about.

## Back It Up, Please

[
Section titled “Back It Up, Please”](#back-it-up-please)

I heard you. Here’s some example data. You can get it directly from Knip using
the `--performance` flag when running it on any codebase. Below we have some
data after linting the [Remix monorepo](https://github.com/remix-run/remix).

### Knip v3

[
Section titled “Knip v3”](#knip-v3)

- Terminal window

```

$ knip --performance

Name                           size  min     max      median   sum

-----------------------------  ----  ------  -------  -------  -------

findReferences                  223    0.55  2252.35     8.46  5826.95

createProgram                     2   50.78  1959.92  1005.35  2010.70

getTypeChecker                    2    5.04   667.45   336.24   672.48

getImportsAndExports            396    0.00     7.19     0.11   104.46

Total running time: 9.7s (mem: 1487.39MB)

```

### Knip v4

[
Section titled “Knip v4”](#knip-v4)

Terminal window

```

$ knip --performance

...

Name                           size  min     max      median   sum

-----------------------------  ----  ------  -------  -------  -------

createProgram                     2   54.36  2138.45  1096.40  2192.81

getTypeChecker                    2    7.40   664.83   336.12   672.23

getImportsAndExports            396    0.00    36.36     0.16   224.37

getSymbolAtLocation            2915    0.00    29.71     0.00    65.63

Total running time: 4.3s (mem: 729.67MB)

```

### Takeaways

[
Section titled “Takeaways”](#takeaways)

The main takeaways here:

In v3,`findReferences` is where Knip potentially spends most of its time

- In v4, total running time is down over 50%

- In v4, memory usage is down 50% (calculated using
  `process.memoryUsage().heapUsage`)

- In v4, `getImportsAndExports` is more comprehensive to compensate for the
  absence of `findReferences` - more on that below

Remember, unused class members are no longer reported by default in v4.

## The story of `findReferences`

[
Section titled “The story of findReferences”](#the-story-of-findreferences)

Did I mention Knip uses `findReferences`…? Knip relied on it for everything
that’s not easy to find. Here’s an example of an export/import match that **is**
easy to find:

import.ts

```

import { MyThing } from './thing.ts';

```

export.ts

```

export const MyThing = 'cool';

```

In v2 and v3, Knip collects many of such easy patterns. Other patterns are
harder to find with static analysis. This is especially true for class members.
Let’s take a look at the next example:

MyClass.ts

```

class MyClass {

  constructor() {

    this.method();

  }

  method() {}

  do() {}

}

export const OtherName = MyClass;

```

instance.ts

```

import * as MyNamespace from './MyClass.ts';

const { OtherName } = MyNamespace;

const instance = new OtherName();

instance.do();

```

Without a call or `new` expression to instantiate `OtherName`, its `method`
member would not be used (since the constructor would not be executed). To
figure this out using static analysis goes a long way. Through export
declarations, import declarations, aliases, initializers, call expressions…
the list goes on and on. Yet all this magic is exactly what happens when you use
“Find all references” or “Go to definition” in VS Code.

Knip used `findReferences` extensively, but it’s what makes a part of Knip
rather slow. TypeScript needs to wire things up (through
`ts.createLanguageService` and `program.getTypeChecker`) before it can use this,
and then it tries hard to find all references to anything you throw at it. It
does this very well, but the more class members, enum members and namespaced
imports your codebase has, the longer it inevitably takes to complete the
process.

Besides letting go of class members, a slightly more comprehensive AST traversal
is required to compensate for the absence of `findReferences` (it’s the
`getImportsAndExports` function in the metrics above). I’d like to give you an
idea of what “more comprehensive” means here.

In the following example, `referencedExport` was stored as export from
`namespace.ts`, but it was not imported directly as such:

namespace.ts

```

export const referencedExport = () => {};

```

index.ts

```

import * as NS from './namespace.ts';

NS.referencedExport();

```

Previously, Knip used `findReferences()` to “trace back” the usage of the
exported `referencedExport`.

The gist of the optimization is to pre-determine all imports and exports. During
AST traversal of `index.ts` , Knip sees that `referencedExport` is attached to
the imported `NS` namespace, and stores that as an imported identifier of
`namespace.ts`. When matching exports against imports, this lookup comes at no
extra cost. Additionally, this can be stored as strings, so it can be serialized
too. And that means it can be cached.

Knip already did this for trivial cases as shown in the first example of this
article. This has now been extended to cover more patterns. This is also what
needs to be tested more extensively before v4 can be released. Its own test
suite and the projects in the integration tests are already covered so we’re
well on our way.

For the record, `findReferences` is an absolute gem of functionality provided by
TypeScript. Knip is still backed by TypeScript, and tries to speed things up by
shaking things off. In the end it’s all about trade-offs.

## Let’s Go!

[
Section titled “Let’s Go!”](#lets-go)

You can start using Knip v4 today, feel free to try it out! You might find a
false positive that wasn’t there in v3, please [report this](https://github.com/webpro-nl/knip/issues).

Terminal window

```

npm install -D knip@canary

```

Remember, Knip it before you ship it! Have a great day ☀️

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/blog/slim-down-to-speed-up.md)

[
Previous
Announcing Knip v4 ](/blog/knip-v4) [
Next
A Brief History Of Knip ](/blog/brief-history)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# The State of Knip

**Source:** https://knip.dev/blog/state-of-knip

---

# The State of Knip

_Published: 2025-02-28_

Honestly, Knip was a bit of a “cursed” project from the get-go. Getting anywhere
near a level of being broadly-ish valuable requires a good amount of
foolishness determination, and it has always been clear it would stay far
from perfect. It’s telling that most of [similar projects](../explanations/comparison-and-migration) have been
abandoned.

And even though Knip is in its infancy, this update is meant as a sign we feel
we’re still on to something. External indicators include increased usage looking
at numbers such as dependent repositories on GitHub and weekly downloads on npm,
and bug reports about increasingly less rudimentary issues.

## Two Cases

[
Section titled “Two Cases”](#two-cases)

For those interested, let’s take a look at two cases that hopefully give an
impression of how Knip works under the hood and the level of issues we’re
currently dealing with. It’s assumed you already have a basic understanding of
Knip (otherwise please consider to read at least [entry files](../explanations/entry-files) and
[plugins](../explanations/plugins) first).

### Case 1: Next.js

[
Section titled “Case 1: Next.js”](#case-1-nextjs)

Let’s say this default configuration represents, greatly simplified, [the
default `entry` patterns](../reference/plugins/next#default-configuration) for projects using Next.js:

-

```

{

  "next": {

    "entry": ["next.config.ts", "src/pages/**/*.tsx"]

  }

}

```

Those files will be searched for and then statically analyzed to collect
`import` statements and find other local files and external dependencies. This
is the generic way Knip handles all source files.

However, the game changes if the project uses the following Next.js
configuration:

next.config.ts

```

const nextConfig = {

  pageExtensions: ['page.tsx'],

};

export default nextConfig;

```

Next.js will now look for files matching `src/pages/**/*.page.tsx` instead (note
the subtle change of the glob pattern). Knip should respect this to find used
and unused files properly.

Moving the burden to users for them to either not notice at all and get
incorrect results, or having to override the `next.entry` patterns and include
`src/pages/**/*.page.tsx` isn’t good DX. Knip should take care of it.

To get the configuration object and the value of `pageExtensions`, Knip has to
actually load and execute `next.config.ts` ¹… and trouble is right around the
corner:

next.config.ts

```

const nextConfig = {

  pageExtensions: ['page.tsx'],

  env: {

    BASE_URL: process.env.BASE_URL.toLowerCase(),

  },

};

export default nextConfig;

```

Terminal window

```

$ knip

💥 LoaderError: Error loading next.config.ts

💥 Reason: Cannot read properties of undefined (reading 'toLowerCase')

```

Obviously a contrived example, but the gist is that lots of tooling
configuration expects environment variables to be defined. But when running Knip
there might not be a mechanism to set those. Clearly a breaking change when Knip
starts doing this, only for Next.js projects with a configuration file that
doesn’t read environment variables safely (or has other contextual
dependencies).

By the way, [the ESLint v9 plugin](../reference/plugins/eslint#eslint-v9) has a similar issue.

¹ Another approach could be to statically analyze the `next.config.ts`
configuration file. That would require some additional efforts and get us only
so far, but is definitely useful in some cases and on the radar.

**EDIT:** This has been solved in the Next.js plugin in v5.48.0.

### Case 2: Knip does that?!

[
Section titled “Case 2: Knip does that?!”](#case-2-knip-does-that)

To further bring down user configuration and the number of false positives, the
system required more components. New components have been introduced to keep
improving and nail it for an increasing number of projects. This case is an
illustration of some of those components.

Let’s just dive into this example and find out what’s happening:

package.json

```

{

  "scripts": {

    "test": "yarn --cwd packages/frontend vitest -c vitest.components.config.ts"

  }

}

```

Orchestration is necessary between various components within Knip, such as:

Plugins, the Vitest plugin parses `vitest.components.config.ts`

- Custom CLI argument parsing for executables, e.g. `yarn --cwd [dir]` and
  `vitest --config [file]`

- The workspace graph, to see `packages/frontend` is a descendant workspace of
  the root workspace

Patterns like in the script above do not occur only in `package.json` files, but
could be anywhere. Here’s a similar example in a GitHub Actions workflow:

.github/workflows/test.yml

```

jobs:

  integration:

    runs-on: ubuntu-latest

    steps:

      - run: playwright test -c playwright.e2e.config.ts

        working-directory: e2e

```

The pattern is very similar, because Knip needs to assign a configuration file
to a specific workspace (assuming there’s one in `./e2e`) and apply the Vitest
configuration to that particular workspace with its own set of directory and
entry file patterns.

An essential part of Knip is to build up the module graph for source files. With
the configuration files still in mind, this is the pattern Knip follows towards
this goal:

- Find configuration files at default and custom locations

- Assign them to the right workspace

- Run plugins in their own workspace to take entry file patterns from the
  configuration objects

- Load and parse configuration files to get referenced dependencies

The referenced dependencies are stored in the `DependencyDeputy` class to
eventually determine what dependencies are unused or missing in `package.json`
in each workspace.

Both the configuration and entry files are then used to start building up the
module graph.

## Comprehensive

[
Section titled “Comprehensive”](#comprehensive)

Discussing the two cases briefly covers only part of the whole process. This
might give a sense of the reason why Knip is pretty comprehensive. After all,
building the module graph for internal source files to find unused files and
exports requires the list of external dependencies including internal
workspaces. And on the other hand, a complete module graph is required to find
unused or missing external dependencies.

The comprehensiveness also requires a range of components in the system, such as
the aforementioned ones, [compilers for popular frameworks](../features/compilers) and a [script
parser](../features/script-parser), and other affordances such as [auto-fix](../features/auto-fix).

That said, code organization could be improved to make it more accessible for
contributions and, for instance, expose programmatic APIs to use the generated
module graph outside of Knip. Additionally, existing plugins can better take
advantage of existing components in the system, and new plugins can be developed
to further reduce user configuration and false positives.

## The End

[
Section titled “The End”](#the-end)

That’s all for today, thanks for reading! Have a great one, and don’t forget:
Knip it before you ship it! ✂️

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/blog/state-of-knip.md)

[
Previous
Related Tooling ](/reference/related-tooling) [
Next
Two Years ](/blog/two-years)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Two Years

**Source:** https://knip.dev/blog/two-years

---

# Two Years

_Published: 2024-10-04_

Exactly two years ago the first commit was pushed to GitHub and the first
version of Knip was published to the npm registry. The name was initially
[Exportman](https://www.npmjs.com/package/exportman/v/0.0.1)! We’ve come a loooong way… The JavaScript ecosystem is highly
dynamic and I’ve been crazy enough to even start, try and keep up with it! But
here we are.

October 4th is World Animal Day, so there was really no choice but bring in the
crazy mascot that early adopters may remember:

Today we celebrate an unknown but CRAZY amount of clutter removed from so many
codebases with Knip’s help. Every single day I see many of those little red
blocks for thousands of lines of deleted code and dependencies. Call me crazy,
but to me this is pure joy and never gets old!    🟩 🟥 🟥 🟥 🟥

Hooray, release the clutter!

## Smiling faces

[
Section titled “Smiling faces”](#smiling-faces)

The actual amount of code and dependencies removed and the number of smiling
faces this brings is what matters most, but also remain a good mystery. Clearly
more and more projects add Knip to their projects and CI workflows to keep
ever-growing codebases tidy. It’s wonderful to see if Knip plays its part in
today’s ecosystem to help with that. Thanks for bearing with me, here’s to a lot
more little red blocks in your PRs!    🟩 🟥 🟥 🟥 🟥

## Updates

[
Section titled “Updates”](#updates)

Why not throw in some freshly cooked updates in [v5.31.0](https://github.com/webpro-nl/knip/releases/tag/5.31.0) for you while we’re
at it:

- [The auto-fix feature](../features/auto-fix) has been completely revamped, it’s much better and a
  lot more comprehensive! You have to see it to believe it.

- Knip has upgraded to [Jiti v2](https://github.com/unjs/jiti), resolving a bunch of known issues when
  loading configuration files authored in TypeScript and ESM, such as:

```

Cannot use 'import.meta' outside a module

await is only valid in async functions and the top level bodies of modules

Unexpected identifier 'Promise'

Reflect.metadata is not a function

```

And that pesky “CJS build of Vite’s Node API is deprecated” warning is finally
gone!

Thanks to everyone involved in making this happen, it’s truly much appreciated.

## Stable

[
Section titled “Stable”](#stable)

If you haven’t tried Knip recently, it’s worth taking another look! Version 5
was released 8 months ago, and even though there were no breaking changes, it
includes many enhancements. In fact, Knip has been largely stable since version
3, which came out a year ago. Many releases have a compound effect, as Knip has
kept the pace for two years now.

## Projects using Knip

[
Section titled “Projects using Knip”](#projects-using-knip)

This list of projects using Knip to keep their codebases tidy is something I
couldn’t be more proud of:

[

](https://github.com/adobe) [

](https://github.com/ag-grid) [

](https://github.com/anthropics) [

](https://github.com/arktypeio) [

](https://github.com/withastro) [

](https://github.com/aws-samples) [

03 Logo_Teal

](https://github.com/backstage) [

](https://github.com/cloudflare) [

](https://github.com/datadog) [

](https://github.com/eslint) [

](https://github.com/freeCodeCamp) [

](https://github.com/grafana) [

](https://github.com/guardian) [](https://github.com/JoshuaKGoldberg/create-typescript-app) [

](https://github.com/microsoft) [

](https://github.com/mochajs) [

](https://github.com/nuxt) [

](https://github.com/prettier) [

](https://github.com/getsentry) [
Shopify

](https://github.com/shopify) [

](https://github.com/sourcegraph) [

](https://github.com/statelyai) [

](https://github.com/storybookjs) [

](https://github.com/sveltejs) [](https://github.com/tanstack) [

](https://github.com/typescript-eslint) [

](https://github.com/vercel)

And so many more on and off the radar. Very, very cool!

## Sponsors

[
Section titled “Sponsors”](#sponsors)

Last but not least, eternal gratitude for all the sponsors that have been
supporting me along the way. THANK YOU, THANK YOU, THANK YOU!

And eh.. gotta take my chances: how about [joining this awesome club](/sponsors)?

[

     ](https://datadoghq.com) [

](https://workleap.com) [

](https://sourcegraph.com/community) [

](https://birchill.co.jp)

[ ](https://forge42.dev)

[ ](http://ustark.de)

[ ](https://github.com/samdenty)

[ WebdriverIO  
 ](https://webdriver.io)

[ ](https://understandlegacycode.com)

[ ](https://github.com/mintuhouse)

[ ](http://www.midnight-design.at)

[ ](https://kodfabrik.se)

## Acknowledgements

[
Section titled “Acknowledgements”](#acknowledgements)

Thanks to Joshua Goldberg for [emoji-blast](https://www.emojiblast.dev)! 🎉

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/blog/two-years.mdx)

[
Previous
The State of Knip ](/blog/state-of-knip) [
Next
Announcing Knip v5 ](/blog/knip-v5)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Comparison & Migration

**Source:** https://knip.dev/explanations/comparison-and-migration

---

# Comparison & Migration

First of all, Knip owes a lot to the projects on this page and they’ve all been
inspirational in their own way. For best results, Knip has [a vision embracing
comprehensiveness](./why-use-knip#comprehensive) which is larger in scope than any of the alternatives. So
if any of those tools has the right scope for your requirements, then by all
means, use what suits you best. Note that most projects are no longer
maintained.

All tools have in common that they have less features and don’t support the
concept of [monorepos/workspaces](../features/monorepos-and-workspaces). Feel free to send in projects that Knip
does not handle better, Knip loves to be challenged!

## Migration

[
Section titled “Migration”](#migration)

A migration consists of deleting the dependency and its configuration file and
[getting started with Knip](../overview/getting-started). You should end up with less configuration.

## Comparison

[
Section titled “Comparison”](#comparison)

### depcheck

[
Section titled “depcheck”](#depcheck)

[Depcheck](https://github.com/depcheck/depcheck) is a tool for analyzing the dependencies in a project to see:
how each dependency is used, which dependencies are useless, and which
dependencies are missing from package.json.

The project has plugins (specials), yet not as many as Knip has and they’re not
as advanced. It also supports compilers (parsers) for non-standard files.

The following commands are similar:

- Terminal window

```

depcheck

knip --dependencies

```

### unimported

[
Section titled “unimported”](#unimported)

Find and fix dangling files and unused dependencies in your JavaScript
projects.

[unimported](https://github.com/smeijer/unimported) is fast and works well. It works in what Knip calls “production
mode” exclusively. If you’re fine with a little bit of configuration and don’t
want or need to deal with non-production items (such as `devDependencies` and
test files), then this might work well for you.

The following commands are similar:

Terminal window

```

unimported

knip --production --dependencies --files

```

**Project status**: The project is archived and recommends Knip.

### ts-prune

[
Section titled “ts-prune”](#ts-prune)

Find unused exports in a typescript project. 🛀

[ts-prune](https://github.com/nadeesha/ts-prune) aims to find potentially unused exports in your TypeScript project
with zero configuration.

The following commands are similar:

Terminal window

```

ts-prune

knip --include exports,types,nsExports,nsTypes

```

Use `knip --exports` to also include class and enum members.

**Project status**: The project is archived and recommends Knip.

### ts-unused-exports

[
Section titled “ts-unused-exports”](#ts-unused-exports)

[ts-unused-exports](https://github.com/pzavolinsky/ts-unused-exports) finds unused exported symbols in your Typescript
project

The following commands are similar:

Terminal window

```

ts-unused-exports

knip --include exports,types,nsExports,nsTypes

```

Use `knip --exports` to also include class and enum members.

### tsr

[
Section titled “tsr”](#tsr)

Remove unused code from your TypeScript Project

[tsr](https://github.com/line/tsr) (previously `ts-remove-unused`) removes unused exports, and works based
on a single `tsconfig.json` file (`includes` and `excludes`) and requires no
configuration. It removes the `export` keyword or the whole export declaration.

## Related projects

[
Section titled “Related projects”](#related-projects)

Additional alternative and related projects include:

[deadfile](https://github.com/M-Izadmehr/deadfile)

- [DepClean](https://github.com/mysteryven/depclean)

- [dependency-check](https://github.com/dependency-check-team/dependency-check)

- [find-unused-exports](https://github.com/jaydenseric/find-unused-exports)

- [next-unused](https://github.com/pacocoursey/next-unused)

- [npm-check](https://github.com/dylang/npm-check)

- [renoma](https://github.com/bluwy/renoma)

In general, the [e18e.dev](https://e18e.dev) website and in particular the [Cleanup](https://e18e.dev/guide/cleanup.html)
section is a great resource when dealing with technical debt.

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/explanations/comparison-and-migration.md)

[
Previous
Why use Knip? ](/explanations/why-use-knip) [
Next
Production Mode ](/features/production-mode)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Entry Files

**Source:** https://knip.dev/explanations/entry-files

---

# Entry Files

Entry files are the starting point for Knip to determine what files are used in
the codebase. More entry files lead to increased coverage of the codebase. This
also leads to more dependencies to be discovered. This page explains how Knip
and its plugins try to find entry files so you don’t need to configure them
yourself.

## Default entry file patterns

[
Section titled “Default entry file patterns”](#default-entry-file-patterns)

For brevity, the [default configuration](../overview/configuration#defaults) on the previous page mentions only
`index.js` and `index.ts`, but the default set of file names and extensions is
actually a bit larger:

- `index`, `main` and `cli`

- `js`, `mjs`, `cjs`, `jsx`, `ts`, `mts`, `cts` and `tsx`

This means files like `main.cjs` and `src/cli.ts` are automatically added as
entry files. Here’s the default configuration in full:

```

{

  "entry": [

    "{index,cli,main}.{js,cjs,mjs,jsx,ts,cts,mts,tsx}",

    "src/{index,cli,main}.{js,cjs,mjs,jsx,ts,cts,mts,tsx}"

  ],

  "project": ["**/*.{js,cjs,mjs,jsx,ts,cts,mts,tsx}!"]

}

```

Next to the default locations, Knip looks for `entry` files in other places. In
a monorepo, this is done for each workspace separately.

The values you set override the default values, they are not merged.

Also see [FAQ: Where does Knip look for entry files?](../reference/faq#where-does-knip-look-for-entry-files)

## Plugins

[
Section titled “Plugins”](#plugins)

Plugins often add entry files. For instance, if the Remix, Storybook and Vitest
plugins are enabled in your project, they’ll add additional entry files. See
[the next page about plugins](./plugins) for more details about this.

## Scripts in package.json

[
Section titled “Scripts in package.json”](#scripts-in-packagejson)

The `package.json` is scanned for entry files. The `main`, `bin`, and `exports`
fields may contain entry files. The `scripts` are also parsed to find entry
files and dependencies. See [Script Parser](../features/script-parser) for more details.

## Ignored files

[
Section titled “Ignored files”](#ignored-files)

Knip respects `.gitignore` files. By default, ignored files are not added as
entry files. This behavior can be disabled by using the [`--no-gitignore`](../reference/cli#--no-gitignore)
flag on the CLI.

## Configuring project files

[
Section titled “Configuring project files”](#configuring-project-files)

See [configuring project files](../guides/configuring-project-files) for guidance on tuning `entry` and `project`
and when to use `ignore`.

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/explanations/entry-files.md)

[
Previous
Screenshots & videos ](/overview/screenshots-videos) [
Next
Plugins ](/explanations/plugins)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Plugins

**Source:** https://knip.dev/explanations/plugins

---

# Plugins

This page describes why Knip uses plugins and the difference between `config`
and `entry` files.

Knip has an extensive and growing [list of built-in plugins](../reference/plugins). Feel free to
[write a plugin](../guides/writing-a-plugin) so others can benefit too!

## What does a plugin do?

[
Section titled “What does a plugin do?”](#what-does-a-plugin-do)

Plugins are enabled if the related package is listed in the list of dependencies
in `package.json`. For instance, if `astro` is listed in `dependencies` or
`devDependencies`, then the Astro plugin is enabled. And this means that this
plugin will:

- Handle [configuration files](#configuration-files) like `astro.config.mjs`

- Add [entry files](#entry-files) such as `src/pages/**/*.astro`

- Define [command-line arguments](#command-line-arguments)

## Configuration files

[
Section titled “Configuration files”](#configuration-files)

Knip uses [entry files](./entry-files) as starting points to scan your source code and
resolve other internal files and external dependencies. The module graph can be
statically resolved through the `require` and `import` statements in those
source files. However, configuration files reference external dependencies in
various ways. Knip uses a plugin for each tool to parse configuration files and
find those dependencies.

### Example: ESLint

[
Section titled “Example: ESLint”](#example-eslint)

In the first example we look at [the ESLint plugin](../reference/plugins/eslint). The default `config`
file patterns include `.eslintrc.json`. Here’s a minimal example:

- .eslintrc.json

```

{

  "extends": ["airbnb", "prettier"],

  "plugins": ["@typescript-eslint"]

}

```

Configuration files like this don’t `import` or `require` anything, but they do
require the referenced dependencies to be installed.

In this case, the plugin will return three dependencies:

`eslint-config-airbnb`

- `eslint-config-prettier`

- `@typescript-eslint/eslint-plugin`

Knip will then look for missing dependencies in `package.json` and report those
as unlisted. And vice versa, if there are any ESLint plugins listed in
`package.json`, but unused, those will be reported as well.

### Example: Vitest

[
Section titled “Example: Vitest”](#example-vitest)

The second example uses [the Vitest plugin](../reference/plugins/eslint). Here’s a minimal example of a
Vitest configuration file:

vitest.config.ts

```

import { defineConfig } from 'vitest/config';

export default defineConfig({

  test: {

    coverage: {

      provider: 'istanbul',

    },

    environment: 'happy-dom',

  },

});

```

The Vitest plugin reads this configuration and return two dependencies:

- `@vitest/coverage-istanbul`

- `vitest-environment-happy-dom`

Knip will look for missing and unused dependencies in `package.json` and report
accordingly.

Some tools allow configuration to be stored in `package.json`, that’s why some
plugins contain `package.json` in the list of `config` files.

Summary

Plugins parse `config` files to find external dependencies. Knip uses this to
determine unused and unlisted dependencies.

## Entry files

[
Section titled “Entry files”](#entry-files)

Many plugins have default `entry` files configured. When the plugin is enabled,
Knip will add entry files as configured by the plugin to resolve used files and
dependencies.

For example, if `next` is listed as a dependency in `package.json`, the Next.js
plugin will automatically add multiple patterns as entry files, such as
`pages/**/*.{js,jsx,ts,tsx}`. If `vitest` is listed, the Vitest plugin adds
`**/*.{test,test-d,spec}.ts` as entry file patterns. Most plugins have entry
files configured, so you don’t have to.

It’s mostly plugins for meta frameworks and test runners that have `entry` files
configured.

Plugins result in less configuration

Plugins uses entry file patterns as defined in the configuration files of these
tools. So you don’t need to repeat this in your Knip configuration.

For example, let’s say your Playwright configuration contains the following:

playwright.config.ts

```

import type { PlaywrightTestConfig } from '@playwright/test';

const config: PlaywrightTestConfig = {

  testDir: 'integration',

  testMatch: ['**/*-test.ts'],

};

export default config;

```

The Playwright plugin will read this configuration file and return those entry
patterns (`integration/**/*-test.ts`). Knip will then not use the default entry
patterns.

You can still override this behavior in your Knip configuration:

knip.json

```

{

  "playwright": {

    "entry": "src/**/*.integration.ts"

  }

}

```

This should not be necessary though. Please consider opening a pull request or a
bug report if any plugin is not behaving as expected.

Summary

Plugins try hard to automatically add the correct entry files.

## Entry files from config files

[
Section titled “Entry files from config files”](#entry-files-from-config-files)

Entry files are part of plugin configuration (as described in the previous
section). Yet plugins can also return additional entry files after parsing
configuration files. Below are some examples of configuration files parsed by
plugins to return additional entry files. The goal of these examples is to give
you an idea about the various ways Knip and its plugins try to find entry files
so you don’t need to configure them yourself.

### Angular

[
Section titled “Angular”](#angular)

The Angular plugin parses the Angular configuration file. Here’s a fragment:

angular.json

```

{

  "$schema": "./node_modules/@angular/cli/lib/config/schema.json",

  "projects": {

    "knip-angular-example": {

      "architect": {

        "build": {

          "builder": "@angular-devkit/build-angular:browser",

          "options": {

            "outputPath": "dist/knip-angular-example",

            "main": "src/main.ts",

            "tsConfig": "tsconfig.app.json"

          }

        }

      }

    }

  }

}

```

This will result in `src/main.ts` being added as an entry file (and
`@angular-devkit/build-angular` as a referenced dependency).

Additionally, the Angular plugin returns `tsconfig.app.json` as a configuration
file for the TypeScript plugin.

### GitHub Actions

[
Section titled “GitHub Actions”](#github-actions)

This plugin parses workflow YAML files. This fragment contains three `run`
scripts:

.github/workflows/deploy.yml

```

jobs:

  integration:

    runs-on: ubuntu-latest

    steps:

      - run: npm install

      - run: node scripts/build.js

      - run: node --loader tsx scripts/deploy.ts

      - run: playwright test -c playwright.web.config.ts

        working-dir: e2e

```

From these scripts, the `scripts/build.js` and `scripts/deploy.ts` files will be
added as entry files by the GitHub Actions plugin.

Additionally, the file `e2e/playwright.web.config.ts` is detected and will be
handed over as a Playwright configuration file.

Read more about this in [command-line arguments](#command-line-arguments).

### webpack

[
Section titled “webpack”](#webpack)

Let’s take a look at this example webpack configuration file:

webpack.config.js

```

module.exports = env => {

  return {

    entry: {

      main: './src/app.ts',

      vendor: './src/vendor.ts',

    },

    module: {

      rules: [

        {

          test: /\.(woff|ttf|ico|woff2|jpg|jpeg|png|webp)$/i,

          use: 'base64-inline-loader',

        },

      ],

    },

  };

};

```

The webpack plugin will parse this and add `./src/app.ts` and `./src/vendor.ts`
as entry files. It will also add `base64-inline-loader` as a referenced
dependency.

Summary

Plugins can find additional entry files when parsing config files.

## Bringing it all together

[
Section titled “Bringing it all together”](#bringing-it-all-together)

Sometimes a configuration file is a JavaScript or TypeScript file that imports
dependencies, but also contains configuration that needs to be parsed by a
plugin to find additional dependencies.

Let’s take a look at this example Vite configuration file:

vite.config.ts

```

import { defineConfig } from 'vite';

import react from '@vitejs/plugin-react';

export default defineConfig(async ({ mode, command }) => {

  return {

    plugins: [react()],

    test: {

      setupFiles: ['./setup-tests.ts'],

      environment: 'happy-dom',

      coverage: {

        provider: 'c8',

      },

    },

  };

});

```

This file imports `vite` and `@vitejs/plugin-react` directly, but also
indirectly references the `happy-dom` and `@vitest/coverage-c8` packages.

The Vite plugin of Knip will **dynamically** load this configuration file and
parse the exported configuration. But it’s not aware of the `vite` and
`@vitejs/plugin-react` imports. This is why such `config` files are also
automatically added as `entry` files for Knip to **statically** resolve the
`import` and `require` statements.

Additionally, `./setup-tests.ts` will be added as an `entry` file.

## Command-Line Arguments

[
Section titled “Command-Line Arguments”](#command-line-arguments)

Plugins may define the arguments where Knip should look for entry files,
configuration files and dependencies. We’ve already seen some examples above:

Terminal window

```

node --loader tsx scripts/deploy.ts

playwright test -c playwright.web.config.ts

```

Please see [script parser](../features/script-parser) for more details.

## Summary

[
Section titled “Summary”](#summary)

Summary

Plugins are configured with two distinct types of files:

- `config` files are dynamically loaded and parsed by the plugin

- `entry` files are added to the module graph

- Both can recursively lead to additional entry files, config files and
  dependencies

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/explanations/plugins.md)

[
Previous
Entry Files ](/explanations/entry-files) [
Next
Why use Knip? ](/explanations/why-use-knip)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Why use Knip?

**Source:** https://knip.dev/explanations/why-use-knip

---

# Why use Knip?

The value of removing clutter from your code is undeniable. However, finding it
is a tedious job. This is where Knip comes in. As codebases grow in complexity
and size, comprehensive and automated tooling is indispensable.

TL;DR

Knip finds and fixes unused dependencies, exports and files.

Deep analysis from [fine-grained entry points](./entry-files) based on the actual frameworks
and tooling in [(mono)repos](../features/monorepos-and-workspaces) for accurate and actionable results. Advanced
features for maximum coverage:

- [Custom module resolution](../reference/faq#why-doesnt-knip-use-an-existing-module-resolver)

- [Configuration file parsers](./plugins#configuration-files)

- [Advanced shell script parser](../features/script-parser)

- [Built-in and custom compilers](../features/compilers)

- [Auto-fix most issues](../features/auto-fix)

## Less is more

[
Section titled “Less is more”](#less-is-more)

There are plenty of reasons to delete unused files, dependencies and “dead
code”:

- Easier maintenance: things are easier to manage when there’s less of it.

- Improved performance: startup time, build time and/or bundle size can be
  negatively impacted when unused code, files and/or dependencies are included.
  Relying on tree-shaking when bundling code helps, but it’s not a silver
  bullet.

- Easier onboarding: there should be no doubts about whether files, dependencies
  and exports are actually in use or not. Especially for people new to the
  project and/or taking over responsibilities this is harder to grasp.

- Prevent regressions: tools like TypeScript, ESLint and Prettier do all sorts
  of checks and linting to report violations and prevent regressions. Knip does
  the same for dependencies, exports and files that are obsolete.

- Keeping dead code around has a negative value on readability, as it can be
  misleading and distracting. Even if it serves no purpose it will need to be
  maintained (source: [Safe dead code removal → YAGNI](https://jfmengels.net/safe-dead-code-removal/#yagni-you-arent-gonna-need-it)).

- Also see [Why are unused dependencies a problem?](../typescript/unused-dependencies#why-are-unused-dependencies-a-problem) and [Why are unused
  exports a problem?](../typescript/unused-exports#why-are-unused-exports-a-problem).

## Automation

[
Section titled “Automation”](#automation)

Code and dependency management is usually not the most exciting task for most of
us. Knip’s mission is to automate finding clutter. This is such a tedious job if
you were to do it manually, and where would you even start? Knip applies many
techniques and heuristics to report what you need and save a lot of time.

Tip

Knip not only finds clutter, it can also [remove clutter](../features/auto-fix)!

Use Knip next to a linter like ESLint or Biome: after removing unused variables
inside files, Knip might find even more unused code. Rinse and repeat!

## Comprehensive

[
Section titled “Comprehensive”](#comprehensive)

You can use alternative tools that do the same. However, the advantage of a
strategy that addresses all of dependencies, exports and files is in their
synergy:

- Utilizing plugins to find their dependencies includes the capacity to find
  additional entry and configuration files. This results in more resolved and
  used files. Better coverage gives better insights into unused files and
  exports.

- Analyzing more files reveals more unused exports and dependency usage,
  refining the list of both unused and unlisted dependencies.

- This approach is amplified in a monorepo setting. In fact, files and internal
  dependencies can recursively reference each other (across workspaces).

## Greenfield or Legacy

[
Section titled “Greenfield or Legacy”](#greenfield-or-legacy)

Installing Knip in greenfield projects ensures the project stays neat and tidy
from the start. Add it to your CI workflow and prevent any regressions from
entering the codebase.

Tip

Use Knip in a CI environment to prevent future regressions.

In large and/or legacy projects, Knip may report false positives and require
some configuration. Yet it can be a great assistant when cleaning up parts of
the project or doing large refactors. Even a list of results with a few false
positives is many times better and faster than if you were to do it manually.

## Unobtrusive

[
Section titled “Unobtrusive”](#unobtrusive)

Knip does not introduce new syntax for you to learn. This may sound obvious, but
consider comments like the following:

-

```

// eslint-disable-next-line

// prettier-ignore

// @ts-expect-error

```

Maybe you wonder why Knip does not have similar comments like `// knip-ignore`
so you can get rid of false positives? A variety of reasons:

A false positive may be a bug in Knip, and should be reported, not dismissed.

- Instead of proprietary comments, use [standardized annotations](../reference/jsdoc-tsdoc-tags) that also
  serve as documentation.

- In the event you want to remove Knip, just uninstall `knip` without having to
  remove useless comments scattered throughout the codebase.

Tip: use `@lintignore` in JSDoc comments, so other linters can use the same.

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/explanations/why-use-knip.md)

[
Previous
Plugins ](/explanations/plugins) [
Next
Comparison & Migration ](/explanations/comparison-and-migration)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Auto-fix

**Source:** https://knip.dev/features/auto-fix

---

# Auto-fix

Run Knip as you normally would, and if the report looks good then run it again
with the `--fix` flag to let Knip automatically apply fixes. It fixes the
following [issue types](../reference/issue-types):

- Remove `export` keyword for unused exports and exported types

- Remove `export default` keywords for unused default exports

- Remove exports, re-exports and exported types

- Remove unused enum members

- Remove unused class members (disabled by default)

- Remove unused `dependencies` and `devDependencies` from `package.json`

- Remove unused files

Caution

Use a VCS (version control system) like Git to review and undo changes as
necessary.

## Flags

[
Section titled “Flags”](#flags)

### Fix

[
Section titled “Fix”](#fix)

Add the `--fix` flag to remove unused exports and dependencies:

- Terminal window

```

knip --fix

```

Add `--allow-remove-files` to allow Knip to remove unused files:

Terminal window

```

knip --fix --allow-remove-files

```

Use `--fix-type` to fix only specific issue types:

`files`

- `exports`

- `types`

- `dependencies`

Example:

Terminal window

```

knip --fix-type exports,types

knip --fix-type exports --fix-type types   # same as above

```

### Format

[
Section titled “Format”](#format)

Add `--format` to format the modified files using the formatter and
configuration in your project. Supports Biome, deno fmt, dprint and Prettier
(using [Formatly](https://github.com/JoshuaKGoldberg/formatly)):

Terminal window

```

knip --fix --format

```

## Demo

[
Section titled “Demo”](#demo)

## Post-fix

[
Section titled “Post-fix”](#post-fix)

After Knip has fixed issues, there are four things to consider:

### 1. Use a formatter

[
Section titled “1. Use a formatter”](#1-use-a-formatter)

Use a tool like Prettier or Biome if the code needs formatting. Knip removes the
minimum amount of code while leaving it in a working state.

Tip

Add the `--format` flag to format the modified files using the formatter and
configuration in your project.

### 2. Unused variables

[
Section titled “2. Unused variables”](#2-unused-variables)

Use a tool like ESLint or Biome to find and remove unused variables inside
files. Even better, try [remove-unused-vars](https://github.com/webpro-nl/remove-unused-vars) to remove unused variables
within files.

This may result in more deleted code, and Knip may then find more unused code.
Rinse and repeat!

### 3. Unused dependencies

[
Section titled “3. Unused dependencies”](#3-unused-dependencies)

Verify changes in `package.json` and update dependencies using your package
manager.

- [ npm ](#tab-panel-14)
- [ pnpm ](#tab-panel-15)
- [ bun ](#tab-panel-16)
- [ yarn ](#tab-panel-17)

Terminal window

```

npm install

```

Terminal window

```

pnpm install

```

Terminal window

```

bun install

```

Terminal window

```

yarn

```

### 4. Install unlisted dependencies

[
Section titled “4. Install unlisted dependencies”](#4-install-unlisted-dependencies)

If Knip reports unlisted dependencies or binaries, they should be installed
using the package manager in the project, for example:

- [ npm ](#tab-panel-18)
- [ pnpm ](#tab-panel-19)
- [ bun ](#tab-panel-20)
- [ yarn ](#tab-panel-21)

Terminal window

```

npm install unlisted-package

```

Terminal window

```

pnpm add unlisted-package

```

Terminal window

```

bun add unlisted-package

```

Terminal window

```

yarn add unlisted-package

```

## Example results

[
Section titled “Example results”](#example-results)

### Exports

[
Section titled “Exports”](#exports)

The `export` keyword for unused exports is removed:

module.ts

```

export const unused = 1;

export default class MyClass {}

const unused = 1;

class MyClass {}

```

The `default` keyword was also removed here.

Knip removes the whole or part of export declarations:

module.ts

```

type Snake = 'python' | 'anaconda';

const Owl = 'Hedwig';

const Hawk = 'Tony';

export type { Snake };

export { Owl, Hawk };

;

;

```

### Re-exports

[
Section titled “Re-exports”](#re-exports)

Knip removes the whole or part of re-exports:

file.js

```

export { Cat, Dog } from './pets';

export { Lion, Elephant } from './jungle';

export { Elephant } from './jungle'

```

Also across any chain of re-exports:

- [ module.ts ](#tab-panel-22)
- [ barrel.ts ](#tab-panel-23)
- [ index.ts ](#tab-panel-24)

```

export const Hawk = 'Tony';

export const Owl = 'Hedwig';

const Owl = 'Hedwig';

```

```

export * from './module.js';

```

```

export { Hawk, Owl } from './barrel.js';

export { Hawk } from './barrel.js'

```

### Export assignments

[
Section titled “Export assignments”](#export-assignments)

Knip removes individual exported items in “export assignments”, but does not
remove the entire export declaration if it’s empty:

file.js

```

export const { a, b  } = fn();

export const {   } = fn();

export const [c, d] = [c, d];

export const [, ] = [c, d];

```

Reason: the right-hand side of the assignment might have side-effects. It’s not
safe to always remove the whole declaration. This could be improved in the
future (feel free to open an issue/RFC).

### Enum members

[
Section titled “Enum members”](#enum-members)

Unused members of enums are removed:

file.ts

```

export enum Directions {

  North = 1,

  East = 2,

  South = 3,

  West = 4,

}

```

### CommonJS

[
Section titled “CommonJS”](#commonjs)

Knip supports CommonJS and removes unused exports:

common.js

```

module.exports = { identifier, unused };

module.exports = { identifier,  };

module.exports.UNUSED = 1;

module.exports['ACCESS'] = 1;

```

Warning: the right-hand side of such an assignment might have side-effects. Knip
currently removes the whole declaration (feel free to open an issue/RFC).

### Dependencies

[
Section titled “Dependencies”](#dependencies)

Unused dependencies are removed from `package.json`:

package.json

```

{

  "name": "my-package",

  "dependencies": {

    "rimraf": "*",

    "unused-dependency": "*"

    "rimraf": "*"

  },

  "devDependencies": {

    "unreferenced-package": "5.3.3"

  }

  "devDependencies": {}

}

```

### Class members   experimental

[
Section titled “Class members   ”](#class-members-)

Unused members of classes can be removed:

file.ts

```

export class Rectangle {

  constructor(public width: number, public height: number) {}

  static Key = 1;

  area() {

    return this.width * this.height;

  }

  public get unusedGetter(): string {

    return 'unusedGetter';

  }

}

```

Currently Knip might be too eager removing class members when they’re not
referenced internally but meant to be called by an external library. For
instance, Knip might think `componentDidMount` and `render` in React class
component are unused and will remove those.

Note that [`classMembers` aren’t included by default](../guides/handling-issues#class-members).

## What’s not included

[
Section titled “What’s not included”](#whats-not-included)

Operations that auto-fix does not (yet) perform and why:

- Add unlisted (dev) dependencies to `package.json` (should it go into
  `dependencies` or `devDependencies`? For monorepos in current workspace or
  root?)

- Add unlisted binaries (which package and package version contains the used
  binary?)

- Fix duplicate exports (which one should removed?)

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/features/auto-fix.mdx)

[
Previous
Rules & Filters ](/features/rules-and-filters) [
Next
Compilers ](/features/compilers)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Compilers

**Source:** https://knip.dev/features/compilers

---

# Compilers

Projects may have source files that are not JavaScript or TypeScript, and thus
require compilation (or transpilation, or pre-processing, you name it). Files
like `.mdx`, `.astro`, `.vue` and `.svelte` may also import other source files
and external dependencies. So ideally, these files are included when linting the
project. That’s why Knip supports compilers.

## Built-in compilers

[
Section titled “Built-in compilers”](#built-in-compilers)

Knip has built-in “compilers” for the following file extensions:

- `.astro`

- `.mdx`

- `.svelte`

- `.vue`

Knip does not include real compilers for those files, but regular expressions to
collect `import` statements. This is fast, requires no dependencies, and enough
for Knip to build the module graph.

On the other hand, real compilers may expose their own challenges in the context
of Knip. For instance, the Svelte compiler keeps `exports` intact, while they
might represent component properties. This results in those exports being
reported as unused by Knip.

The built-in functions seem to do a decent job, but override them however you
like.

Compilers are enabled only if certain dependencies are found. If that’s not
working for your project, set `true` and enable any compiler manually:

- knip.ts

```

export default {

  compilers: {

    mdx: true,

  },

};

```

## Custom compilers

[
Section titled “Custom compilers”](#custom-compilers)

Built-in compilers can be overridden, and additional compilers can be added.
Since compilers are functions, the Knip configuration file must be a dynamic
`.js` or `.ts` file.

### Interface

[
Section titled “Interface”](#interface)

The compiler function interface is straightforward. Text in, text out:

```

(source: string, filename: string) => string;

```

This may also be an `async` function.

Note

Compilers will automatically have their extension added as a default extension
to Knip. This means you don’t need to add something like `**/*.{ts,vue}` to the
`entry` or `project` file patterns manually.

### Examples

[
Section titled “Examples”](#examples)

[CSS](#css)

- [MDX](#mdx)

- [Svelte](#svelte)

- [Vue](#vue)

#### CSS

[
Section titled “CSS”](#css)

Here’s an example, minimal compiler for CSS files:

knip.ts

```

export default {

  compilers: {

    css: (text: string) => [...text.matchAll(/(?<=@)import[^;]+/g)].join('\n'),

  },

};

```

You may wonder why the CSS compiler is not included by default. It’s currently
not clear if it should be included. And if so, what would be the best way to
determine it should be enabled, and what syntax(es) it should support.

#### MDX

[
Section titled “MDX”](#mdx)

Another example, in case the built-in MDX compiler is not enough:

```

import { compile } from '@mdx-js/mdx';

export default {

  compilers: {

    mdx: async text => (await compile(text)).toString(),

  },

};

```

#### Svelte

[
Section titled “Svelte”](#svelte)

In a Svelte project, the compiler is automatically enabled. Override and use
Svelte’s compiler for better results if the built-in “compiler” is not enough:

```

import type { KnipConfig } from 'knip';

import { compile } from 'svelte/compiler';

export default {

  compilers: {

    svelte: (source: string) => compile(source, {}).js.code,

  },

} satisfies KnipConfig;

```

#### Vue

[
Section titled “Vue”](#vue)

In a Vue project, the compiler is automatically enabled. Override and use Vue’s
parser for better results if the built-in “compiler” is not enough:

```

import type { KnipConfig } from 'knip';

import {

  parse,

  type SFCScriptBlock,

  type SFCStyleBlock,

} from 'vue/compiler-sfc';

function getScriptBlockContent(block: SFCScriptBlock | null): string[] {

  if (!block) return [];

  if (block.src) return [`import '${block.src}'`];

  return [block.content];

}

function getStyleBlockContent(block: SFCStyleBlock | null): string[] {

  if (!block) return [];

  if (block.src) return [`@import '${block.src}';`];

  return [block.content];

}

function getStyleImports(content: string): string {

  return [...content.matchAll(/(?<=@)import[^;]+/g)].join('\n');

}

const config = {

  compilers: {

    vue: (text: string, filename: string) => {

      const { descriptor } = parse(text, { filename, sourceMap: false });

      return [

        ...getScriptBlockContent(descriptor.script),

        ...getScriptBlockContent(descriptor.scriptSetup),

        ...descriptor.styles.flatMap(getStyleBlockContent).map(getStyleImports),

      ].join('\n');

    },

  },

} satisfies KnipConfig;

export default config;

```

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/features/compilers.md)

[
Previous
Auto-fix ](/features/auto-fix) [
Next
Reporters & Preprocessors ](/features/reporters)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Integrated Monorepos

**Source:** https://knip.dev/features/integrated-monorepos

---

# Integrated Monorepos

Some repositories have a single `package.json`, but consist of multiple projects
with configuration files across the repository. A good example is the [Nx
integrated monorepo style](https://nx.dev/getting-started/tutorials/integrated-repo-tutorial).

Tip

An integrated monorepo is a single workspace.

## Entry Files

[
Section titled “Entry Files”](#entry-files)

The default entrypoints files might not be enough. Here’s an idea that might fit
this type of monorepo:

knip.json

```

{

  "entry": ["{apps,libs}/**/src/index.{ts,tsx}"],

  "project": ["{apps,libs}/**/src/**/*.{ts,tsx}"]

}

```

## Plugins

[
Section titled “Plugins”](#plugins)

Let’s assume some of these projects are applications (“apps”) which have their
own ESLint configuration files and Cypress configuration and test files. This
may result in those files getting reported as unused, and consequently also the
dependencies they import and refer to.

In that case, we could configure the ESLint and Cypress plugins like this:

knip.json

```

{

  "eslint": {

    "config": ["{apps,libs}/**/.eslintrc.json"]

  },

  "cypress": {

    "entry": ["apps/**/cypress.config.ts", "apps/**/cypress/e2e/*.spec.ts"]

  }

}

```

Adapt the file patterns to your project, and the relevant `config` and `entry`
files and dependencies should no longer be reported as unused.

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/features/integrated-monorepos.md)

[
Previous
Monorepos & Workspaces ](/features/monorepos-and-workspaces) [
Next
Source Mapping ](/features/source-mapping)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Monorepos & Workspaces

**Source:** https://knip.dev/features/monorepos-and-workspaces

---

# Monorepos & Workspaces

Workspaces are handled out-of-the-box by Knip.

Workspaces are sometimes also referred to as package-based monorepos, or as
packages in a monorepo. Knip uses the term workspace exclusively to indicate a
directory that has a `package.json`.

## Configuration

[
Section titled “Configuration”](#configuration)

Here’s example configuration with custom `entry` and `project` patterns:

- knip.json

```

{

  "workspaces": {

    ".": {

      "entry": "scripts/*.js",

      "project": "scripts/**/*.js"

    },

    "packages/*": {

      "entry": "{index,cli}.ts",

      "project": "**/*.ts"

    },

    "packages/cli": {

      "entry": "bin/cli.js"

    }

  }

}

```

Tip

Run Knip without any configuration to see if and where custom `entry` and/or
`project` files are necessary per workspace.

Each workspace has the same [default configuration](../overview/configuration#defaults).

The root workspace is named `"."` under `workspaces` (like in the example
above).

Caution

In a project with workspaces, the `entry` and `project` options at the root
level are ignored. Use the workspace named `"."` for those (like in the example
above).

## Workspaces

[
Section titled “Workspaces”](#workspaces)

Knip reads workspaces from four possible locations:

The `workspaces` array in `package.json` (npm, Bun, Yarn, Lerna)

- The `packages` array in `pnpm-workspace.yaml` (pnpm)

- The `workspaces.packages` array in `package.json` (legacy)

- The `workspaces` object in Knip configuration

The `workspaces` in Knip configuration (4) not already defined in the root
`package.json` or `pnpm-workspace.yaml` (1, 2, 3) are added to the analysis.

Caution

A workspace must have a `package.json` file.

For projects with only a root `package.json`, please see [integrated
monorepos](./integrated-monorepos).

## Additional workspaces

[
Section titled “Additional workspaces”](#additional-workspaces)

If a workspaces is not configured as such in `package.json#workspaces` (or
`pnpm-workspace.yaml`) it can be added to the Knip configuration manually. Add
their path to the `workspaces` configuration object the same way as
`"packages/cli": {}` in the example above.

## Source mapping

[
Section titled “Source mapping”](#source-mapping)

See [Source Mapping](./source-mapping).

## Additional options

[
Section titled “Additional options”](#additional-options)

The following options are available inside workspace configurations:

- [ignore](../reference/configuration#ignore)

- [ignoreBinaries](../reference/configuration#ignorebinaries)

- [ignoreDependencies](../reference/configuration#ignoredependencies)

- [ignoreMembers](../reference/configuration#ignoremembers)

- [ignoreUnresolved](../reference/configuration#ignoreunresolved)

- [includeEntryExports](../reference/configuration#includeentryexports)

[Plugins](../reference/configuration#plugins) can be configured separately per workspace.

Use `--debug` for verbose output and see the workspaces Knip includes, their
configurations, enabled plugins, glob options and resolved files.

## Lint a single workspace

[
Section titled “Lint a single workspace”](#lint-a-single-workspace)

Use the `--workspace` (or `-W`) argument to focus on a single workspace (and let
Knip run faster). Example:

Terminal window

```

knip --workspace packages/my-lib

```

This will include the target workspace, but also ancestor and dependent
workspaces. For two reasons:

- Ancestor workspaces may list dependencies in `package.json` the linted
  workspace uses.

- Dependent workspaces may reference exports from the linted workspace.

To lint the workspace in isolation, there are two options:

- Combine the `workspace` argument with [strict production mode](./production-mode#strict-mode).

- Run Knip from inside the workspace directory.

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/features/monorepos-and-workspaces.md)

[
Previous
Production Mode ](/features/production-mode) [
Next
Integrated Monorepos ](/features/integrated-monorepos)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Production Mode

**Source:** https://knip.dev/features/production-mode

---

# Production Mode

The default mode for Knip is comprehensive and targets all project code,
including configuration files, test files, Storybook stories, and so on. Test
files usually import production files. This prevents production files or their
exports from being reported as unused, while sometimes both of them can be
deleted. Knip features a “production mode” to focus only on the code that you
ship.

## Configuration

[
Section titled “Configuration”](#configuration)

To tell Knip what is production code, add an exclamation mark behind each
`pattern!` that represents production code:

- knip.json

```

{

  "entry": ["src/index.ts!", "build/script.js"],

  "project": ["src/**/*.ts!", "build/*.js"]

}

```

Depending on file structure and enabled plugins, it’s possible that you don’t
need to modify your configuration at all.

Then run Knip with the `--production` flag:

Terminal window

```

knip --production

```

Here’s what’s included in production mode:

Only `entry` and `project` patterns suffixed with `!`

- Only production `entry` file patterns exported by plugins (such as Next.js and
  Remix)

- Only the `start` and `postinstall` scripts

- Ignore exports with the [`@internal` tag](../reference/jsdoc-tsdoc-tags#internal)

Note

The production run does not replace the default run. Depending on your needs you
can run either of them or both separately. Usually both modes can share the same
configuration.

To see the difference between default and production mode in great detail, use
the `--debug` flag and inspect what entry and project files are used, and the
plugins that are enabled. For instance, in production mode this shows that files
such as tests and Storybook files (stories) are excluded from the analysis.

In case files like mocks and test helpers are reported as unused files, use
negated patterns to exclude those files in production mode:

knip.json

```

{

  "entry": ["src/index.ts!"],

  "project": ["src/**/*.ts!", "!src/test-helpers/**!"]

}

```

Also see [configuring project files](../guides/configuring-project-files) to align `entry` and `project` with
production mode.

## Strict Mode

[
Section titled “Strict Mode”](#strict-mode)

Additionally, the `--strict` flag can be added to:

- Consider `dependencies` (not `devDependencies`) when finding unused or
  unlisted dependencies

- Include `peerDependencies` when finding unused or unlisted dependencies

- Verify isolation: workspaces should use strictly their own `dependencies`

- Type-only imports should be in `devDependencies`

Terminal window

```

knip --production --strict

```

Using `--strict` implies `--production`, so the latter can be omitted.

## Types

[
Section titled “Types”](#types)

Add `--exclude types` if you don’t want to include types in the report:

Terminal window

```

knip --production --exclude types

```

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/features/production-mode.md)

[
Previous
Comparison & Migration ](/explanations/comparison-and-migration) [
Next
Monorepos & Workspaces ](/features/monorepos-and-workspaces)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Reporters & Preprocessors

**Source:** https://knip.dev/features/reporters

---

# Reporters & Preprocessors

## Built-in Reporters

[
Section titled “Built-in Reporters”](#built-in-reporters)

Knip provides the following built-in reporters:

- `codeowners`

- `compact`

- [`disclosure`](#disclosure)

- [`github-actions`](#github-actions)

- [`json`](#json)

- [`markdown`](#markdown)

- [`codeclimate`](#codeclimate)

- `symbols` (default)

Example usage:

Terminal window

```

knip --reporter compact

```

### JSON

[
Section titled “JSON”](#json)

The built-in `json` reporter output is meant to be consumed by other tools. It
reports in JSON format with unused `files` and `issues` as an array with one
object per file structured like this:

```

{

  "files": ["src/unused.ts"],

  "issues": [

    {

      "file": "package.json",

      "owners": ["@org/admin"],

      "dependencies": [{ "name": "jquery", "line": 5, "col": 6, "pos": 71 }],

      "devDependencies": [{ "name": "lodash", "line": 9, "col": 6, "pos": 99 }],

      "unlisted": [{ "name": "react" }, { "name": "@org/unresolved" }],

      "exports": [],

      "types": [],

      "duplicates": []

    },

    {

      "file": "src/Registration.tsx",

      "owners": ["@org/owner"],

      "dependencies": [],

      "devDependencies": [],

      "binaries": [],

      "unresolved": [

        { "name": "./unresolved", "line": 8, "col": 23, "pos": 407 }

      ],

      "exports": [{ "name": "unusedExport", "line": 1, "col": 14, "pos": 13 }],

      "types": [

        { "name": "unusedEnum", "line": 3, "col": 13, "pos": 71 },

        { "name": "unusedType", "line": 8, "col": 14, "pos": 145 }

      ],

      "enumMembers": {

        "MyEnum": [

          { "name": "unusedMember", "line": 13, "col": 3, "pos": 167 },

          { "name": "unusedKey", "line": 15, "col": 3, "pos": 205 }

        ]

      },

      "classMembers": {

        "MyClass": [

          { "name": "unusedMember", "line": 40, "col": 3, "pos": 687 },

          { "name": "unusedSetter", "line": 61, "col": 14, "pos": 1071 }

        ]

      },

      "duplicates": ["Registration", "default"]

    }

  ]

}

```

The keys match the [reported issue types](../reference/issue-types). Example usage:

Terminal window

```

knip --reporter json

```

### Github Actions

[
Section titled “Github Actions”](#github-actions)

TODO

Example usage:

Terminal window

```

knip --reporter github-actions

```

### Markdown

[
Section titled “Markdown”](#markdown)

The built-in `markdown` reporter output is meant to be saved to a Markdown file.
This allows following the changes in issues over time. It reports issues in
Markdown tables separated by issue types as headings, for example:

```

# Knip report

## Unused files (1)

- src/unused.ts

## Unlisted dependencies (2)

| Name            | Location          | Severity |

| :-------------- | :---------------- | :------- |

| unresolved      | src/index.ts:8:23 | error    |

| @org/unresolved | src/index.ts:9:23 | error    |

## Unresolved imports (1)

| Name         | Location           | Severity |

| :----------- | :----------------- | :------- |

| ./unresolved | src/index.ts:10:12 | error    |

```

### Disclosure

[
Section titled “Disclosure”](#disclosure)

This reporter is useful for sharing large reports. Groups of issues are rendered
in a closed state initially. The reporter renders this:

```

$ knip --reporter disclosure

<details>

<summary>Unused files (2)</summary>

```

unused.ts

dangling.js

```

</details>

<details>

<summary>Unused dependencies (2)</summary>

```

unused-dep package.json

my-package package.json

```

</details>

```

The above can be copy-pasted where HTML and Markdown is supported, such as a
GitHub issue or pull request, and renders like so:

Unused files (2)

```

unused.ts

dangling.js

```

Unused dependencies (2)

```

unused-dep     package.json

my-package     package.json

```

### CodeClimate

[
Section titled “CodeClimate”](#codeclimate)

The built-in `codeclimate` reporter generates output in the Code Climate Report
JSON format. Example usage:

```

$ knip --reporter codeclimate

[

  {

    "type": "issue",

    "check_name": "Unused exports",

    "description": "isUnused",

    "categories": ["Bug Risk"],

    "location": {

      "path": "path/to/file.ts",

      "positions": {

        "begin": {

          "line": 6,

          "column": 1

        }

      }

    }

    "severity": "major",

    "fingerprint": "e9789995c1fe9f7d75eed6a0c0f89e84",

  }

]

```

## Custom Reporters

[
Section titled “Custom Reporters”](#custom-reporters)

When the provided built-in reporters are not sufficient, a custom local reporter
can be implemented or an external reporter can be used. Multiple reporters can
be used at once by repeating the `--reporter` argument.

The results are passed to the function from its default export and can be used
to write issues to `stdout`, a JSON or CSV file, or sent to a service. It
supports a local JavaScript or TypeScript file or an external dependency.

### Local

[
Section titled “Local”](#local)

The default export of the reporter should be a function with this interface:

```

type Reporter = async (options: ReporterOptions): void;

type ReporterOptions = {

  report: Report;

  issues: Issues;

  counters: Counters;

  configurationHints: ConfigurationHints;

  isDisableConfigHints: boolean;

  isTreatConfigHintsAsErrors: boolean;

  cwd: string;

  isProduction: boolean;

  isShowProgress: boolean;

  options: string;

};

```

The data can then be used to write issues to `stdout`, a JSON or CSV file, or
sent to a service.

Here’s a most minimal reporter example:

./my-reporter.ts

```

import type { Reporter } from 'knip';

const reporter: Reporter = function (options) {

  console.log(options.issues);

  console.log(options.counters);

};

export default reporter;

```

Example usage:

Terminal window

```

knip --reporter ./my-reporter.ts

```

### External

[
Section titled “External”](#external)

Pass `--reporter [pkg-name]` to use an external reporter. The default exported
function of the `main` script (default: `index.js`) will be invoked with the
`ReporterOptions`, just like a local reporter.

## Preprocessors

[
Section titled “Preprocessors”](#preprocessors)

A preprocessor is a function that runs after the analysis is finished. It
receives the results from the analysis and should return data in the same
shape/structure (unless you pass it to only your own reporter).

The data goes through the preprocessors before the final data is passed to the
reporters. There are no built-in preprocessors. Just like reporters, use e.g.
`--preprocessor ./my-preprocessor` from the command line (can be repeated).

The default export of the preprocessor should be a function with this interface:

```

type Preprocessor = async (options: ReporterOptions) => ReporterOptions;

```

Like reporters, you can use local JavaScript or TypeScript files and external
npm packages as preprocessors.

Example preprocessor:

./preprocess.ts

```

import type { Preprocessor } from 'knip';

const preprocess: Preprocessor = function (options) {

  // modify options.issues and options.counters

  return options;

};

export default preprocess;

```

Example usage:

Terminal window

```

knip --preprocessor ./preprocess.ts

```

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/features/reporters.md)

[
Previous
Compilers ](/features/compilers) [
Next
Script Parser ](/features/script-parser)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Rules & Filters

**Source:** https://knip.dev/features/rules-and-filters

---

# Rules & Filters

Use rules or filters to customize Knip’s output. This has various use cases, a
few examples:

- Temporarily focus on a specific issue type.

- You don’t want to see unused `type`, `interface` and `enum` exports reported.

- Specific issue types should be printed, but not counted against the total
  error count.

If you’re looking to handle one-off exceptions, also see [JSDoc tags](../reference/jsdoc-tsdoc-tags).

## Filters

[
Section titled “Filters”](#filters)

You can `--include` or `--exclude` any of the reported issue types to slice &
dice the report to your needs. Alternatively, they can be added to the
configuration (e.g. `"exclude": ["dependencies"]`).

Use `--include` to report only specific issue types. The following example
commands do the same:

- Terminal window

```

knip --include files --include dependencies

knip --include files,dependencies

```

Or the other way around, use `--exclude` to ignore the types you’re not
interested in:

Terminal window

```

knip --include files --exclude enumMembers,duplicates

```

Also see the [list of issue types](../reference/issue-types).

### Shorthands

[
Section titled “Shorthands”](#shorthands)

Knip has shortcuts to include only specific issue types.

The `--dependencies` flag includes:

`dependencies` (and `devDependencies` + `optionalPeerDependencies`)

- `unlisted`

- `binaries`

- `unresolved`

-

The `--exports` flag includes:

`exports`

- `types`

- `enumMembers`

- `duplicates`

-

The `--files` flag is a shortcut for `--include files`

## Rules

[
Section titled “Rules”](#rules)

Use `rules` in the configuration to customize the issue types that count towards
the total error count, or to exclude them altogether.

ValueDefaultPrintedCountedDescription`"error"`✓✓✓Similar to the `--include` filter`"warn"`-✓-Printed in faded/gray color`"off"`---Similar to the `--exclude` filter

Example:

knip.json

```

{

  "rules": {

    "files": "warn",

    "classMembers": "off",

    "duplicates": "off"

  }

}

```

Also see the [issue types overview](../reference/issue-types).

NOTE: If the `dependencies` issue type is included, the `devDependencies` and
`optionalPeerDependencies` types can still be set to `"warn"` separately.

The rules are modeled after the ESLint `rules` configuration, and could be
extended in the future.

## Rules or filters?

[
Section titled “Rules or filters?”](#rules-or-filters)

Filters are meant to be used as command-line flags, rules allow for more
fine-grained configuration.

- Rules are more fine-grained since they also have “warn”.

- Rules could be extended in the future.

- Filters can be set in configuration and from CLI (rules only in
  configuration).

- Filters have shorthands (rules don’t have this).

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/features/rules-and-filters.md)

[
Previous
Source Mapping ](/features/source-mapping) [
Next
Auto-fix ](/features/auto-fix)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Script Parser

**Source:** https://knip.dev/features/script-parser

---

# Script Parser

Knip parses shell commands and scripts to find additional dependencies, entry
files and configuration files in various places:

- In [`package.json`](#packagejson)

- In [CLI arguments](#cli-arguments)

- In [scripts](#scripts)

- In [source code](#source-code)

Shell scripts can be read and statically analyzed, but they’re not executed.

## package.json

[
Section titled “package.json”](#packagejson)

The `main`, `bin`, `exports` and `scripts` fields may contain entry files. Let’s
take a look at this example:

- package.json

```

{

  "name": "my-package",

  "main": "index.js",

  "exports": {

    "./lib": {

      "import": "./dist/index.mjs",

      "require": "./dist/index.cjs"

    }

  },

  "bin": {

    "program": "bin/cli.js"

  },

  "scripts": {

    "build": "rollup src/entry.ts",

    "start": "node --loader tsx server.ts"

  }

}

```

From this example, Knip automatically adds the following files as entry files:

`index.js`

- `./dist/index.mjs`

- `./dist/index.cjs`

- `bin/cli.js`

- `src/entry.ts`

- `server.ts`

### Excluded files

[
Section titled “Excluded files”](#excluded-files)

Knip would not add the `exports` if the `dist` folder is matching a pattern in a
relevant `.gitignore` file or `ignore` option.

Knip does not add scripts without a standard extension. For instance, the
`bin/tool` file might be a valid executable for Node.js, but wouldn’t be added
or parsed by Knip.

### CLI Arguments

[
Section titled “CLI Arguments”](#cli-arguments)

When parsing the `scripts` of `package.json` and other files, Knip detects
various types of inputs. Some examples:

- The first positional argument is usually an entry file

- Configuration files are often in the `-c` or `--config` argument

- The `--require`, `--loader` or `--import` arguments are often dependencies

```

{

  "name": "my-lib",

  "scripts": {

    "start": "node --import tsx/esm run.ts",

    "bundle": "tsup -c tsup.lib.config.ts",

    "type-check": "tsc -p tsconfig.app.json"

  }

}

```

The `"start"` script will have `tsx` marked as a referenced dependency, and adds
`run.ts` as an entry file.

Additionally, the following files are detected as configuration files:

- `tsup.lib.config.ts` - to be handled by the tsup plugin

- `tsconfig.app.json` - to be handled by the TypeScript plugin

Such executables and their arguments are all defined in plugins separately for
fine-grained results.

## Scripts

[
Section titled “Scripts”](#scripts)

Plugins may also use the script parser to extract entry files and dependencies
from commands. A few examples:

- GitHub Actions: workflow files may contain `run` commands (e.g.
  `.github/workflows/ci.yml`)

- Husky & Lefthook: Git hooks such as `.git/hooks/pre-push` contain scripts;
  also `lefthook.yml` has `run` commands

- Lint Staged: configuration values are all commands

- Nx: task executors and `nx:run-commands` executors in `project.json` contains
  scripts

- Release It: `hooks` contain commands

Plugins can also return configuration files. Some examples:

- The Angular plugin detects `options.tsConfig` as a TypeScript config file

- The GitHub Actions plugin parses `run` commands which may contain
  configuration file paths

## Source Code

[
Section titled “Source Code”](#source-code)

When Knip is walking the abstract syntax trees (ASTs) of JavaScript and
TypeScript source code files, it looks for imports and exports. But there’s a
few more (rather obscure) things that Knip detects in the process. Below are
examples of additional scripts Knip parses to find entry files and dependencies.

### bun

[
Section titled “bun”](#bun)

If the `bun` dependency is imported in source code, Knip considers the contents
of `$` template tags to be scripts:

```

import { $ } from 'bun';

await $`bun boxen I ❤ unicorns`;

await $`boxen I ❤ unicorns`;

```

Parsing the script results in the `boxen` binary (the `boxen-cli` dependency) as
referenced (twice).

### execa

[
Section titled “execa”](#execa)

If the `execa` dependency is imported in source code, Knip considers the
contents of `$` template tags to be scripts:

```

await $({ stdio: 'inherit' })`c8 node hydrate.js`;

```

Parsing the script results in `hydrate.js` added as an entry file and the `c8`
binary/dependency as referenced.

### zx

[
Section titled “zx”](#zx)

If the `zx` dependency is imported in source code, Knip considers the contents
of `$` template tags to be scripts:

```

await $`node scripts/parse.js`;

```

This will add `scripts/parse.js` as an entry file.

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/features/script-parser.md)

[
Previous
Reporters & Preprocessors ](/features/reporters) [
Next
Configuring Project Files ](/guides/configuring-project-files)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Source Mapping

**Source:** https://knip.dev/features/source-mapping

---

# Source Mapping

Knip is mostly interested in source code. Analyzing build artifacts hurts
performance and often leads to false positives, as they potentially contain
bundled code and unresolvable imports.

That’s why Knip tries to map such build artifacts back to their original source
files and analyze those instead. This is done based on `tsconfig.json` settings.

## Example 1: package.json

[
Section titled “Example 1: package.json”](#example-1-packagejson)

Let’s look at an example case with `package.json` and `tsconfig.json` files, and
see how “dist” files are mapped to “src” files.

- package.json

```

{

  "name": "my-workspace",

  "main": "index.js",

  "exports": {

    ".": "./src/entry.js",

    "./feat": "./lib/feat.js",

    "./public": "./dist/app.js",

    "./public/*": "./dist/*.js",

    "./public/*.js": "./dist/*.js",

    "./dist/internal/*": null,

  },

}

```

With this TypeScript configuration:

tsconfig.json

```

{

  "compilerOptions": {

    "baseUrl": "src",

    "outDir": "dist"

  }

}

```

`./src/entry.js` is not in an `outDir` folder, so it’s added as an entry file

- `./lib/feat.js` is not in an `outDir` folder, so it’s added as an entry file

- `./dist/app.js` is in a `dist` folder and mapped to `./src/app.{js,ts}` (¹)

- `./dist/*.js` is in a `dist` folder and mapped to `./src/**/*.{js,ts}` (¹)

- `./dist/internal/*` is translated to `./dist/internal/**` and files in this
  directory and deeper are ignored when globbing entry files

(¹) full extensions list is actually: `js`, `mjs`, `cjs`, `jsx`, `ts`, `tsx`,
`mts`, `cts`

In `--debug` mode, look for “Source mapping” to see this in action.

Tip

Using `./dist/*.js` means that all files matching `./src/**/*.{js,ts}` are added
as entry files. By default, unused exports of entry files are not reported. Use
[includeEntryExports](../reference/configuration#includeentryexports) to include them.

## Example 2: monorepo

[
Section titled “Example 2: monorepo”](#example-2-monorepo)

Let’s say we have this module in a monorepo that imports `helper` from another
workspace in the same monorepo:

index.js

```

import { helper } from '@org/shared';

```

The target workspace `@org/shared` has this `package.json`:

package.json

```

{

  "name": "@org/shared",

  "main": "dist/index.js"

}

```

The module resolver will resolve `@org/shared` to `dist/index.js`. That file is
usually compiled and git-ignored, while Knip wants the source file instead.

Tip

You may need to compile build artifacts to `outDir` first before Knip can
successfully apply source mapping for internal references in a monorepo.

If the target workspace has a `tsconfig.json` file with an `outDir` option, Knip
will try to map the “dist” file to the “src” file. Then if `src/index.ts`
exists, Knip will use that file instead of `dist/index.js`.

Currently this only works based on `tsconfig.json`, in the future more source
mappings may be added.

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/features/source-mapping.md)

[
Previous
Integrated Monorepos ](/features/integrated-monorepos) [
Next
Rules & Filters ](/features/rules-and-filters)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Configuring Project Files

**Source:** https://knip.dev/guides/configuring-project-files

---

# Configuring Project Files

The `entry` and `project` file patterns are the first and most important
options. Getting those right is essential to get the most value and performance
out of Knip.

TL;DR;

- Start with defaults. Only add targeted `entry` overrides when needed.

- Use `project` patterns (with negations) to define “what belongs to the
  codebase” for unused file detection.

- Use production mode to exclude tests and other non-production files.

- Use `ignore` only to suppress issues in specific files. It does not exclude
  files from analysis.

Let’s dive in and expand on all of these.

## Entry files

[
Section titled “Entry files”](#entry-files)

Avoid adding too many files as `entry` files:

- Knip does not report [unused exports](../typescript/unused-exports) in entry files by default.

- Proper `entry` and `project` patterns allow Knip to find unused files and
  exports.

## Unused files

[
Section titled “Unused files”](#unused-files)

Files are reported as unused if they are in the set of `project` files, but are
not resolved from the `entry` files:

-

```

unused files = project files - (entry files + resolved files)

```

See [entry files](../explanations/entry-files) to see where Knip looks for entry files. Fine-tune `entry`
and adjust `project` to fit your codebase.

Tip

Use negated `project` patterns to precisely include/exclude files for unused
files detection.

Use `ignore` to suppress issues in matching files; it does not remove those
files from analysis.

## Negated patterns

[
Section titled “Negated patterns”](#negated-patterns)

When there are too many files in the analysis, start here.

For instance, routes are entry files except those prefixed with an underscore:

```

{

  "entry": ["src/routes/*.ts", "!src/routes/_*.ts"]

}

```

Some files are not part of your source and are reported as unused (false
positives)? Use negated `project` patterns:

```

{

  "entry": ["src/index.ts"],

  "project": ["src/**/*.ts", "!src/exclude/**"]

}

```

❌   Don’t use `ignore` for generated artifacts:

knip.json

```

{

  "entry": ["src/index.ts", "scripts/*.ts"],

  "ignore": ["build/**", "dist/**", "src/generated.ts"]

}

```

✅   Do define your project boundaries:

knip.json

```

{

  "entry": ["src/index.ts", "scripts/*.ts"],

  "project": ["src/**", "scripts/**"],

  "ignore": ["src/generated.ts"]

}

```

Why is this better:

`project` defines “what belongs to the codebase” so build outputs are not part
of the analysis and don’t appear in unused file detection at all

- `ignore` is for the few files that should be analyzed but contain exceptions

- increases performance by analyzing only source files

## Ignore issues in specific files

[
Section titled “Ignore issues in specific files”](#ignore-issues-in-specific-files)

Use `ignore` when a specific analyzed file is not handled properly by Knip or
intentionally contains unused exports (e.g. generated files exporting
“everything”):

```

{

  "entry": ["src/index.ts"],

  "project": ["src/**/*.ts"],

  "ignore": ["src/generated.ts"]

}

```

Also see [ignoreExportsUsedInFile](../reference/configuration#ignoreexportsusedinfile) for a more targeted approach.

## Production Mode

[
Section titled “Production Mode”](#production-mode)

Default mode includes tests and other non-production files in the analysis. To
focus on production code, use [production mode](../features/production-mode).

Don’t try to exclude tests via `ignore` or negated `project` patterns. That’s
inefficient and ineffective due to entries added by plugins. Use production mode
instead.

❌   Don’t do this:

```

{

  "ignore": ["**/*.test.js"]

}

```

Why not: `ignore` only hides issues from the report; it does not exclude files
from analysis.

❌   Also don’t do this:

```

{

  "entry": ["index.ts", "!**/*.test.js"]

}

```

Why not: plugins for test frameworks add test file as `entry` files, you can’t
and shouldn’t override that globally.

❌   Or this:

```

{

  "project": ["**/*.ts", "!**/*.spec.ts"]

}

```

Why not: `project` is used for unused file detection. Negating test files here
is ineffective, because they’re `entry` files.

✅   Do this instead:

Terminal window

```

knip --production

```

To fine-tune the resulting production file set, for instance to exclude test
helper files that still show as unused, use the exclamation mark suffix on
production patterns:

```

{

  "entry": ["src/index.ts!"],

  "project": ["src/**/*.ts!", "!src/test-helpers/**!"]

}

```

Remember to keep adding the exclamation mark `suffix!` for production file
patterns.

Tip

Use the exclamation mark (`!`) on both ends (`!`) to exclude files in production
mode.

## Defaults & Plugins

[
Section titled “Defaults & Plugins”](#defaults--plugins)

To reiterate, the default `entry` and `project` files for each workspace:

```

{

  "entry": [

    "{index,cli,main}.{js,cjs,mjs,jsx,ts,cts,mts,tsx}",

    "src/{index,cli,main}.{js,cjs,mjs,jsx,ts,cts,mts,tsx}"

  ],

  "project": ["**/*.{js,cjs,mjs,jsx,ts,cts,mts,tsx}!"]

}

```

Next to this, there are other places where [Knip looks for entry files](../explanations/entry-files).

Additionally, [plugins have plenty of entry files configured](../explanations/plugins#entry-files) that are
automatically added as well.

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/guides/configuring-project-files.md)

[
Previous
Script Parser ](/features/script-parser) [
Next
Troubleshooting ](/guides/troubleshooting)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Contributing to Knip

**Source:** https://knip.dev/guides/contributing

---

# Contributing to Knip

Here are some ways to contribute to Knip:

- Spread the word!

- [Star the project](https://github.com/webpro-nl/knip)

- [File an issue with a reproduction](./issue-reproduction)

- [Pull requests are welcome](https://github.com/webpro-nl/knip/blob/main/.github/CONTRIBUTING.md)

[Writing a plugin](./writing-a-plugin) is a great and fun way to get started.

The [CONTRIBUTING.md](https://github.com/webpro-nl/knip/blob/main/.github/CONTRIBUTING.md) and [DEVELOPMENT.md](https://github.com/webpro-nl/knip/blob/main/.github/DEVELOPMENT.md) guides should get you up and
running quickly.

The main goal of Knip is to keep projects clean & tidy. Everything that
contributes to that goal is welcome!

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/guides/contributing.md)

[
Previous
Issue Reproduction ](/guides/issue-reproduction) [
Next
Namespace Imports ](/guides/namespace-imports)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Handling Issues

**Source:** https://knip.dev/guides/handling-issues

---

# Handling Issues

A long report can be frustrating. The list may contain false positives, but also
tons of useful information. To get the most value out of Knip, it may require
some initial configuration.

This guide helps you deal with false positives to find solutions and create the
perfect report, with minimal configuration that will keep your project tidy.

If you start out using Knip in a large project and have a long report, it makes
sense to go over the issue types one by one. For instance, reducing the number
of unused files will also reduce the number of unused dependencies.

- [Unused files](#unused-files)

- [Unused dependencies](#unused-dependencies)

- [Unresolved imports](#unresolved-imports)

- [Unused exports](#unused-exports)

## Unused files

[
Section titled “Unused files”](#unused-files)

Getting the list of unused files right trickles down into the other issue types
as well, so we start here. Files are reported as unused if they are in the set
of `project` files, but not in the set of files resolved from the `entry` files:

-

```

unused files = project files - (entry files + resolved files)

```

Let’s go over common causes for unused files:

[Missing generated files](#missing-generated-files)

- [Dynamic import specifiers](#dynamic-import-specifiers)

- [Unsupported arguments in scripts](#unsupported-arguments-in-scripts)

- [Unsupported file formats](#unsupported-file-formats)

- [Missing plugin](#missing-plugin)

- [Incomplete plugin](#incomplete-plugin)

- [TypeScript path aliases in monorepos](#typescript-path-aliases-in-monorepos)

- [Relative paths across workspaces](#relative-paths-across-workspaces)

- [Integrated monorepos](#integrated-monorepos)

- [Auto-mocking or auto-imports](#auto-mocking-or-auto-imports)

In most cases you can add `entry` patterns manually.

Use `--files` to [filter the report](../features/rules-and-filters#filters) and focus only on unused files:

Terminal window

```

knip --files

```

This works with other issue types as well. For instance, use `--dependencies` to
focus only on dependencies and exclude issues related to unused files and
exports.

Caution

Don’t add unused files to the `ignore` option before reading [configuring
project files](./configuring-project-files). Learn why and when to use `entry`, `project`, production
mode and `ignore` patterns for better results and performance.

### Missing generated files

[
Section titled “Missing generated files”](#missing-generated-files)

For certain features, Knip needs to run after relevant files are generated. For
instance, [source mapping](../features/source-mapping) in a monorepo may require files to be built into
`dist` folders first. And generated files in the `src` directory may import
other files. For instance, the `src/routeTree.gen.ts` file generated by
`@tanstack/router` must exists so Knip can find the imported route files.

**Solution**: compile and/or generate the relevant files first so Knip can
resolve and find the source files.

If Knip still reports false positives, you may need to strategically add an
`entry` file manually.

### Dynamic import specifiers

[
Section titled “Dynamic import specifiers”](#dynamic-import-specifiers)

Dynamic import specifiers aren’t resolved, such as:

```

const entry = await import(path.join(baseDir, 'entry.ts'));

```

**Solution**: add `entry.ts` to `entry` patterns.

### Unsupported arguments in scripts

[
Section titled “Unsupported arguments in scripts”](#unsupported-arguments-in-scripts)

Some tooling command arguments aren’t recognized:

```

{

  "name": "my-lib",

  "version": "1.0.0",

  "scripts": {

    "build": "unknown-build-cli --entry production.ts"

  }

}

```

**Solution**: add `production.ts` to `entry` patterns.

This works the same for any script, also those in GitHub Actions workflows or
Git hooks. See [script parser](../features/script-parser) for more details about Knip’s script parser.

### Unsupported file formats

[
Section titled “Unsupported file formats”](#unsupported-file-formats)

Entry files referenced in HTML files (e.g. `<script src="production.js">`).

```

<html>

  <body>

    <script type="module" src="production.js"></script>

  </body>

</html>

```

**Solution**: add `production.js` to `entry` patterns. Or add an `.html`
compiler to extract and resolve the value of `<script src>` elements.

Knip has support for some popular framework formats through [compilers](../features/compilers), and
additional compilers can be added for for any file type. The recommended
solution is usually to add the file as shown in each example as an `entry` file.

### Missing plugin

[
Section titled “Missing plugin”](#missing-plugin)

You might be using a tool or framework for which Knip doesn’t have a plugin
(yet). Configuration and entry files (and related dependencies) may be reported
as unused because there is no plugin yet that would include those files. Two
examples:

- Configuration file `tool.config.js` contains a reference to the package
  `"@tool/plugin"` → both the file and the dependency are reported as unused.

- A framework automatically imports all files matching `src/models/*.ts` → those
  files are reported as unused.

**Solution**: [create a new plugin](./writing-a-plugin) for the tool or framework that’s not [in
the list](../reference/plugins) yet. Or work around it and add `entry` patterns and maybe ignore a
dependency or two (using [`ignoreDependencies`](../reference/configuration#ignoredependencies)).

### Incomplete plugin

[
Section titled “Incomplete plugin”](#incomplete-plugin)

Files may be reported as unused if existing plugins do not include that entry
file pattern yet. See the [plugins section of entry files](../explanations/plugins#entry-files) for more details.

**Solution**: [override plugin configuration](../explanations/entry-files#plugins) to customize default patterns
for existing plugins. Or even better: send a pull request to improve the plugin.

### TypeScript path aliases in monorepos

[
Section titled “TypeScript path aliases in monorepos”](#typescript-path-aliases-in-monorepos)

When using TypeScript path aliases in import specifiers, aliases referencing
other workspaces are not special-cased. This may cause false positives. For
Knip, it’s better to be explicit and list other workspaces as dependencies in
`package.json`.

**Solution**: move such “workspace aliases” from `compilerOptions`…

tsconfig.json

```

{

  "compilerOptions": {

    "paths": {

      "@org/common/*": ["packages/common/*"]

    }

  }

}

```

…to `dependencies` or `devDependencies` in `package.json`:

package.json

```

{

  "name": "@org/lib",

  "dependencies": {

    "@org/common": "workspace:*"

  }

}

```

An additional benefit is that Knip will report unused and unlisted dependencies
from now on.

Also see [FAQ: Why can’t I use path aliases to reference other workspaces?](../reference/faq#why-cant-i-use-path-aliases-to-reference-other-workspaces)

### Relative paths across workspaces

[
Section titled “Relative paths across workspaces”](#relative-paths-across-workspaces)

Relative paths to import from other workspaces are not special-cased. For Knip,
it’s better to be explicit and list other workspaces as dependencies in
`package.json` to be used in imports.

**Solution**: migrate from relative paths…

```

import { something } from '../../common/file.ts';

```

…to dependency-based imports…

```

import { something } from '@org/common';

```

An additional benefit is that Knip will report unused and unlisted dependencies
from now on.

Also see [TypeScript path aliases in monorepos](#typescript-path-aliases-in-monorepos).

### Integrated monorepos

[
Section titled “Integrated monorepos”](#integrated-monorepos)

Multiple instances of configuration files like `.eslintrc` and
`jest.config.json` across the repository may be reported as unused when working
in a (mono)repo with a single `package.json`.

**Solution**: see [integrated monorepos](../features/integrated-monorepos) for more details and how to
configure plugins to target those configuration files.

### Auto-mocking or auto-imports

[
Section titled “Auto-mocking or auto-imports”](#auto-mocking-or-auto-imports)

Some frameworks have features like “auto-mocking” or “auto-imports” enabled,
such as Jest and Nuxt.

**Solution**: include such entry files by extending the `entry` file patterns.
This is recommended in most cases:

```

{

  "entry": ["src/index.ts", "src/models/*.ts"]

}

```

Alternatively, exceptions and outliers can be excluded from the analysis using
negated `project` patterns:

```

{

  "project": ["src/**/*.ts", "!**/__mocks__/**"]

}

```

## Unused dependencies

[
Section titled “Unused dependencies”](#unused-dependencies)

Dependencies imported in unused files are reported as unused dependencies.
That’s why it’s strongly recommended to try and remedy [unused files](#unused-files) first.
Better `entry` and `project` file coverage will solve many cases of reported
unused dependencies.

The most common causes for unused dependencies include:

- [Missing or incomplete plugins](#missing-or-incomplete-plugin)

- [Unrecognized references](#unrecognized-reference)

- [Type Definition Packages](#type-definition-packages)

Use `--dependencies` to [filter the report](../features/rules-and-filters#filters) and focus only on issues related
to dependencies:

Terminal window

```

knip --dependencies

```

Monorepo

In a monorepo, a dependency is unused in the root workspace’s `package.json` if
it’s also listed in an descendent workspace, and referenced only in the
descendent workspace.

### Missing or incomplete plugin

[
Section titled “Missing or incomplete plugin”](#missing-or-incomplete-plugin)

If a plugin exists and the dependency is referenced in the configuration file,
but its custom dependency finder does not detect it, then that’s a false
positive. Please open a pull request or issue to fix it.

**Solution**: adding the configuration file as an `entry` file pattern may be a
temporary stopgap that fixes your situation, but it’s better to create a new
plugin or fix an existing one.

### Unrecognized reference

[
Section titled “Unrecognized reference”](#unrecognized-reference)

Sometimes a reference to a dependency is unrecognizable or unreachable to Knip,
so it’s a false positive and incorrectly reported as unused.

**Solution**: add a new plugin or improve an existing one. If you don’t feel
like a plugin could solve it, a last resort is to use [ignoreDependencies](../reference/configuration#ignoredependencies).

If a binary (or “executable”) is referenced you’ll want to use `ignoreBinaries`
instead. See [unlisted binaries](#unlisted-binaries).

### Type Definition Packages

[
Section titled “Type Definition Packages”](#type-definition-packages)

#### Bundled types

[
Section titled “Bundled types”](#bundled-types)

Many packages come with their type definitions bundled. This means the
`package.json#types` field in the package points to an internal/local type
definition file. In this case, the separate types package is obsolete.

Knip reporting this is also useful for future regressions: if a package had a
DefinitelyTyped or similar package for its types before and later on starts
shipping those types bundled with the source code, Knip will report the obsolete
types dependency as unused.

Examples include ESLint, Webpack and React Router, rendering the
`@types/eslint`, `@types/webpack` and `@types/react-router` dependencies
obsolete respectively.

**Solution**: remove the types dependency (often `@types/...`)

#### Production types

[
Section titled “Production types”](#production-types)

Knip is strict in the divide between `dependencies` and `devDependencies`. Some
packages are published with one or more type packages listed in `dependencies`.
In strict production mode, even when re-exported and part of the package’s
public API, Knip does not try to figure out what exactly are “production types”
and expects those in `devDependencies`.

**Solution**: list exceptions in [ignoreDependencies](../reference/configuration#ignoredependencies).

### Unlisted dependencies

[
Section titled “Unlisted dependencies”](#unlisted-dependencies)

This means that a dependency is referenced directly in source code or
configuration, but not listed in `package.json`.

An unlisted dependency is usually a transitive dependency that’s imported or
referenced directly. The dependency is installed (since it’s a dependency of
another dependency) and lives in `node_modules`, but it’s not listed explicitly
in `package.json`.

You should not rely on transitive dependencies for various reasons, including
control, security and stability.

**Solution**: install and list the dependency in `dependencies` or
`devDependencies`.

### Unlisted binaries

[
Section titled “Unlisted binaries”](#unlisted-binaries)

Binaries are executable Node.js scripts. Some npm packages, when installed, add
one or more executable files to be used from scripts in `package.json`. Examples
include TypeScript that comes with the `tsc` binary, ESLint comes with `eslint`,
Next.js with `next`, and so on.

Knip detects such binaries in scripts and checks whether there’s a package
installed that includes that binary. It looks up the `bin` field in the
`package.json` file of installed packages. If it doesn’t find it, it will be
reported as an unlisted binary as there is no package that contains it.

Binaries that are installed on the OS already and thus likely not meant to be
installed from npm are not reported as unlisted (details: [list of ignored
binaries in source](https://github.com/webpro-nl/knip/blob/b70958a58ea255ee7a7831e404786da807ca93d7/packages/knip/src/constants.ts#L37-L139)).

#### Missing binaries

[
Section titled “Missing binaries”](#missing-binaries)

An unused dependency and an unlisted binary with the same name indicates
`node_modules` not containing the relevant package. And this might be caused by
either the way your package manager installs dependencies and binaries, or by
not running Knip from the root of the repository.

**Solution**: run Knip from the project root. If needed, [lint workspaces
individually](../features/monorepos-and-workspaces#lint-a-single-workspace).

Sometimes binaries and how they’re reported can be a bit confusing. See this
example:

```

{

  "name": "lib",

  "scripts": {

    "commitlint": "commitlint --edit"

  },

  "devDependencies": {

    "@commitlint/cli": "*"

  }

}

```

This example works fine without anything reported, as the `@commitlint/cli`
package includes the `commitlint` binary. However, some script may contain
`npx commitlint` and here Knip assumes `commitlint` is the name of the package.
This technically works, as `commitlint` is a transitive dependency of
`@commitlint/cli`.

**Solution**: use `npx @commitlint/cli`

In some cases, using `npx` in a script may result in Knip not understanding
intention without an explicit `--yes` or `--no-install` flag.

**Solution**: use `npx --yes` or `npx --no-install` so Knip will either ignore
or consider the binary and package(s) referenced, respectively.

## Unresolved imports

[
Section titled “Unresolved imports”](#unresolved-imports)

Knip may ignore or be unable to resolve an import specifier or dependency
references. The most common causes for unresolved imports:

- [Template strings](#template-strings)

- [Extensionless imports](#unrecognized-reference)

- [Unrecognized path aliases](#unrecognized-path-aliases)

- [External aliased imports](#external-aliased-imports)

### Template strings

[
Section titled “Template strings”](#template-strings)

Using template strings in dynamic imports might be ignored or not handled
properly by Knip, resulting in false positives. Examples of dynamic import
template strings:

```

import(`./${value}.ts`);

import(`@org/name/dist/${value}.js`);

```

**Solution**: for internal source files, add the file(s) to the `entry`
patterns. For external dependencies, add the dependency to the
`ignoreDependencies` list.

### Extensionless imports

[
Section titled “Extensionless imports”](#extensionless-imports)

Knip does not support extensionless imports for some non-standard extensions,
such as for `.svg` files. Bundlers like Webpack may support this, but Knip does
not. Here’s an example:

App.vue

```

import Component from './Component'; // → Should resolve to ./Component.vue

import ArrowIcon from '../icons/Arrow'; // → Does NOT resolve to ../icons/Arrow.svg

```

The first import is resolved properly, because `.vue` is a known extension if
the Vue plugin is enabled. The second import might not be resolved, because
`.svg` is not a known extension.

The recommendation is to always add the extension when importing such files,
similar to how standard ES Modules specifies file extensions are necessary.

### Unrecognized path aliases

[
Section titled “Unrecognized path aliases”](#unrecognized-path-aliases)

Knip considers TS config path aliases and [paths configured in knip.json](../reference/configuration#paths),
but not those in e.g. Webpack or Vite configurations.

**Solution**: configure [paths](../reference/configuration#paths) or try relative imports. Otherwise, use
[`ignoreUnresolved`](../reference/configuration#ignoreunresolved) as a last resort.

### External aliased imports

[
Section titled “External aliased imports”](#external-aliased-imports)

External libraries may use aliased imports that aren’t resolved by Knip.

For instance, [unplugin-icons](#incomplete-plugin) does this to import icons from icon sets as
components. Such imports are reported as unused. Use the [`paths` configuration
option](#integrated-monorepos) to tell Knip where to find the icon types:

knip.json

```

{

  "paths": {

    "~icons/*": ["node_modules/unplugin-icons/types/[framework].d.ts"]

  }

}

```

Where `[framework]` is the name of the framework you’re using (see [available
types](#build-artifacts-and-ignored-files)).

**Solution**: try [—include-libs](#external-libraries) or configure [paths](../reference/configuration#paths).

## Unused exports

[
Section titled “Unused exports”](#unused-exports)

By default, Knip does not report unused exports of `entry` files.

The most common causes for unused exports include:

- [Namespace enumerations](#namespace-enumerations)

- [External libraries](#external-libraries)

Use the `--exports` flag to [filter](../features/rules-and-filters#filters) and focus only on issues related to
exports:

Terminal window

```

knip --exports

```

Use [includeEntryExports](../reference/configuration#includeentryexports) to report unused exports of entry files as well.
This can be set per workspace.

### Namespace enumerations

[
Section titled “Namespace enumerations”](#namespace-enumerations)

For exports on an imported namespace, Knip considers all exports referenced if
that namespace is used in certain patterns like enumeration. Individual exports
are then **not** reported.

**Solution**: if all exports on imported namespaces should be considered
individually, include the `nsExports` issue type to disable the heuristic.

See [namespace imports](../guides/namespace-imports) to see all related patterns.

### External libraries

[
Section titled “External libraries”](#external-libraries)

Are the exports consumed or imported by an external library, resulting in a
non-standard consumption of your exports? Here’s an example:

- [ index.js ](#tab-panel-12)
- [ components.js ](#tab-panel-13)

```

import loadable from '@loadable/component';

export const DynamicApple = dynamic(() =>

  import('./components.js').then(mod => mod.Apple)

);

export const LoadableOrange = loadable(() => import('./components.js'), {

  resolveComponent: components => components.Orange,

});

```

```

export const Apple = () => 'Apple';

export const Orange = () => 'Orange';

```

Knip understands `Apple` is used, since it’s standard usage. But `Orange` is
referenced through a function of an external library. For performance reasons,
Knip does not include external type definitions by default so it won’t see the
export being referenced.

**Solution**: include the type definitions of external libraries with the
[—include-libs](../reference/cli#--include-libs) flag:

Terminal window

```

knip --include-libs

```

This comes at a performance and memory penalty, but should give better results
if you need it. This flag is implied when [classMembers](#class-members) are included (that
feature comes with roughly the same performance penalty).

### Exclude exports from the report

[
Section titled “Exclude exports from the report”](#exclude-exports-from-the-report)

To exclude false positives from the report, there are a few options:

- [Ignore exports used in file](../reference/configuration#ignoreexportsusedinfile) for exports used internally.

- Individual exports can be [tagged using JSDoc syntax](../reference/jsdoc-tsdoc-tags).

- Have the export in an entry file:

Add the file to the `entry` file patterns array in the configuration.

- Move the export(s) to an entry file.

- Add the file to the `exports` field of `package.json`

- Re-export the unused export(s) from an entry file.

### Missing unused exports?

[
Section titled “Missing unused exports?”](#missing-unused-exports)

Did you expect certain exports in the report, but are they missing? They might
be exported from an entry file. In that case, use [—include-entry-exports](../reference/configuration#includeentryexports)
to make Knip also report unused exports in entry files.

The exports of non-standard extensions like `.astro`, `.mdx`, `.vue` or
`.svelte` are not available by default. See [compilers](../features/compilers) for more details on
how to include them.

### Class members

[
Section titled “Class members”](#class-members)

Unused members of exported classes are not reported by default, here’s how to
enable them:

Terminal window

```

knip --include classMembers

```

This option is also available in the Knip configuration file. Note that this
feature comes at a cost: linting will take more time and more memory.

Individual class members can be [tagged using JSDoc syntax](../reference/jsdoc-tsdoc-tags).

Classes exported from entry files are ignored, and so are their members. Use
[—include-entry-exports](../reference/configuration#includeentryexports) to make Knip also report members of unused exports
in entry files.

### Enum members

[
Section titled “Enum members”](#enum-members)

Unused enums and unused members of exported enums are reported by default.
Reporting such members can be disabled:

Terminal window

```

knip --exclude enumMembers

```

Individual enum members can be [tagged using JSDoc syntax](../reference/jsdoc-tsdoc-tags).

Enums exported from entry files are ignored, and so are their members. Use
[—include-entry-exports](../reference/configuration#includeentryexports) to make Knip also report members of unused exports
in entry files.

## Feedback or false positives?

[
Section titled “Feedback or false positives?”](#feedback-or-false-positives)

If you believe Knip incorrectly reports something as unused (i.e. there’s a
false positive), feel free to create a [minimal reproduction](../guides/issue-reproduction) and open an
issue on GitHub. It’ll make Knip better for everyone!

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/guides/handling-issues.mdx)

[
Previous
Troubleshooting ](/guides/troubleshooting) [
Next
Issue Reproduction ](/guides/issue-reproduction)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Issue Reproduction

**Source:** https://knip.dev/guides/issue-reproduction

---

# Issue Reproduction

If you encounter an issue or false positives when using Knip, you can [open an
issue on GitHub](https://github.com/webpro-nl/knip/issues?q=is%3Aissue). This will help you in your project, and will also improve
Knip for everyone else!

Think of Knip as a kitchen sink, it handles a large amount of projects and
configurations, and your project is different from all others. Many factors may
influence the issue at hand, such as:

- Code syntax, import and export structure in source files

- Dependencies, scripts and entry files in `package.json`

- TypeScript configuration in `tsconfig.json`

- Enabled plugins and related configuration files

- Dependent or depending workspaces in a monorepo

- Knip configuration in `knip.json`

Create the minimum of source code and configuration with a few files to
reproduce and demonstrate the issue. Having this as a basis has many benefits:

- Minimize barriers and unrelated contextual overhead

- Optimize shared understanding of the situation

- Serves as a “contract” to fix the actual issue

- Files serve as a fixture to the project

Providing this with an issue description will help us help you and improve the
chances the issue can be looked into efficiently and in a timely manner.

## Before opening an issue

[
Section titled “Before opening an issue”](#before-opening-an-issue)

Before opening an issue, please make sure you:

- are using the latest version

- have read the relevant documentation

- have searched [existing issues](https://github.com/webpro-nl/knip/issues?q=is%3Aissue)

- have checked the list of [known issues](https://knip.dev/reference/known-issues)

Please file only a single issue at a time, so each of them can be labeled and
tracked separately.

## Templates

[
Section titled “Templates”](#templates)

A convenient way to create a minimal reproduction is by starting with one of
these templates in CodeSandbox or StackBlitz:

TemplateBasic[CodeSandbox](https://codesandbox.io/p/devbox/github/webpro-nl/knip/main/templates/issue-reproduction/basic)[StackBlitz](https://stackblitz.com/github/webpro-nl/knip/tree/main/templates/issue-reproduction/basic)Monorepo[CodeSandbox](https://codesandbox.io/p/devbox/github/webpro-nl/knip/main/templates/issue-reproduction/monorepo)[StackBlitz](https://stackblitz.com/github/webpro-nl/knip/tree/main/templates/issue-reproduction/monorepo)

Shoutout to [CodeSandbox](https://codesandbox.io) and [StackBlitz](https://stackblitz.com) for generously providing these
free dev containers!

## Alternatives

[
Section titled “Alternatives”](#alternatives)

Other solutions to share a minimal and reproducible case may work well too,
including:

- A public repository on e.g. GitHub or GitLab.

- A new [fixtures folder in the Knip repository](https://github.com/webpro-nl/knip/tree/main/packages/knip/fixtures).

The goal is to have an easy and common understanding and reproduction. A link to
your existing project repository will likely not be considered “minimal”. Issues
containing just a screenshot, or snippets of output or source code don’t provide
the full picture and aren’t complete nor actionable.

If you’re unable to create a reproduction using one of the methods described
then please clearly explain this in the issue or [contact me](https://github.com/webpro).

## Pull Request

[
Section titled “Pull Request”](#pull-request)

The optimal way is to add fixtures and failing tests to the Knip repository, and
open a pull request to discuss the issue! Also see [instructions for
development](https://github.com/webpro-nl/knip/blob/main/.github/DEVELOPMENT.md).

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/guides/issue-reproduction.md)

[
Previous
Handling Issues ](/guides/handling-issues) [
Next
Contributing to Knip ](/guides/contributing)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Namespace Imports

**Source:** https://knip.dev/guides/namespace-imports

---

# Namespace Imports

The intention of exports used through namespace imports may not always be clear
to Knip. Here’s a guide to better understand how Knip handles such exports.

## Example

[
Section titled “Example”](#example)

We start off by having two exports:

- my-namespace.js

```

export const version = 'v5';

export const getRocket = () => '🚀';

```

The next snippet shows how to import all the exports above on a namespace. All
exports of the `my-namespace.js` module will be members on the `NS` object:

my-module.ts

```

import * as NS from './my-namespace.js';

import send from 'stats';

send(NS);

```

The intention of export usage is not always clear. In the example above is
`version` or `getRocket` used? We’re not sure, but we _probably_ don’t want them
to be reported as unused. The same goes for the next example:

my-module.ts

```

import * as NS from './my-namespace.js';

export { NS };

```

If this all usage of the `NS` namespace object, we also don’t know whether
individual exports like `version` or `getRocket` will be used. However, if at
least one reference to a property such as `NS.version` is found, then the
individual exports are considered separately again and `getRocket` will be
marked as unused:

index.ts

```

import { NS } from './my-module.js';

const version = NS.version;

```

## The default heuristic

[
Section titled “The default heuristic”](#the-default-heuristic)

Knip uses the following heuristic to determine which of the individual exports
are used:

If there’s one or more references to the import namespace object, but without
any property access, all exports on that namespace are considered used.

- Otherwise, exports are considered separately.

Below are a few more examples, and a way to disable this default behavior.

## Examples

[
Section titled “Examples”](#examples)

Let’s take a look at more examples:

my-namespace.ts

```

export const start = 1;

export const end = 1;

```

In the following cases all exports of `my-namespace.ts` are considered used:

index.ts

```

import * as NS from './my-namespace.js';

import send from 'stats';

send(NS);

const spread = { ...NS };

const shorthand = { NS };

const assignment = NS;

const item = [NS];

type TypeOf = typeof NS;

Object.values(NS);

for (const fruit in Fruits) {

  //

}

export { NS };

export { NS as AliasedNS };

export = NS;

```

However, this is no longer the case when one of the properties is accessed:

index.js

```

import * as NS from './namespace.js';

const begin = NS.start;

send(NS);

```

In this case, the `end` export will be reported as unused, even though the `NS`
object itself is referenced on its own as well.

## Include `nsExports` and `nsTypes`

[
Section titled “Include nsExports and nsTypes”](#include-nsexports-and-nstypes)

To disable the heuristic as explained above, and enforce Knip to consider each
export on a namespace individually, include the `nsExports` issue type:

```

{

  "include": ["nsExports"]

}

```

Or use the `--include nsExports` argument from the CLI. The `nsTypes` can be
added as well to do the same for exported types.

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/guides/namespace-imports.md)

[
Previous
Contributing to Knip ](/guides/contributing) [
Next
Performance ](/guides/performance)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Performance

**Source:** https://knip.dev/guides/performance

---

# Performance

This page describes a few topics around Knip’s performance, and how you might
improve it.

Knip does not want to tell you how to structure files or how to write your code,
but it might still be good to understand inefficient patterns for Knip.

Use the `--debug` and `--performance` flags to find potential bottlenecks.

## Cache

[
Section titled “Cache”](#cache)

Use `--cache` to speed up consecutive runs.

## Ignoring files

[
Section titled “Ignoring files”](#ignoring-files)

Files matching the `ignore` patterns are not excluded from the analysis. They’re
just not printed in the report. Use negated `entry` and `project` patterns to
exclude files from the analysis.

Read [configuring project files](./configuring-project-files) for details and examples. Improving
configuration may have a significant impact on performance.

## Workspace sharing

[
Section titled “Workspace sharing”](#workspace-sharing)

Knip shares files from separate workspaces if the configuration in
`tsconfig.json` allows this. This aims to reduce memory consumption and run
duration. Relevant compiler options include `baseUrl`, `paths` and
`moduleResolution`.

With the `--debug` flag you can see how many programs Knip uses. Look for
messages like this:

Terminal window

```

...

[*] Installed 2 programs for 29 workspaces

...

[*] Analyzing used resolved files [P1/1] (123)

...

[*] Analyzing used resolved files [P1/2] (8)

...

[*] Analyzing used resolved files [P2/1] (41)

...

```

The first number in `P1/1` is the number of the programs, the second number
indicates additional entry files were found so it does another round of analysis
on those files.

Use [—isolate-workspaces](../reference/cli#--isolate-workspaces) to disable this behavior. This is usually not
necessary, but more of an escape hatch in cases with memory usage issues or
incompatible `compilerOptions` across workspaces. Workspaces are analyzed
sequentially to spread out memory usage more evenly, which may prevent crashes
on large monorepos.

## Language Service

[
Section titled “Language Service”](#language-service)

Knip does not install the TypeScript Language Service (LS) by default. This is
expensive, as TypeScript needs to set up symbols and caching for the rather slow
`findReferences` function.

There are two cases that enforce Knip to install the LS.

### 1. Class members

[
Section titled “1. Class members”](#1-class-members)

The `findReferences` function is used to find unused members of imported classes
(i.e. when the issue type `classMembers` is included).

### 2. Include external type definitions

[
Section titled “2. Include external type definitions”](#2-include-external-type-definitions)

When [`--include-libs`](../guides/handling-issues#external-libraries) is enabled, Knip enables loading type definitions of
external dependencies. This will also install the LS to access its
`findReferences` function. It acts as an extra line of defense: only exports
that weren’t referenced to during default procedure go through this.

## Metrics

[
Section titled “Metrics”](#metrics)

Use [the `--performance` flag](../reference/cli#--performance) to see how many times potentially expensive
functions (e.g. `findReferences`) are invoked and how much time is spent in
those functions. Example usage:

Terminal window

```

knip --include classMembers --performance

```

## A last resort

[
Section titled “A last resort”](#a-last-resort)

In case Knip is unbearably slow (or even crashes), you could resort to [lint
individual workspaces](../features/monorepos-and-workspaces#lint-a-single-workspace).

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/guides/performance.md)

[
Previous
Namespace Imports ](/guides/namespace-imports) [
Next
Using Knip in CI ](/guides/using-knip-in-ci)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Troubleshooting

**Source:** https://knip.dev/guides/troubleshooting

---

# Troubleshooting

We can distinguish two types of issues:

- [Lint issues reported by Knip](#lint-issues-reported-by-knip)

- [Exceptions thrown by Knip](#exceptions-thrown-by-knip)

Also see the [debug](#debug) and [trace](#trace) options below that can help to
troubleshoot issues.

Note

The JavaScript/TypeScript ecosystem has a vast amount of frameworks and tools,
and even more ways to configure those. Files and dependencies can be referenced
in many ways, not just through static import statements. In short: “it’s
complicated”. Knip and documentation are always a work in progress.

If it doesn’t come your way at the first try, please understand this also shows
the dynamic and innovative nature of the ecosystem. Often only small changes go
a long way towards success. When a bit of configuration doesn’t improve things,
consider [opening an issue](https://github.com/webpro-nl/knip/issues/new/choose).

## Lint issues reported by Knip

[
Section titled “Lint issues reported by Knip”](#lint-issues-reported-by-knip)

Knip reports lint issues in your codebase. See [handling issues](../guides/handling-issues) to deal with
the reported issues.

If Knip reports false positives and you’re considering filing a GitHub issue,
please do! It’ll make Knip better for everyone. Please read [issue
reproduction](./issue-reproduction) first.

Exit code 1 indicates a successful run, but lint issues were found.

## Exceptions thrown by Knip

[
Section titled “Exceptions thrown by Knip”](#exceptions-thrown-by-knip)

Knip may throw an exception, resulting in an unsuccessful run.

See [known issues](../reference/known-issues) as it may be listed there and a workaround may be
available. If it isn’t clear what’s throwing the exception, try another run with
`--debug` to locate the cause of the issue with more details.

If Knip throws an exception and you’re considering filing a GitHub issue, please
do! It’ll make Knip better for everyone. Please read [issue reproduction](./issue-reproduction)
first.

Exit code 2 indicates an exception was thrown by Knip.

## Debug

[
Section titled “Debug”](#debug)

To better understand why Knip reports what it does, run it in debug mode by
adding `--debug` to the command:

- Terminal window

```

knip --debug

```

This will give a lengthy output, including:

Included workspaces

- Used configuration per workspace

- Enabled plugins per workspace

- Glob patterns and options followed by matching file paths

- Plugin config file paths and found dependencies per plugin

- Compiled non-standard source files

## Trace

[
Section titled “Trace”](#trace)

Use `--trace` to see where all exports are used. Or be more specific:

- Use `--trace-file [path]` to output this only for the given file.

- Use `--trace-export [name]` to output this only for the given export name.

- Use both to trace a specific named or default export of a certain file.

This works across re-exports, barrel files and workspaces. Here’s an example
screenshot:

It’s like a reversed module graph. Instead of traversing imports it goes in the
opposite direction and shows where exports are imported.

#### Legend

[
Section titled “Legend”](#legend)

Description`✓`Contains import and reference to the export`x`Is not imported`◯`Entry file

## Opening an issue

[
Section titled “Opening an issue”](#opening-an-issue)

If you want to open an issue, please see [issue reproduction](./issue-reproduction).

## Understanding Knip

[
Section titled “Understanding Knip”](#understanding-knip)

Looking to better understand how Knip works? The [entry files](../explanations/entry-files) and
[plugins](../explanations/plugins) explanations cover two core concepts. After this you might want to
check out features like [production mode](../features/production-mode) and [monorepos & workspaces](../features/monorepos-and-workspaces).

In a more general sense, [Why use Knip?](../explanations/why-use-knip) explains what Knip can do for you.

## Asking for help

[
Section titled “Asking for help”](#asking-for-help)

If you can’t find your answer in any of the aforementioned resources, feel free
to [open an issue on GitHub](https://github.com/webpro-nl/knip/issues).

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/guides/troubleshooting.md)

[
Previous
Configuring Project Files ](/guides/configuring-project-files) [
Next
Handling Issues ](/guides/handling-issues)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Using Knip in CI

**Source:** https://knip.dev/guides/using-knip-in-ci

---

# Using Knip in CI

Knip is your companion during local development. But it is even more valuable in
a continuous integration (CI) environment to prevent regressions over time. Knip
will notify you of unused dependencies, exports and files if you forgot to
remove them.

Knip will exit the process with code `1` if there are one or more issues.

## GitHub Actions

[
Section titled “GitHub Actions”](#github-actions)

Here’s an example workflow configuration for GitHub Actions:

-

```

name: Lint project

on: push

jobs:

  lint:

    runs-on: ubuntu-latest

    name: Ubuntu/Node v20

    steps:

      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4

      - name: Install dependencies

        run: npm install --ignore-scripts

      - name: Run knip

        run: npm run knip

```

## Notes

[
Section titled “Notes”](#notes)

In CI environments, the [—no-progress](../reference/cli#--no-progress) flag is set automatically.

## Related features

[
Section titled “Related features”](#related-features)

[—cache](../reference/cli#--cache)

- [—max-issues](../reference/cli#--max-issues)

- [—no-exit-code](../reference/cli#--no-exit-code)

- [—reporter](../reference/cli#--reporter-reporter)

## Related reading

[
Section titled “Related reading”](#related-reading)

- [Why use Knip?](../explanations/why-use-knip)

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/guides/using-knip-in-ci.md)

[
Previous
Performance ](/guides/performance) [
Next
Working with CommonJS ](/guides/working-with-commonjs)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Working with CommonJS

**Source:** https://knip.dev/guides/working-with-commonjs

---

# Working with CommonJS

CommonJS is the JavaScript module system using `require()` and `module.exports`
statements.

Knip works well with CommonJS. You don’t need to use ES Modules or a
`tsconfig.json` to use Knip.

The dynamic nature of CommonJS leaves room for ambiguity: it’s sometimes unclear
whether an export is a default or a named export (and thus how it should be
imported). So we’ll have to agree on a few conventions to prevent false
positives. Those conventions are designed to minimize impact on existing
codebases, improve consistency, and ease migration to ES Modules or TypeScript.

For **named exports**, the recommendation is to assign keys to `module.exports`:

```

const B = function () {};

module.exports.A = { option: true };

module.exports.B = B;

```

Alternatively, assign an object with ONLY shorthand property assignments to
`module.exports`:

```

const A = function () {};

const B = { option: true };

module.exports = { A, B };

```

Anything else assigned to `module.exports` is considered a default export, and
should be imported as such.

The following **default import** of the **named exports** above will result in
all those exports reported as unused, even when referenced like below:

```

const DefaultImport = require('./common.js');

const runtime = [DefaultImport.A, DefaultImport.B];

```

Instead, do this:

```

const { A, B } = require('./common.js');

const runtime = [A, B];

```

Not recommended per se, but the following import syntax also results in the
named export `A` being used:

```

const runtime = [require('./common.js').A];

```

Add a non-shorthand property to turn the named object notation into a single
**default export**:

```

const A = function () {};

const B = { option: true };

module.exports = { __esModule: true, A, B };

```

The `__esModule` key could be named differently (but makes sense given it’s an
informal “CJS/ESM interop” standard amongst compilers and bundlers).

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/guides/working-with-commonjs.md)

[
Previous
Using Knip in CI ](/guides/using-knip-in-ci) [
Next
Writing A Plugin ](/writing-a-plugin)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Configuration

**Source:** https://knip.dev/overview/configuration

---

# Configuration

## Defaults

[
Section titled “Defaults”](#defaults)

Knip has good defaults and aims for “zero config”. Here’s a simplified version
of the default configuration:

-

```

{

  "entry": ["index.{js,ts}", "src/index.{js,ts}"],

  "project": ["**/*.{js,ts}"]

}

```

Entry files are the starting point for Knip to find more source files and
external dependencies.

Tip

Run Knip without configuration. If it reports false positives, you need a
configuration file. Then read [configuring project files](../guides/configuring-project-files) to avoid overusing
`ignore` patterns.

## Location

[
Section titled “Location”](#location)

By default, Knip will look for a configuration file with the following names:

`knip.json`

- `knip.jsonc`

- `.knip.json`

- `.knip.jsonc`

- `knip.ts`

- `knip.js`

- `knip.config.ts`

- `knip.config.js`

- `package.json` (in the `"knip"` property)

If you want to use a custom file name or path, use the `--config` flag:

Terminal window

```

knip --config path/to/knip.json

```

## Customize

[
Section titled “Customize”](#customize)

Your project structure may not match the default `entry` and `project` files.
Here’s an example custom configuration to include `.js` files in the `scripts`
folder:

knip.json

```

{

  "$schema": "https://unpkg.com/knip@5/schema.json",

  "entry": ["src/index.ts", "scripts/{build,create}.js"],

  "project": ["src/**/*.ts", "scripts/**/*.js"]

}

```

If you override the `entry` file patterns, you may also want to override
`project` file patterns. The set of project files is used to determine what
files are unused. The `project` patterns can also be negated to exclude files
from the analysis. See [configuring project files](../guides/configuring-project-files) for details.

The values you set override the default values, they are not merged.

Tip

Be specific with `entry` files. Minimize the number of entry files and wildcards
for better results.

Plugins set entry files for you, such as those for Next.js, Remix, Vitest, and
many more.

Knip looks in many places for entry files. Learn more about this in the next
page about [entry files](../explanations/entry-files).

## Configuration Options

[
Section titled “Configuration Options”](#configuration-options)

See [configuration file options](../reference/configuration).

To use JavaScript or TypeScript in the configuration file, see [dynamic
configuration](../reference/dynamic-configuration).

## What’s next?

[
Section titled “What’s next?”](#whats-next)

The best way to understand Knip and what it can do for you is to read the pages
in the “Understanding Knip” sections, starting with [entry files](../explanations/entry-files).

Want to learn more about some of the main features?

- Working with [monorepos & workspaces](../features/monorepos-and-workspaces).

- Learn more about [production mode](../features/production-mode).

Having troubles configuring Knip?

- [Configuring project files](../guides/configuring-project-files)

- [Handling issues](../guides/handling-issues)

Search this website using the bar at the top (`Ctrl+K` or `⌘+K`).

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/overview/configuration.md)

[
Previous
Getting Started ](/overview/getting-started) [
Next
Features ](/overview/features)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Features

**Source:** https://knip.dev/overview/features

---

# Features

Overview of capabilities in support of the core feature: find many [types of
issues](../reference/issue-types).

Also see [related tooling](../reference/related-tooling).

## Overview

[
Section titled “Overview”](#overview)

NameDescription or example[Auto-fix](../features/auto-fix)Use `--fix` to auto-fix issues[Cache](../reference/cli#--cache)Use `--cache` to speed up consecutive runs[CommonJS](../guides/working-with-commonjs)Traditional JavaScript is just fine[Compilers](../features/compilers)Support for Astro, MDX, Svelte, Vue and custom compilers[Debug](../guides/troubleshooting#issues-reported-by-knip)Use `--debug` for troubleshooting[Filters](../features/rules-and-filters#filters)Exclude or focus on specific issue types[Format](../features/auto-fix#format)Add `--format` to `--fix` and auto-format modified files[JSDoc tags](../reference/jsdoc-tsdoc-tags)Exclude specific exports from the report[Memory usage](../reference/cli#--memory)Use `--memory` for detailed memory usage insights[Monorepos](../features/monorepos-and-workspaces)Workspaces are first-class citizen[Performance](../reference/cli#--performance)Use `--performance` for detailed timing insights[Plugins](../explanations/plugins)Over 100 plugins with custom entry paths and config parsing[Preprocessors](../features/reporters#preprocessors)Preprocess issues before being reported[Production mode](../features/production-mode)Use `--production` to lint only production code[Reporters](../features/reporters)Choose from many built-in reporters or use your own[Rules](../features/rules-and-filters#rules)Exclude or focus on specific issue types[Script parser](../features/script-parser)Shell scripts and `package.json` contain entry paths and dependencies[Trace](../guides/troubleshooting#trace)Trace exports to find where they are used[Watch mode](../reference/cli#--watch)Use `--watch` for live updates of unused files and exports[Workspace](../features/monorepos-and-workspaces#lint-a-single-workspace)Use `--workspace` to lint a single workspace in a monorepo

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/overview/features.md)

[
Previous
Configuration ](/overview/configuration) [
Next
Screenshots & videos ](/overview/screenshots-videos)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Getting Started

**Source:** https://knip.dev/overview/getting-started

---

# Getting Started

## Requirements

[
Section titled “Requirements”](#requirements)

Knip v5 requires Node.js v18.18.0 or higher. Or Bun.

Want to try Knip without installation? Visit [the playground](/playground).

## Installation

[
Section titled “Installation”](#installation)

This is the easiest and recommended way to install Knip:

- [ npm ](#tab-panel-25)
- [ pnpm ](#tab-panel-26)
- [ bun ](#tab-panel-27)
- [ yarn ](#tab-panel-28)

- Terminal window

```

npm init @knip/config

```

Terminal window

```

pnpm create @knip/config

```

Terminal window

```

bun create @knip/config

```

Terminal window

```

yarn create @knip/config

```

Now you can run Knip to lint your project:

[ npm ](#tab-panel-29)

- [ pnpm ](#tab-panel-30)
- [ bun ](#tab-panel-31)
- [ yarn ](#tab-panel-32)

Terminal window

```

npm run knip

```

Terminal window

```

pnpm knip

```

Terminal window

```

bun knip

```

Terminal window

```

yarn knip

```

Knip will lint your project and report unused dependencies, exports and files.

You can skip the rest of this page and go to [configuration](./configuration).

## Manual

[
Section titled “Manual”](#manual)

Alternatively, manually install Knip using your package manager:

- [ npm ](#tab-panel-33)
- [ pnpm ](#tab-panel-34)
- [ bun ](#tab-panel-35)
- [ yarn ](#tab-panel-36)

Terminal window

```

npm install -D knip typescript @types/node

```

Terminal window

```

pnpm add -D knip typescript @types/node

```

Terminal window

```

bun add -D knip typescript @types/node

```

Terminal window

```

yarn add -D knip typescript @types/node

```

Knip uses `typescript` and ` @types/node` as peer dependencies to increase
compatibility with your project. No worries, they’re probably in your
`node_modules` already.

Then add a `knip` script to your `package.json`:

package.json

```

{

  "name": "my-project",

  "scripts": {

    "knip": "knip"

  }

}

```

## Without installation

[
Section titled “Without installation”](#without-installation)

To run Knip without adding it to your project:

- [ npm ](#tab-panel-37)
- [ pnpm ](#tab-panel-38)
- [ bun ](#tab-panel-39)

Terminal window

```

npx knip

```

Terminal window

```

pnpm dlx knip

```

Terminal window

```

bunx knip

```

In this scenario `typescript` and `@types/node` are expected to be installed
already.

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/overview/getting-started.mdx)

[
Next
Configuration ](/overview/configuration)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Screenshots & videos

**Source:** https://knip.dev/overview/screenshots-videos

---

# Screenshots & videos

## Watch & auto-fix

[
Section titled “Watch & auto-fix”](#watch--auto-fix)

This video demonstrates using the `--watch` and `--fix` options inside Visual
Studio Code:

This works in any terminal. See [—watch](../reference/cli#--watch) and [auto-fix](../features/auto-fix) for more details.

## Trace

[
Section titled “Trace”](#trace)

Here’s an example screenshot that traces the `mapIterator` export in the
TypeScript codebase:

See [Trace](../guides/troubleshooting#trace) for more details.

## Performance

[
Section titled “Performance”](#performance)

An example screenshot showing `--performance` output for the Knip codebase:

Also see [—performance](../reference/cli#--performance).

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/overview/screenshots-videos.md)

[
Previous
Features ](/overview/features) [
Next
Entry Files ](/explanations/entry-files)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# CLI Arguments

**Source:** https://knip.dev/reference/cli

---

# CLI Arguments

## General

[
Section titled “General”](#general)

### `--help`

[
Section titled “--help”](#--help)

Shortcut: `-h`

Prints a summary of this page.

### `--version`

[
Section titled “--version”](#--version)

Shortcut: `-V`

Print the version number.

### `--no-progress`

[
Section titled “--no-progress”](#--no-progress)

Shortcut: `-n`

Don’t show dynamic progress updates. Progress is automatically disabled in CI
environments.

### `knip-bun`

[
Section titled “knip-bun”](#knip-bun)

Run Knip using the Bun runtime (instead of Node.js + jiti).

- Terminal window

```

knip-bun

```

This is equal to `bunx --bun knip`

Requires [Bun](https://bun.sh) to be installed. Also see [known issues](../reference/known-issues) for the type of
issues this might help with.

### NO_COLOR

[
Section titled “NO_COLOR”](#no_color)

The default reporters use the [NO_COLOR](https://no-color.org/) friendly [picocolors](https://www.npmjs.com/package/picocolors):

Terminal window

```

NO_COLOR=1 knip

```

## Troubleshooting

[
Section titled “Troubleshooting”](#troubleshooting)

### `--debug`

[
Section titled “--debug”](#--debug)

Shortcut: `-d`

Show debug output.

### `--memory`

[
Section titled “--memory”](#--memory)

Terminal window

```

knip --memory

(results)

heapUsed  heapTotal  freemem

--------  ---------  -------

   42.09      70.91  2251.00

  927.04    1042.58  1166.47

  973.29    1047.33  1160.92

  971.54    1079.83  1121.66

  997.80    1080.33  1120.34

 1001.88    1098.08  1100.72

 1038.69    1116.58  1100.72

 1082.12    1166.33  1100.72

 1145.46    1224.50  1100.72

 1115.82    1240.25  1100.72

 1182.35    1249.75   973.05

  637.32    1029.17   943.63

  674.30    1029.33   943.39

  682.24    1029.33   941.63

  707.70    1029.33   937.48

Total running time: 4.3s

```

Can be used with [—isolate-workspaces](#--isolate-workspaces) to see the difference in garbage
collection during the process.

### `--memory-realtime`

[
Section titled “--memory-realtime”](#--memory-realtime)

Use this if Knip crashes to still see memory usage info over time:

Terminal window

```

knip --memory-realtime

heapUsed  heapTotal  freemem

--------  ---------  -------

   42.09      70.91  2251.00

  927.04    1042.58  1166.47

...mem info keeps being logged...

(results)

```

### `--performance`

[
Section titled “--performance”](#--performance)

Use this flag to get the count and execution time of potentially expensive
functions in a table. Example:

Terminal window

```

$ knip --performance

(results)

Name                           size  min       max       median    sum

-----------------------------  ----  --------  --------  --------  --------

findReferences                  648     84.98   7698.61     96.41  70941.70

createProgram                     2   6295.84   7064.68   6680.26  13360.52

glob                              6      0.05    995.78    513.82   3150.87

findESLintDependencies            2      0.01     74.41     37.21     74.41

findGithubActionsDependencies     6      0.16     12.71      0.65     23.45

findBabelDependencies             2      0.00     38.75     19.37     38.75

...

Total running time: 5s

```

`name`: the internal Knip function name

- `size`: number of function invocations

- `min`: the fastest invocation

- `max`: the slowest invocation

- `median`: the median invocation

- `sum` the accumulated time of all invocations

This is not yet available in Bun, since it does not support
`performance.timerify` ([GitHub issue](https://github.com/oven-sh/bun/issues/9271)).

### `--performance-fn`

[
Section titled “--performance-fn”](#--performance-fn)

Limit the output of `--performance` to a single function to minimize the
overhead of the `timerify` Node.js built-in and focus on that function alone:

Terminal window

```

$ knip --performance-fn resolveSync

(results)

Name         size   min   max   median   sum

-----------  -----  ----  ----  ------  ------

resolveSync  66176  0.00  5.69    0.00  204.85

Total running time: 12.9s

```

### `--trace`

[
Section titled “--trace”](#--trace)

Trace exports to see where they are imported.

Also see [Trace](../guides/troubleshooting#trace).

### `--trace-export [name]`

[
Section titled “--trace-export [name]”](#--trace-export-name)

Trace export name to see where it’s imported. Implies [—trace](#--trace).

### `--trace-file [path]`

[
Section titled “--trace-file [path]”](#--trace-file-path)

Trace file to see where its exports are imported. Implies [—trace](#--trace).

## Configuration

[
Section titled “Configuration”](#configuration)

### `--config [file]`

[
Section titled “--config [file]”](#--config-file)

Use an alternative path for the configuration file. Default locations:

- `knip.json`

- `knip.jsonc`

- `.knip.json`

- `.knip.jsonc`

- `knip.js`

- `knip.ts`

- `package.json#knip`

Shortcut: `-c`

### `--tsConfig [file]`

[
Section titled “--tsConfig [file]”](#--tsconfig-file)

Shortcut: `-t`

Use an alternative path for the TypeScript configuration file.

Using `-t jsconfig.json` is also supported.

Default location: `tsconfig.json`

### `--workspace [dir]`

[
Section titled “--workspace [dir]”](#--workspace-dir)

[Lint a single workspace](../features/monorepos-and-workspaces#lint-a-single-workspace) including its ancestor and dependent workspaces.
The default behavior is to lint all configured workspaces.

Shortcut: `-W`

### `--directory [dir]`

[
Section titled “--directory [dir]”](#--directory-dir)

Default: `cwd` (current directory)

Run the process from a different directory.

### `--no-gitignore`

[
Section titled “--no-gitignore”](#--no-gitignore)

Ignore `.gitignore` files.

### `--include-entry-exports`

[
Section titled “--include-entry-exports”](#--include-entry-exports)

When a repository is self-contained or private, you may want to include entry
files when reporting unused exports:

Terminal window

```

knip --include-entry-exports

```

Also see [includeEntryExports](./configuration#includeentryexports).

### `--include-libs`

[
Section titled “--include-libs”](#--include-libs)

Getting false positives for exports consumed by external libraries? Try the
`--include-libs` flag:

Terminal window

```

knip --include-libs

```

Also see [external libs](../guides/handling-issues#external-libraries).

### `--isolate-workspaces`

[
Section titled “--isolate-workspaces”](#--isolate-workspaces)

By default, Knip optimizes performance using [workspace sharing](../guides/performance#workspace-sharing) to existing
TypeScript programs, based on the compatibility of their `compilerOptions`. This
flag disables this behavior and creates one program per workspace, which is
slower but memory usage is spread more evenly over time.

## Modes

[
Section titled “Modes”](#modes)

### `--production`

[
Section titled “--production”](#--production)

Lint only production source files. This excludes:

- entry files defined by plugins:

test files

- configuration files

- Storybook stories

- `devDependencies` from `package.json`

Read more at [Production Mode](../features/production-mode).

### `--strict`

[
Section titled “--strict”](#--strict)

Isolate workspaces and consider only direct dependencies. Implies [production
mode](#--production).

Read more at [Production Mode](../features/production-mode).

### `--fix`

[
Section titled “--fix”](#--fix)

Read more at [auto-fix](../features/auto-fix).

### `--cache`

[
Section titled “--cache”](#--cache)

Enable caching.

Consecutive runs are 10-40% faster as the results of file analysis (AST
traversal) are cached. Conservative. Cache strategy based on file meta data
(modification time + file size).

### `--cache-location`

[
Section titled “--cache-location”](#--cache-location)

Provide alternative cache location.

Default location: `./node_modules/.cache/knip`

### `--watch`

[
Section titled “--watch”](#--watch)

Watch current directory, and update reported issues when a file is modified,
added or deleted.

Watch mode focuses on imports and exports in source files. During watch mode,
changes in `package.json` or `node_modules` may not cause an updated report.

## Filters

[
Section titled “Filters”](#filters)

Available [issue types](./issue-types) when filtering output using `--include` or
`--exclude`:

- `files`

- `dependencies`

- `optionalPeerDependencies`

- `unlisted`

- `unresolved`

- `exports`

- `nsExports`

- `classMembers`

- `types`

- `nsTypes`

- `enumMembers`

- `duplicates`

### `--exclude`

[
Section titled “--exclude”](#--exclude)

Exclude provided issue types from report. Can be comma-separated or repeated.

Example:

Terminal window

```

knip --exclude classMembers,enumMembers

knip --exclude classMembers --exclude enumMembers

```

### `--include`

[
Section titled “--include”](#--include)

Report only provided issue types. Can be comma-separated or repeated.

Example:

Terminal window

```

knip --include files,dependencies

knip --include files --include dependencies

```

### `--dependencies`

[
Section titled “--dependencies”](#--dependencies)

Shortcut to include all types of dependency issues:

Terminal window

```

--include dependencies,optionalPeerDependencies,unlisted,binaries,unresolved

```

### `--exports`

[
Section titled “--exports”](#--exports)

Shortcut to include all types of export issues:

Terminal window

```

--include exports,nsExports,classMembers,types,nsTypes,enumMembers,duplicates

```

### `--experimental-tags`

[
Section titled “--experimental-tags”](#--experimental-tags)

Deprecated. Use [—tags](#--tags) instead.

### `--tags`

[
Section titled “--tags”](#--tags)

Exports can be tagged with known or arbitrary JSDoc/TSDoc tags:

```

/**

 * Description of my exported value

 *

 * @type number

 * @internal Important matters

 * @lintignore

 */

export const myExport = 1;

```

And then include (`+`) or exclude (`-`) these tagged exports from the report
like so:

Terminal window

```

knip --tags=-lintignore,-internal

knip --tags=+custom

```

This way, you can either focus on or ignore specific tagged exports with tags
you define yourself. This also works for individual class or enum members.

The default directive is `+` (include) and the `@` prefix is ignored, so the
notation below is valid and will report only exports tagged `@lintignore` or
`@internal`:

Terminal window

```

knip --tags @lintignore --tags @internal

```

## Reporters & Preprocessors

[
Section titled “Reporters & Preprocessors”](#reporters--preprocessors)

### `--reporter [reporter]`

[
Section titled “--reporter [reporter]”](#--reporter-reporter)

Available reporters:

- `symbols` (default)

- `compact`

- `codeowners`

- `json`

- `markdown`

Can be repeated. Example:

Terminal window

```

knip --reporter compact

```

Also see [Reporters & Preprocessors](../features/reporters).

### `--reporter-options [json]`

[
Section titled “--reporter-options [json]”](#--reporter-options-json)

Pass extra options to the preprocessor (as JSON string, see —reporter-options
example)

Example:

Terminal window

```

knip --reporter codeowners --reporter-options '{"path":".github/CODEOWNERS"}'

```

### `--preprocessor [preprocessor]`

[
Section titled “--preprocessor [preprocessor]”](#--preprocessor-preprocessor)

Preprocess the results before providing it to the reporters.

Can be repeated. Examples:

Terminal window

```

knip --preprocessor ./my-preprocessor.ts

```

Terminal window

```

knip --preprocessor preprocessor-package

```

### `--preprocessor-options [json]`

[
Section titled “--preprocessor-options [json]”](#--preprocessor-options-json)

Pass extra options to the preprocessor as JSON string.

Terminal window

```

knip --preprocessor ./preproc.ts --preprocessor-options '{"key":"value"}'

```

Also see [Reporters & Preprocessors](../features/reporters).

## Exit code

[
Section titled “Exit code”](#exit-code)

The default exit codes:

CodeDescription`0`Knip ran successfully, no lint issues`1`Knip ran successfully, but there is at least one lint issues`2`Knip did not run successfully due to bad input or internal error

### `--no-exit-code`

[
Section titled “--no-exit-code”](#--no-exit-code)

Always exit with code zero (`0`), even when there are lint issues.

### `--max-issues`

[
Section titled “--max-issues”](#--max-issues)

Maximum number of issues before non-zero exit code. Default: `0`

### `--no-config-hints`

[
Section titled “--no-config-hints”](#--no-config-hints)

Suppress configuration hints.

### `--treat-config-hints-as-errors`

[
Section titled “--treat-config-hints-as-errors”](#--treat-config-hints-as-errors)

Exit with non-zero code (`1`) if there are any configuration hints.

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/reference/cli.md)

[
Previous
Argument Parsing ](/writing-a-plugin/argument-parsing) [
Next
Configuration ](/reference/configuration)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Configuration

**Source:** https://knip.dev/reference/configuration

---

# Configuration

This page lists all configuration file options.

## File Types

[
Section titled “File Types”](#file-types)

### JSON Schema

[
Section titled “JSON Schema”](#json-schema)

A `$schema` field is a URL that you put at the top of your JSON file. This
allows you to get red squiggly lines inside of your IDE when you make a typo or
provide an otherwise invalid configuration option.

In JSON, use the provided JSON schema:

- knip.json

```

{

  "$schema": "https://unpkg.com/knip@5/schema.json"

}

```

### JSONC

[
Section titled “JSONC”](#jsonc)

In JSONC, use the provided JSONC schema:

knip.jsonc

```

{

  "$schema": "https://unpkg.com/knip@5/schema-jsonc.json"

}

```

Use JSONC if you want to use comments and/or trailing commas.

### TypeScript

[
Section titled “TypeScript”](#typescript)

See [dynamic configuration](../reference/dynamic-configuration) about dynamic and typed configuration files.

## Project

[
Section titled “Project”](#project)

### `entry`

[
Section titled “entry”](#entry)

Array of glob patterns to find entry files. Prefix with `!` for negation.
Example:

knip.json

```

{

  "entry": ["src/index.ts", "scripts/*.ts", "!scripts/except-this-one.ts"]

}

```

Also see [configuration](../overview/configuration) and [entry files](../explanations/entry-files).

### `project`

[
Section titled “project”](#project-1)

Array of glob patterns to find project files. Example:

knip.json

```

{

  "project": ["src/**/*.ts", "scripts/**/*.ts"]

}

```

Also see [configuration](../overview/configuration) and [entry files](../explanations/entry-files).

### `paths`

[
Section titled “paths”](#paths)

Tools like TypeScript, webpack and Babel support import aliases in various ways.
Knip automatically includes `compilerOptions.paths` from the TypeScript
configuration, but does not automatically use other types of import aliases.
They can be configured manually:

knip.json

```

{

  "paths": {

    "@lib": ["./lib/index.ts"],

    "@lib/*": ["./lib/*"]

  }

}

```

Each workspace can have its own `paths` configured. Knip `paths` follow the
TypeScript semantics:

Path values are an array of relative paths.

- Paths without an `*` are exact matches.

## Workspaces

[
Section titled “Workspaces”](#workspaces)

Individual workspace configurations may contain all other options listed on this
page, except for the following root-only options:

- `exclude` / `include`

- `ignoreExportsUsedInFile`

- `ignoreWorkspaces`

- `workspaces`

Workspaces can’t be nested in a Knip configuration, but they can be nested in a
monorepo folder structure.

Also see [Monorepos and workspaces](../features/monorepos-and-workspaces).

## Plugins

[
Section titled “Plugins”](#plugins)

There are a few options to modify the behavior of a plugin:

- Override a plugin’s `config` or `entry` location

- Force-enable a plugin by setting its value to `true`

- Disable a plugin by setting its value to `false`

knip.json

```

{

  "mocha": {

    "config": "config/mocha.config.js",

    "entry": ["**/*.spec.js"]

  },

  "playwright": true,

  "webpack": false

}

```

It should be rarely necessary to override the `entry` patterns, since plugins
also read custom entry file patterns from the tooling configuration (see
[Plugins → entry files](../explanations/plugins#entry-files)).

Plugin configuration can be set on root and on a per-workspace level. If enabled
on root level, it can be disabled on workspace level by setting it to `false`
there, and vice versa.

Also see [Plugins](../explanations/plugins).

## Rules & Filters

[
Section titled “Rules & Filters”](#rules--filters)

### `rules`

[
Section titled “rules”](#rules)

See [Rules & Filters](../features/rules-and-filters#filters).

### `include`

[
Section titled “include”](#include)

See [Rules & Filters](../features/rules-and-filters#filters).

### `exclude`

[
Section titled “exclude”](#exclude)

See [Rules & Filters](../features/rules-and-filters#filters).

### `tags`

[
Section titled “tags”](#tags)

Exports can be tagged with known or arbitrary JSDoc/TSDoc tags:

```

/**

 * Description of my exported value

 *

 * @type number

 * @internal Important matters

 * @lintignore

 */

export const myExport = 1;

```

And then include (`+`) or exclude (`-`) these tagged exports from the report
like so:

```

{

  "tags": ["-lintignore"]

}

```

This way, you can either focus on or ignore specific tagged exports with tags
you define yourself. This also works for individual class or enum members.

The default directive is `+` (include) and the `@` prefix is ignored, so the
notation below is valid and will report only exports tagged `@lintignore` or
`@internal`:

```

{

  "tags": ["@lintignore", "@internal"]

}

```

Also see [JSDoc & TSDoc Tags](./jsdoc-tsdoc-tags).

### `treatConfigHintsAsErrors`

[
Section titled “treatConfigHintsAsErrors”](#treatconfighintsaserrors)

Exit with non-zero code (1) if there are any configuration hints.

knip.json

```

{

  "treatConfigHintsAsErrors": true

}

```

## Ignore Issues

[
Section titled “Ignore Issues”](#ignore-issues)

### `ignore`

[
Section titled “ignore”](#ignore)

Tip

Please read [configuring project files](../guides/configuring-project-files) before using the `ignore` option,
because in most cases you’ll want to fine‑tune `entry` and `project` (or use
production mode) instead.

Array of glob patterns to ignore issues from matching files. Example:

knip.json

```

{

  "ignore": ["src/generated.ts", "fixtures/**"]

}

```

### `ignoreFiles`

[
Section titled “ignoreFiles”](#ignorefiles)

Array of glob patterns of files to exclude from the “Unused files” section only.

Unlike `ignore`, which suppresses all issue types for matching files,
`ignoreFiles` only affects the `files` issue type. Use this when a file should
remain analyzed for other issues (exports, dependencies, unresolved) but should
not be considered for unused file detection.

knip.json

```

{

  "ignoreFiles": ["src/generated/**", "fixtures/**"]

}

```

Suffix an item with `!` to enable it only in production mode.

### `ignoreBinaries`

[
Section titled “ignoreBinaries”](#ignorebinaries)

Exclude binaries that are used but not provided by any dependency from the
report. Value is an array of binary names or regular expressions. Example:

knip.json

```

{

  "ignoreBinaries": ["zip", "docker-compose", "pm2-.+"]

}

```

Actual regular expressions can be used in dynamic configurations:

knip.ts

```

export default {

  ignoreBinaries: [/^pm2-.+/],

};

```

Suffix an item with `!` to enable it only in production mode.

### `ignoreDependencies`

[
Section titled “ignoreDependencies”](#ignoredependencies)

Array of package names to exclude from the report. Regular expressions allowed.
Example:

knip.json

```

{

  "ignoreDependencies": ["hidden-package", "@org/.+"]

}

```

Actual regular expressions can be used in dynamic configurations:

knip.ts

```

export default {

  ignoreDependencies: [/@org\/.*/, /^lib-.+/],

};

```

Suffix an item with `!` to enable it only in production mode.

### `ignoreMembers`

[
Section titled “ignoreMembers”](#ignoremembers)

Array of class and enum members to exclude from the report. Regular expressions
allowed. Example:

knip.json

```

{

  "ignoreMembers": ["render", "on.+"]

}

```

Actual regular expressions can be used in dynamic configurations.

### `ignoreUnresolved`

[
Section titled “ignoreUnresolved”](#ignoreunresolved)

Array of specifiers to exclude from the report. Regular expressions allowed.
Example:

knip.json

```

{

  "ignoreUnresolved": ["ignore-unresolved-import", "#virtual/.+"]

}

```

Actual regular expressions can be used in dynamic configurations:

knip.ts

```

export default {

  ignoreUnresolved: [/^#/.+/],

};

```

### `ignoreWorkspaces`

[
Section titled “ignoreWorkspaces”](#ignoreworkspaces)

Array of workspaces to ignore, globs allowed. Example:

knip.json

```

{

  "ignoreWorkspaces": [

    "packages/go-server",

    "packages/flat/*",

    "packages/deep/**"

  ]

}

```

Suffix an item with `!` to enable it only in production mode.

### `ignoreIssues`

[
Section titled “ignoreIssues”](#ignoreissues)

Ignore specific issue types for specific file patterns. Keys are glob patterns
and values are arrays of issue types to ignore for matching files. This allows
ignoring specific issues (like unused exports) in generated files while still
reporting other issues in those same files.

knip.json

```

{

  "ignoreIssues": {

    "src/generated/**": ["exports", "types"],

    "**/*.generated.ts": ["exports", "classMembers"]

  }

}

```

## Exports

[
Section titled “Exports”](#exports)

### `ignoreExportsUsedInFile`

[
Section titled “ignoreExportsUsedInFile”](#ignoreexportsusedinfile)

In files with multiple exports, some of them might be used only internally. If
these exports should not be reported, there is a `ignoreExportsUsedInFile`
option available. With this option enabled, when something is also no longer
used internally, it will be reported as unused.

knip.json

```

{

  "ignoreExportsUsedInFile": true

}

```

In a more fine-grained manner, to ignore only specific issue types:

knip.json

```

{

  "ignoreExportsUsedInFile": {

    "interface": true,

    "type": true

  }

}

```

### `includeEntryExports`

[
Section titled “includeEntryExports”](#includeentryexports)

By default, Knip does not report unused exports in entry files. When a
repository (or workspace) is self-contained or private, you may want to include
entry files when reporting unused exports:

knip.json

```

{

  "includeEntryExports": true

}

```

If enabled, Knip will report unused exports in entry source files. But not in
entry and configuration files as configured by plugins, such as `next.config.js`
or `src/routes/+page.svelte`.

This will also enable reporting unused members of exported classes and enums.

Set this option at root level to enable this globally, or within workspace
configurations individually.

## Compilers

[
Section titled “Compilers”](#compilers)

Knip supports custom compilers to transform files before analysis.

Note

Since compilers are functions, they can only be used in dynamic configuration
files (`.js` or `.ts`), not in JSON configuration files.

### `compilers`

[
Section titled “compilers”](#compilers-1)

Override built-in compilers or add custom compilers for additional file types.

Also see [Compilers](../features/compilers).

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/reference/configuration.md)

[
Previous
CLI Arguments ](/reference/cli) [
Next
Dynamic Configuration ](/reference/dynamic-configuration)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Dynamic Configuration

**Source:** https://knip.dev/reference/dynamic-configuration

---

# Dynamic Configuration

## TypeScript

[
Section titled “TypeScript”](#typescript)

Instead of a JSON file, you can use a JavaScript or TypeScript file for a
dynamic configuration and type annotations:

- [ TypeScript ](#tab-panel-40)
- [ JavaScript ](#tab-panel-41)

- knip.ts

```

import type { KnipConfig } from 'knip';

const config: KnipConfig = {

  entry: ['src/index.ts'],

  project: ['src/**/*.ts'],

};

export default config;

```

knip.js

```

/** @type {import('knip').KnipConfig} */

const config = {

  entry: ['src/index.ts'],

  project: ['src/**/*.ts'],

};

export default config;

```

## Function

[
Section titled “Function”](#function)

You can export a regular or async function that returns the configuration object
as well:

[ TypeScript ](#tab-panel-42)

- [ JavaScript ](#tab-panel-43)

knip.ts

```

import type { KnipConfig } from 'knip';

const config = async (): Promise<KnipConfig> => {

  const items = await fetchRepoInfo();

  return {

    entry: ['src/index.ts', ...items],

    project: ['src/**/*.ts'],

  };

};

export default config;

```

knip.js

```

const config = async () => ({

  entry: ['src/index.ts'],

  project: ['src/**/*.ts'],

});

export default config;

```

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/reference/dynamic-configuration.mdx)

[
Previous
Configuration ](/reference/configuration) [
Next
FAQ ](/reference/faq)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# FAQ

**Source:** https://knip.dev/reference/faq

---

# FAQ

## Introduction

[
Section titled “Introduction”](#introduction)

Knip finds and fixes unused dependencies, exports and files. As a “kitchen sink”
in the npm ecosystem, it creates comprehensive module and dependency graphs of
your project.

Note

The JavaScript/TypeScript ecosystem has a vast amount of frameworks and tools,
and even more ways to configure those. Files and dependencies can be referenced
in many ways, not just through static import statements. In short: “it’s
complicated”. Knip and documentation are always a work in progress.

This FAQ is an attempt to provide some perspective on a few design decisions and
why certain things work the way they do. Here and there it’s intentionally a bit
more in-depth than the rest of the docs.

## Comparison

[
Section titled “Comparison”](#comparison)

### Why isn’t Knip an ESLint plugin?

[
Section titled “Why isn’t Knip an ESLint plugin?”](#why-isnt-knip-an-eslint-plugin)

Linters like ESLint analyze files separately, while Knip lints projects as a
whole.

Knip requires full module and dependency graphs to find clutter across the
project. Creating these comprehensive graphs is not a trivial task and it seems
no such tool exists today, even more so when it comes to monorepos.

File-oriented linters like ESLint are complementary to Knip.

### Isn’t tree-shaking enough?

[
Section titled “Isn’t tree-shaking enough?”](#isnt-tree-shaking-enough)

In short: no. They share an important goal: improve UX by removing unused code.
The main takeaway here is that tree-shaking and Knip are different and
complementary tools.

Tree-shaking is a build or compile-time activity to reduce production bundle
size. It typically operates on bundled production code, which might include
external/third-party code. An optimization in the build process, “out of your
hands”.

On the other hand, Knip is a project linter that should be part of the QA phase.
It lints, reports and fixes only your own source code. Moreover, in contrast
with other linters, focuses on inter-file dependencies, so dead code within a
file may not be caught by Knip.

Issues reported by the linter are then for you to handle (except for everything
that Knip [auto-fixes](../features/auto-fix) for you).

Besides those differences, Knip has a broader scope:

- Improve DX (see [less is more](../explanations/why-use-knip#less-is-more)).

- Include non-production code and dependencies in the process by default.

- Handle more [types of issues](./issue-types) (such as unlisted dependencies).

## Synergy

[
Section titled “Synergy”](#synergy)

### Why does Knip have plugins?

[
Section titled “Why does Knip have plugins?”](#why-does-knip-have-plugins)

Plugins are an essential part of Knip. They prevent you from a lot of
configuration out of the box, by adding entry files as accurately as possible
and only for the tools actually installed. Yet the real magic is in their custom
parsers for configuration files and command-line argument definitions.

For instance, Vitest has the `environment` configuration option. The Vitest
plugin knows `"node"` is the default value for `environment` which does not
require an extra package, but will translate `"edge-runtime"` to the
`@edge-runtime/vm` package. This allows Knip to report it if this package is not
listed in `package.json`, or when it is no longer used after changes in the
Vitest configuration.

Configuration files may also contain references to entry files. For instance,
Jest has `setupFilesAfterEnv: "<rootDir>/jest.setup.js"` or a reference may
point to a file in another workspace in the same monorepo, e.g.
`setupFiles: ['@org/shared/jest-setup.ts']`. Those entry files may also contain
imports of internal modules or external dependencies, and so on.

### Why is Knip so heavily engineered?

[
Section titled “Why is Knip so heavily engineered?”](#why-is-knip-so-heavily-engineered)

Even though a modular approach has its merits, for Knip it makes sense to have
all the pieces in a single tool.

Building up the module and dependency graphs requires non-standard module
resolution and not only static but also dynamic analysis (i.e. actually load and
execute modules), such as for parsers of plugins to receive the exported value
of dynamic tooling configuration files. Additionally, [exports consumed by
external libraries](../guides/handling-issues#external-libraries) require type information, as supported by the TypeScript
backend. Last but not least, shell script parsing is required to find the right
entry files, configuration files and dependencies accurately.

The rippling effect of plugins and recursively adding entry files and
dependencies to build up the graphs is also exactly what’s meant by
[“comprehensive” here](../explanations/why-use-knip#comprehensive).

## Building the graphs

[
Section titled “Building the graphs”](#building-the-graphs)

### Where does Knip look for entry files?

[
Section titled “Where does Knip look for entry files?”](#where-does-knip-look-for-entry-files)

- In default locations such as `index.js` and `src/index.ts`

- In `main`, `bin` and `exports` fields in `package.json`

- In the entry files as configured by enabled plugins

- In `config` files as configured and parsed by enabled plugins

- The `config` files themselves are entry files

- In dynamic imports (i.e. `require()` and `import()` calls)

- In `require.resolve('./entry.js')`

- In `import.meta.resolve('./entry.mjs')`

- Through scripts inside template strings in source files such as:

-

```

await $({ stdio: 'inherit' })`c8 node hydrate.js`; // execa

await $`node scripts/parse.js`; // bun/zx

```

Through scripts in `package.json` such as:

```

{

  "name": "my-lib",

  "scripts": {

    "start": "node --import tsx/esm run.ts",

    "start": "vitest -c config/vitest.config.ts"

  }

}

```

- Through plugins handling CI workflow files like `.github/workflows/ci.yml`:

```

jobs:

  test:

    steps:

      run: playwright test e2e/**/*.spec.ts --config playwright.e2e.config.ts

      run: node --import tsx/esm run.ts

```

Scripts like the ones shown here may also contain references to configuration
files (`config/vitest.config.ts` and `playwright.e2e.config.ts` in the examples
above). They’re recognized as configuration files and passed to their respective
plugins, and may contain additional entry files.

Entry files are added to the module graph. [Module resolution](#module-resolution) might result
in additional entry files recursively until no more entry files are found.

### What does Knip look for in source files?

[
Section titled “What does Knip look for in source files?”](#what-does-knip-look-for-in-source-files)

The TypeScript source file parser is powerful and fault-tolerant. Knip visits
all nodes of the generated AST to find:

- Imports and dynamic imports of internal modules and external dependencies

- Exports

- Accessed properties on namespace imports and re-exports to track individual
  export usage

- Calls to `require.resolve` and `import.meta.resolve`

- Scripts in template strings (passed to [script parser](../features/script-parser))

### What’s in the graphs?

[
Section titled “What’s in the graphs?”](#whats-in-the-graphs)

Once the module and dependency graphs are created, they contain the information
required to create the report including all issue types:

- Unused files

- Unused dependencies

- Unused devDependencies

- Referenced optional peerDependencies

- Unlisted dependencies

- Unlisted binaries

- Unresolved imports

- Unused exports

- Unused exported types

- Unused exported enum members

- Duplicate exports

And optionally more issue types like individual exports and exported types in
namespace imports, and unused class members.

The graphs allows to report more interesting details, such as:

- Circular references

- Usage numbers per export

- Export usage across workspaces in a monorepo

- List of all binaries used

- List of all used (OS) binaries not installed in `node_modules`

### Why doesn’t Knip just read the lockfile?

[
Section titled “Why doesn’t Knip just read the lockfile?”](#why-doesnt-knip-just-read-the-lockfile)

Knip reads the `package.json` file of each dependency. Most of the information
required is in the lockfile as well, which would be more efficient. However,
there are a few issues with this approach:

- It requires lockfile parsing for each lockfile format and version of each
  package manager.

- The lockfile doesn’t contain whether the package [has types included](../guides/handling-issues#types-packages).

## Module Resolution

[
Section titled “Module Resolution”](#module-resolution)

### Why doesn’t Knip use an existing module resolver?

[
Section titled “Why doesn’t Knip use an existing module resolver?”](#why-doesnt-knip-use-an-existing-module-resolver)

Runtimes like Node.js provide `require.resolve` and `import.meta.resolve`.
TypeScript comes with module resolution built-in. More module resolvers are out
there and bundlers are known to use or come with module resolvers. None of them
seem to meet all requirements to be usable on its own by Knip:

- Support non-standard extensions like `.css`, `.svelte` and `.png`

- Support path aliases

- Support `exports` map in `package.json`

- Support self-referencing imports

- Rewire `package.json#main` build artifacts like `dist/module.js` to its source
  at `src/module.ts`

- Don’t resolve to type definition paths like `module.d.ts` but source code at
  `module.js`

A few strategies have been tried and tweaked, and Knip currently uses a
combination of [oxc-resolver](https://oxc.rs/docs/guide/usage/resolver.html), the TypeScript module resolver and a few
customizations. This single custom module resolver function is hooked into the
TypeScript compiler and language service hosts.

Everything else is handled by `oxc-resolver` for things like [script parsing](../features/script-parser)
and resolving references to files in other workspaces.

### How does Knip handle non-standard import syntax?

[
Section titled “How does Knip handle non-standard import syntax?”](#how-does-knip-handle-non-standard-import-syntax)

Knip tries to be resilient against import syntax like what’s used by e.g.
webpack loaders or Vite asset imports. Knip strips off the prefixes and suffixes
in import specifiers like this:

component.ts

```

import Icon from './icon.svg?raw';

import Styles from '-!style-loader!css-loader?modules!./styles.css';

```

In this example, the `style-loader` and `css-loader` dependencies should be
dependencies found in webpack configuration, handled by Knip’s webpack plugin.

## TypeScript

[
Section titled “TypeScript”](#typescript)

### What’s the difference between workspaces, projects and programs?

[
Section titled “What’s the difference between workspaces, projects and programs?”](#whats-the-difference-between-workspaces-projects-and-programs)

A workspace is a directory with a `package.json` file. They’re configured in
`package.json#workspaces` (or `pnpm-workspaces.yml`). In case a directory has a
`package.json` file, but is not a workspace (from a package manager
perspective), it can be added as a workspace to the Knip configuration.

Projects - in the context of TypeScript - are directories with a `tsconfig.json`
file. They’re not a concept in Knip.

A TypeScript program has a 1-to-1 relationship with workspaces if they’re
analyzed in isolation. However, by default Knip optimizes for performance and
utilizes [workspace sharing](../guides/performance#workspace-sharing). That’s why debug output contains messages like
“Installed 2 programs for 29 workspaces”.

### Why doesn’t Knip match my TypeScript project structure?

[
Section titled “Why doesn’t Knip match my TypeScript project structure?”](#why-doesnt-knip-match-my-typescript-project-structure)

Repositories and workspaces in a monorepo aren’t necessarily structured like
TypeScript projects. Put simply, the location of `package.json` files isn’t
always adjacent to `tsconfig.json` files. Knip follows the structure of
workspaces in a monorepo.

An additional layering of TypeScript projects would complicate things. The
downside is that a `tsconfig.json` file not used by Knip may have conflicting
module resolution settings, potentially resulting in missed files.

In practice, this is rarely an issue. Knip sticks to the workspaces structure
and installs a single “kitchen sink” module resolver function per workspace.
Different strategies might add more complexity and performance penalties, while
the current strategy is simple, fast and good enough.

Note that any directory with a `package.json` not listed in the root
`package.json#workspaces` can be added to the Knip configuration manually to
have it handled as a separate workspace.

### Why doesn’t Knip analyze workspaces in isolation by default?

[
Section titled “Why doesn’t Knip analyze workspaces in isolation by default?”](#why-doesnt-knip-analyze-workspaces-in-isolation-by-default)

Knip creates TypeScript programs to create a module graph and traverse file
ASTs. In a monorepo, it would make a lot of sense to create one program per
workspace. However, this slows down the whole process considerably. That’s why
Knip shares the files of multiple workspaces in a single program if their
configuration allows it. This optimization is enabled by default, while it also
allows the module resolver (one per program) to do some more caching.

Also see [workspace sharing](../guides/performance#workspace-sharing).

### Why doesn’t Knip just use `ts.findReferences`?

[
Section titled “Why doesn’t Knip just use ts.findReferences?”](#why-doesnt-knip-just-use-tsfindreferences)

TypeScript has a very good “Find references” feature, that you might be using in
your IDE as well. Yet at scale this becomes too slow. That’s why Knip builds up
its own module graph to look up export usages. Additional benefits for this
comprehensive graph include:

- serializable and cacheable

- enables more features

- usable for other tools to build upon as well

Without sacrificing these benefits, Knip does use `ts.findReferences` to find
references to class members (i.e. when the issue type `classMembers` is
included). In case analysis of exports requires type information of external
dependencies, the [`--include-libs ` flag](../guides/handling-issues#external-libraries) will trigger the same.

### Why can’t I use path aliases to reference other workspaces?

[
Section titled “Why can’t I use path aliases to reference other workspaces?”](#why-cant-i-use-path-aliases-to-reference-other-workspaces)

Projects can use `compilerOptions.paths` to alias paths in other workspaces in
the same monorepo. Knip doesn’t understand those paths might represent internal
workspaces and might report false positives. How we ended up here is a bit
complicated, but a major reason is that Knip does it’s job based on the
workspace graph, while [workspaces are different from TypeScript programs](#whats-the-difference-between-workspaces-projects-and-programs)
and [the workspace graph doesn’t match TypeScript project structure](#why-doesnt-knip-match-my-typescript-project-structure).

The recommendation and best practice is to list such workspaces/dependencies in
`package.json`, and import them as such. Other tooling should not have any
issues with this standard approach either.

Also see the example in [TypeScript path aliases in monorepos](../guides/handling-issues#typescript-path-aliases-in-monorepos).

### What’s up with that configurable `tsconfig.json` location?

[
Section titled “What’s up with that configurable tsconfig.json location?”](#whats-up-with-that-configurable-tsconfigjson-location)

There’s a difference between `--tsConfig [file]` as a CLI argument and the
`typescript.config` option in Knip configuration.

The [`--tsConfig [file]` option](../reference/cli#--tsconfig-file) is used to provide an alternative location
for the default root `tsconfig.json` file. Relevant `compilerOptions` include
`paths` and `moduleResolution`. This setting is only available at the root
level.

On the other hand, the [`config` option of the plugin](../explanations/plugins#configuration-files) can be set per
workspace. The TypeScript plugin extracts referenced external dependencies such
as those in `extends`, `compilerOptions.types` and JSX settings:

tsconfig.json

```

{

  "extends": "@tsconfig/node20/tsconfig.json",

  "compilerOptions": {

    "jsxImportSource": "hastscript/svg"

  }

}

```

From this example, Knip can determine whether the `@tsconfig/node20` and
`hastscript` dependencies are properly listed in `package.json`.

#### Notes

[
Section titled “Notes”](#notes)

- The TypeScript plugin doesn’t add support for TypeScript to Knip (that’s
  already built-in). Like other plugins, it extracts dependencies from
  `tsconfig.json`. With the `typescript.config` option an alternative location
  for `tsconfig.json` can be set per workspace.

- In case path aliases from `compilerOptions.paths` aren’t picked up by Knip,
  either use `--tsConfig [file]` to target a different `tsconfig.json`, or
  manually add [paths](../reference/configuration#paths) to the Knip configuration. The latter can be done per
  workspace.

## Compilers

[
Section titled “Compilers”](#compilers)

### How does Knip handle Svelte or Astro files?

[
Section titled “How does Knip handle Svelte or Astro files?”](#how-does-knip-handle-svelte-or-astro-files)

To further increase the coverage of the module graph, non-standard files other
than JavaScript and TypeScript modules should be included as well. For instance,
`.mdx` and `.astro` files can import each other, internal modules and external
dependencies.

Knip includes basic “compilers” for a few common file types (Astro, MDX, Svelte,
Vue). Knip does not include actual compilers for reasons of potential
incompatibility with the existing compiler, and dependency size. Knip allows to
override them with the compilers in your project, and add additional ones for
other file types.

### Why are the exports of my `.vue` files not used?

[
Section titled “Why are the exports of my .vue files not used?”](#why-are-the-exports-of-my-vue-files-not-used)

Knip comes with basic “compilers” for a few common non-standard file types.
They’re not actual compilers, they’re regular expressions only to extract import
statements. Override the built-in Vue “compiler” with the real one in your
project. Also see the answer to the previous question and [Compilers](../features/compilers).

## Miscellaneous

[
Section titled “Miscellaneous”](#miscellaneous)

### Why isn’t production mode the default?

[
Section titled “Why isn’t production mode the default?”](#why-isnt-production-mode-the-default)

The default mode of Knip includes all source files, tests, dependencies, dev
dependencies and tooling configuration.

On the other hand, production mode considers only source files and production
dependencies. Plugins add only production entry files.

Which mode should’ve been the default? They both have their merits:

- Production mode catches dead production code and dependencies. This mode has
  the most impact on UX, since less code tends to be faster and safer.

- Default mode potentially catches more issues, e.g. lots of unused plugins of
  tooling, including most issues found in production mode. This mode has the
  most impact on DX, for the same reason.

Also see [production mode](../features/production-mode).

### Why doesn’t Knip have…?

[
Section titled “Why doesn’t Knip have…?”](#why-doesnt-knip-have)

Examples of features that have been requested include:

- Expose programmatic API

- Add local/custom plugins

- Expose the module and dependency graphs

- Custom AST visitors, e.g. to find and return:

Unused interface/type members

- Unused object members (and e.g. React component props)

- Unused object props in function return values

- Analyze workspaces in parallel

- Plugins for editors like VS Code and WebStorm (LSP-based?)

- Support Deno

- Improve internal code structures and accessibility to support contributions

- One-shot dead code removal (more comprehensive removal of unused variables,
  duplicate exports, dead code, etc).

- Replace dependencies for better performance and correctness, such as for shell
  script parsing, module resolution and globbing with “unignores”.

These are all interesting ideas, but most increase the API surface area, and all
require more development efforts and maintenance. Time is limited and
[sponsorships](/sponsors) currently don’t cover - this can change though!

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/reference/faq.md)

[
Previous
Dynamic Configuration ](/reference/dynamic-configuration) [
Next
Issue Types ](/reference/issue-types)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Issue Types

**Source:** https://knip.dev/reference/issue-types

---

# Issue Types

Knip reports the following types of issues:

TitleDescriptionKeyUnused filesUnable to find a reference to this file🔧`files`Unused dependenciesUnable to find a reference to this dependency🔧`dependencies`Unused devDependenciesUnable to find a reference to this devDependency🔧`dependencies`Referenced optional peerDependenciesOptional peer dependency is referenced`dependencies`Unlisted dependenciesUsed dependencies not listed in package.json`unlisted`Unlisted binariesBinaries from dependencies not listed in package.json`binaries`Unresolved importsUnable to resolve this (import) specifier`unresolved`Unused exportsUnable to find a reference to this export🔧`exports`Unused exported typesUnable to find a reference to this exported type🔧`types`Exports in used namespaceNamespace with export is referenced, but not export itself🔧 🟠`nsExports`Exported types in used namespaceNamespace with type is referenced, but not type itself🔧 🟠`nsTypes`Unused exported enum membersUnable to find a reference to this enum member🔧`enumMembers`Unused exported class membersUnable to find a reference to this class member🔧 🟠`classMembers`Duplicate exportsThis is exported more than once`duplicates`

## Legend

[
Section titled “Legend”](#legend)

Description🔧[Auto-fixable](../features/auto-fix) issue types🟠Not included by default (include with [filters](../features/rules-and-filters#filters))

## Notes

[
Section titled “Notes”](#notes)

- When an issue type has zero issues, it is not shown.

- The `devDependencies` and `optionalPeerDependencies` are covered in a single
  key for all `dependencies`. In [strict production mode](../features/production-mode#strict-mode), `devDependencies`
  are not included.

- The `types` issue type includes `enum`, `interface` and `type` exports.

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/reference/issue-types.md)

[
Previous
FAQ ](/reference/faq) [
Next
JSDoc & TSDoc Tags ](/reference/jsdoc-tsdoc-tags)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# JSDoc & TSDoc Tags

**Source:** https://knip.dev/reference/jsdoc-tsdoc-tags

---

# JSDoc & TSDoc Tags

JSDoc or TSDoc tags can be used to make exceptions for unused or duplicate
exports.

Knip tries to minimize configuration and introduces no new syntax. That’s why it
hooks into JSDoc and TSDoc tags.

Caution

Adding tags or excluding a certain type of issues from the report is usually not
recommended. It hides issues, which is often a sign of code smell or ambiguity
and ends up harder to maintain. It’s usually better to refactor the code (or
report an issue with Knip for false positives).

JSDoc comments always start with `/**` (not `//`) and can be single or
multi-line.

## Tags

[
Section titled “Tags”](#tags)

Use arbitrary [tags](../reference/cli#--tags) to exclude or include tagged exports from the report.
Example:

```

/** @lintignore */

export const myUnusedExport = 1;

/** @lintignore */

import Unresolved from './generated/lib.js';

```

And then include (`+`) or exclude (`-`) these tagged exports from the report
like so:

Terminal window

```

knip --tags=-lintignore,-internal

```

Tags can also be [configured in `knip.json`](./configuration#tags).

## `@public`

[
Section titled “@public”](#public)

By default, Knip reports unused exports in non-entry files.

Tag the export as `@public` and Knip will not report it.

Example:

```

/**

 * @public

 */

export const unusedFunction = () => {};

```

This tag can also be used to make exceptions in entry files when using
[—include-entry-exports](./cli#--include-entry-exports).

[JSDoc: @public](https://jsdoc.app/tags-public.html) and [TSDoc: @public](https://tsdoc.org/pages/tags/public/)

## `@internal`

[
Section titled “@internal”](#internal)

Internal exports are not meant for public consumption, but only for internal
usage such as tests. This means they would be reported in [production mode](../features/production-mode).

Mark the export with `@internal` and Knip will not report the export in
production mode.

Example:

```

/** @internal */

export const internalTestedFunction = () => {};

```

In general it’s not recommended to expose and test implementation details, but
exceptions are possible. Those should not be reported as false positives, so
when using production mode you’ll need to help Knip out by tagging them as
`@internal`.

[TSDoc: @internal](https://tsdoc.org/pages/tags/internal/)

## `@alias`

[
Section titled “@alias”](#alias)

Knip reports duplicate exports. To prevent this, tag one of the exports as
`@alias`.

Example:

```

export const Component = () => {};

/** @alias */

export default Component;

```

An alternative solution is to use `--exclude duplicates` and exclude all
duplicates from being reported.

[JSDoc: @alias](https://jsdoc.app/tags-alias.html)

## `@beta`

[
Section titled “@beta”](#beta)

Works identical to [`@public`](#public). Knip ignores other tags like `@alpha` and
`@experimental`.

[TSDoc: @beta](https://tsdoc.org/pages/tags/beta/)

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/reference/jsdoc-tsdoc-tags.md)

[
Previous
Issue Types ](/reference/issue-types) [
Next
Known Issues ](/reference/known-issues)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Known Issues

**Source:** https://knip.dev/reference/known-issues

---

# Known Issues

This page contains a list of known issues you might run into when using Knip.

## Exceptions from config files

[
Section titled “Exceptions from config files”](#exceptions-from-config-files)

An exception may be thrown when a Knip plugin loads a JavaScript or TypeScript
configuration file such as `webpack.config.js` or `vite.config.ts`. Knip may
load such files differently, in a different environment, or without certain
environment variables set.

If it isn’t clear what’s throwing the exception, try another run with `--debug`
to locate the cause of the issue with more details. Examples of issues when Knip
loads configuration files:

- Missing environment variable

- Relative path (e.g. which is sometimes not resolved from correct directory,
  try `import.meta.dirname` or `__dirname`)

As a last resort, the [plugin can be disabled](./configuration#plugins).

## Path aliases in config files

[
Section titled “Path aliases in config files”](#path-aliases-in-config-files)

Loading the configuration file (e.g. `cypress.config.ts`) for one of Knip’s
plugins may give an error:

-

```

Analyzing workspace ....

Error loading .../cypress.config.ts

Reason: Cannot find module '@alias/name'

Require stack:

- .../cypress.config.ts

```

Some tools (such as Cypress and Jest) support using TypeScript path aliases in
the configuration file. Jiti does support aliases, but in a different format
compared to `tsconfig.json#compilerOptions.paths` and `knip.json#paths` (e.g.
the target values are not arrays).

Potential workarounds:

Rewrite the import in the configuration file to a relative import.

- Use Bun with [knip-bun](./cli#knip-bun).

- [Disable the plugin](./configuration#plugins) (not recommended, try the other options first).

## Nx Daemon

[
Section titled “Nx Daemon”](#nx-daemon)

In Nx projects you might encounter this error:

Terminal window

```

NX   Daemon process terminated and closed the connection

```

The solution is to [disable the Nx Daemon](https://nx.dev/concepts/nx-daemon#turning-it-off):

Terminal window

```

NX_DAEMON=false knip

```

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/reference/known-issues.md)

[
Previous
JSDoc & TSDoc Tags ](/reference/jsdoc-tsdoc-tags) [
Next
Plugins (115) ](/reference/plugins)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Plugins (115)

**Source:** https://knip.dev/reference/plugins

---

# Plugins (115)

- [Angular](/reference/plugins/angular)

- [Astro](/reference/plugins/astro)

- [Ava](/reference/plugins/ava)

- [Babel](/reference/plugins/babel)

- [Biome](/reference/plugins/biome)

- [bumpp](/reference/plugins/bumpp)

- [Bun](/reference/plugins/bun)

- [c8](/reference/plugins/c8)

- [Capacitor](/reference/plugins/capacitor)

- [Changelogen](/reference/plugins/changelogen)

- [Changelogithub](/reference/plugins/changelogithub)

- [Changesets](/reference/plugins/changesets)

- [Commitizen](/reference/plugins/commitizen)

- [commitlint](/reference/plugins/commitlint)

- [Convex](/reference/plugins/convex)

- [create-typescript-app](/reference/plugins/create-typescript-app)

- [CSpell](/reference/plugins/cspell)

- [Cucumber](/reference/plugins/cucumber)

- [Cypress](/reference/plugins/cypress)

- [Danger](/reference/plugins/danger)

- [dependency-cruiser](/reference/plugins/dependency-cruiser)

- [Docusaurus](/reference/plugins/docusaurus)

- [dotenv](/reference/plugins/dotenv)

- [Drizzle](/reference/plugins/drizzle)

- [Eleventy](/reference/plugins/eleventy)

- [ESLint](/reference/plugins/eslint)

- [Expo](/reference/plugins/expo)

- [Gatsby](/reference/plugins/gatsby)

- [GitHub Action](/reference/plugins/github-action)

- [GitHub Actions](/reference/plugins/github-actions)

- [glob](/reference/plugins/glob)

- [GraphQL Codegen](/reference/plugins/graphql-codegen)

- [Hardhat](/reference/plugins/hardhat)

- [husky](/reference/plugins/husky)

- [i18next Parser](/reference/plugins/i18next-parser)

- [Jest](/reference/plugins/jest)

- [Karma](/reference/plugins/karma)

- [Ladle](/reference/plugins/ladle)

- [Lefthook](/reference/plugins/lefthook)

- [lint-staged](/reference/plugins/lint-staged)

- [LintHTML](/reference/plugins/linthtml)

- [lockfile-lint](/reference/plugins/lockfile-lint)

- [Lost Pixel](/reference/plugins/lost-pixel)

- [markdownlint](/reference/plugins/markdownlint)

- [Metro](/reference/plugins/metro)

- [Mocha](/reference/plugins/mocha)

- [moonrepo](/reference/plugins/moonrepo)

- [Mock Service Worker](/reference/plugins/msw)

- [Nano Staged](/reference/plugins/nano-staged)

- [Nest](/reference/plugins/nest)

- [Netlify](/reference/plugins/netlify)

- [Next.js](/reference/plugins/next)

- [Node.js](/reference/plugins/node)

- [node-modules-inspector](/reference/plugins/node-modules-inspector)

- [nodemon](/reference/plugins/nodemon)

- [npm-package-json-lint](/reference/plugins/npm-package-json-lint)

- [Nuxt](/reference/plugins/nuxt)

- [Nx](/reference/plugins/nx)

- [nyc](/reference/plugins/nyc)

- [oclif](/reference/plugins/oclif)

- [Oxlint](/reference/plugins/oxlint)

- [Playwright](/reference/plugins/playwright)

- [Playwright for components](/reference/plugins/playwright-ct)

- [playwright-test](/reference/plugins/playwright-test)

- [Plop](/reference/plugins/plop)

- [pnpm](/reference/plugins/pnpm)

- [PostCSS](/reference/plugins/postcss)

- [Preconstruct](/reference/plugins/preconstruct)

- [Prettier](/reference/plugins/prettier)

- [Prisma](/reference/plugins/prisma)

- [React Cosmos](/reference/plugins/react-cosmos)

- [React Router](/reference/plugins/react-router)

- [Relay](/reference/plugins/relay)

- [Release It!](/reference/plugins/release-it)

- [Remark](/reference/plugins/remark)

- [Remix](/reference/plugins/remix)

- [Rollup](/reference/plugins/rollup)

- [Rsbuild](/reference/plugins/rsbuild)

- [Rslib](/reference/plugins/rslib)

- [Rspack](/reference/plugins/rspack)

- [Rstest](/reference/plugins/rstest)

- [Semantic Release](/reference/plugins/semantic-release)

- [Sentry](/reference/plugins/sentry)

- [simple-git-hooks](/reference/plugins/simple-git-hooks)

- [size-limit](/reference/plugins/size-limit)

- [SST](/reference/plugins/sst)

- [Starlight](/reference/plugins/starlight)

- [Storybook](/reference/plugins/storybook)

- [Stryker](/reference/plugins/stryker)

- [Stylelint](/reference/plugins/stylelint)

- [Svelte](/reference/plugins/svelte)

- [SVGO](/reference/plugins/svgo)

- [Syncpack](/reference/plugins/syncpack)

- [Tailwind](/reference/plugins/tailwind)

- [Travis CI](/reference/plugins/travis)

- [ts-node](/reference/plugins/ts-node)

- [tsdown](/reference/plugins/tsdown)

- [tsup](/reference/plugins/tsup)

- [tsx](/reference/plugins/tsx)

- [TypeDoc](/reference/plugins/typedoc)

- [TypeScript](/reference/plugins/typescript)

- [unbuild](/reference/plugins/unbuild)

- [UnoCSS](/reference/plugins/unocss)

- [Vercel OG](/reference/plugins/vercel-og)

- [Vike](/reference/plugins/vike)

- [Vite](/reference/plugins/vite)

- [Vitest](/reference/plugins/vitest)

- [Vue](/reference/plugins/vue)

- [WebdriverIO](/reference/plugins/webdriver-io)

- [webpack](/reference/plugins/webpack)

- [Wireit](/reference/plugins/wireit)

- [Wrangler](/reference/plugins/wrangler)

- [xo](/reference/plugins/xo)

- [Yarn](/reference/plugins/yarn)

- [yorkie](/reference/plugins/yorkie)

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/reference/plugins.md)

[
Previous
Known Issues ](/reference/known-issues) [
Next
Related Tooling ](/reference/related-tooling)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Related Tooling

**Source:** https://knip.dev/reference/related-tooling

---

# Related Tooling

This is an overview of related tooling for features Knip does not support.

## Unused variables

[
Section titled “Unused variables”](#unused-variables)

Knip doesn’t look for unused variables within a file. The focus is on exported
and imported values and types across files.

Use [ESLint](https://eslint.org), [Biome](https://biomejs.dev/linter/) or [oxlint](https://oxc.rs/docs/guide/usage/linter.html) to find unused variables within
files.

Use [remove-unused-vars](https://github.com/webpro-nl/remove-unused-vars) to remove unused code within files, but in a more
valiant way. Using input from any of the above linters, it actually removes a
lot more unused code. This pairs great with Knip.

## Unused properties

[
Section titled “Unused properties”](#unused-properties)

Knip does not yet support finding unused members of types, interfaces and
objects. This includes returned objects from exported functions and objects
passed as React component props.

Knip does support finding unused members of enums and classes, and exported
values and types on imported namespaces.

## Circular dependencies

[
Section titled “Circular dependencies”](#circular-dependencies)

Knip has no issues with circular dependencies, and does not report them. Tools
that do support this include [DPDM](https://github.com/acrazing/dpdm), [Madge](https://github.com/pahen/madge) and [skott](https://github.com/antoine-coulon/skott).

## Cleanup

[
Section titled “Cleanup”](#cleanup)

The [e18e.dev](https://e18e.dev) website and in particular the [Cleanup](https://e18e.dev/guide/cleanup.html) section is a great
resource when dealing with technical debt.

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/reference/related-tooling.md)

[
Previous
Plugins (115) ](/reference/plugins) [
Next
The State of Knip ](/blog/state-of-knip)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Unused dependencies

**Source:** https://knip.dev/typescript/unused-dependencies

---

# Unused dependencies

One of Knip’s core features is finding unused dependencies in your JavaScript
and TypeScript projects. And it comes with many more features to remove clutter
and keep your projects in great shape.

## Why are unused dependencies a problem?

[
Section titled “Why are unused dependencies a problem?”](#why-are-unused-dependencies-a-problem)

Having unused dependencies in your `package.json` is an issue for various
reasons:

- They might end up in the final production bundle, increasing size and load
  times for end users.

- They waste space in `node_modules` and add to the installation time of the
  project.

- They slow down tooling such as linters and bundlers that analyze dependencies.

- They are confusing and noisy in `package.json`.

- They cause unnecessary extra work when managing and upgrading dependencies.

- They can cause version conflicts with other dependencies in use.

- They can cause false security alerts.

- They might have restrictive licenses and make your project subject to theirs.

- They usually come with transitive dependencies that have the same issues.

## How do I find unused dependencies?

[
Section titled “How do I find unused dependencies?”](#how-do-i-find-unused-dependencies)

Use Knip to find and remove unused dependencies. It also finds dependencies that
are missing in `package.json` and has a lot more features to keep your
JavaScript and TypeScript projects tidy.

It’s easy to [get started](../overview/getting-started) and make package management easier and more fun!

## How does Knip identify unused dependencies?

[
Section titled “How does Knip identify unused dependencies?”](#how-does-knip-identify-unused-dependencies)

Knip works by analyzing `package.json` files, source code and configuration
files for other tooling in the project to find unused and missing dependencies.
Knip has many heuristics, [plugins](../reference/plugins) and [compilers](../features/compilers) to fully automate the
process.

## Can Knip remove unused dependencies?

[
Section titled “Can Knip remove unused dependencies?”](#can-knip-remove-unused-dependencies)

Yes, Knip can automatically remove unused dependencies installed by a package
manager like npm or pnpm for you. Add the `--fix` argument to [auto-fix](../features/auto-fix) and
remove unused dependencies from `package.json`.

## Can Knip detect missing dependencies?

[
Section titled “Can Knip detect missing dependencies?”](#can-knip-detect-missing-dependencies)

Yes, Knip detects missing dependencies. It analyzes `package.json` files, and
reports packages that are missing. They should be added to `package.json` to
avoid relying on transitive dependencies that can cause version mismatches and
breakage.

## Does Knip work with monorepos?

[
Section titled “Does Knip work with monorepos?”](#does-knip-work-with-monorepos)

Yes, Knip has first-class support for [monorepos and workspaces](../features/monorepos-and-workspaces). It analyzes
all workspaces in the project and understands their relationship.

For instance, if a dependency is listed in the root `package.json` it does not
need to be listed in other workspaces. Except if you enable `--strict` checking.

## Does Knip separate dependencies and devDependencies?

[
Section titled “Does Knip separate dependencies and devDependencies?”](#does-knip-separate-dependencies-and-devdependencies)

Yes, Knip understands the difference between dependencies and devDependencies.
It has a [production mode](../features/production-mode) to focus on production code only and find dead
code and dependencies that would otherwise only be referenced by tests and other
tooling. This allows you to remove both unused exported code and their tests.

## Does Knip work with my package manager?

[
Section titled “Does Knip work with my package manager?”](#does-knip-work-with-my-package-manager)

Yes, Knip works with any package manager: npm, pnpm, Bun and Yarn are all
supported. It’s easy to [get started](../overview/getting-started) with any package manager.

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/typescript/unused-dependencies.md)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Unused exports

**Source:** https://knip.dev/typescript/unused-exports

---

# Unused exports

Finding unused exports in your JavaScript and TypeScript projects is one of
Knip’s core features. And it comes with even more features to identify and
remove clutter to keep your projects in great shape.

## Why are unused exports a problem?

[
Section titled “Why are unused exports a problem?”](#why-are-unused-exports-a-problem)

Having unused exports in your codebase is problematic for several reasons:

- They increase bundle sizes if not properly eliminated by tree-shaking.

- They clutter the codebase and make it harder to navigate and understand.

- They mislead developers into thinking certain code is used when it’s not.

- They make refactoring and maintaining the codebase more difficult.

- They slow down tooling that analyze the codebase, such as bundlers, linters
  and type checkers.

- They may represent dead code that is no longer needed but hasn’t been cleaned
  up.

## How do I find unused exports?

[
Section titled “How do I find unused exports?”](#how-do-i-find-unused-exports)

Knip is a powerful tool that can help you find and remove unused exports in your
JavaScript and TypeScript projects. It analyzes the codebase, identifies exports
that are not imported anywhere, and reports them.

[Get started and install Knip](../overview/getting-started) to run it on your project. Knip will scan your
files and provide a detailed report of unused exports, and much more.

## How does Knip identify unused exports?

[
Section titled “How does Knip identify unused exports?”](#how-does-knip-identify-unused-exports)

Knip performs both static and dynamic analysis to determine which exports are
actually being used in your codebase. It looks at import statements, export
usage, and [a lot more code patterns](../reference/faq#what-does-knip-look-for-in-source-files) to identify unused exports.

Knip supports JavaScript and TypeScript projects, and handles both [CommonJS](../guides/working-with-commonjs)
and ES Modules syntax.

## Can Knip remove unused exports?

[
Section titled “Can Knip remove unused exports?”](#can-knip-remove-unused-exports)

Yes, Knip not only finds unused exports but can also remove them for you. Run
Knip with the `--fix` flag to enable [the auto-fix feature](../features/auto-fix), and it will
modify your source code and remove the unused exports.

It’s always recommended to review the changes made by Knip before committing
them to ensure no unintended modifications were made.

## Can Knip handle large codebases?

[
Section titled “Can Knip handle large codebases?”](#can-knip-handle-large-codebases)

Absolutely. Knip supports [monorepos with workspaces](../features/monorepos-and-workspaces) and utilizes [workspace
sharing](../guides/performance#workspace-sharing) to efficiently analyze large monorepos. This makes it easier and
more fun to manage and optimize large multi-package projects.

## Does Knip work with my favorite editor or IDE?

[
Section titled “Does Knip work with my favorite editor or IDE?”](#does-knip-work-with-my-favorite-editor-or-ide)

Knip is a command-line tool that runs independently of your editor or IDE.
However, if you run Knip inside an integrated IDE terminal, the report contains
file names and positions in a format IDEs like VS Code and WebStorm understand
to easily navigate around.

## How is Knip different from ESLint for finding unused exports?

[
Section titled “How is Knip different from ESLint for finding unused exports?”](#how-is-knip-different-from-eslint-for-finding-unused-exports)

While linters like ESLint can find unused variables and imports within
individual files, Knip analyzes the entire project to determine which exports
are actually unused. By building [a comprehensive module graph](../reference/faq#whats-in-the-graphs), Knip
identifies exports that are not imported or used anywhere in the codebase. This
allows Knip to catch unused exports and dead code that ESLint and other linters
would miss.

Also see [Why isn’t Knip an ESLint plugin?](../reference/faq#why-isnt-knip-an-eslint-plugin)

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/typescript/unused-exports.md)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Writing A Plugin

**Source:** https://knip.dev/writing-a-plugin

---

# Writing A Plugin

Plugins provide Knip with entry files and dependencies it would be unable to
find otherwise. Plugins always do at least one of the following:

- Define entry file patterns

- Find dependencies in configuration files

Knip v5.1.0 introduced a new plugin API, which makes them a breeze to write and
maintain.

The new plugin API

Easy things should be easy, and complex things possible.

This tutorial walks through example plugins so you’ll be ready to write your
own! The following examples demonstrate the elements a plugin can implement.

There’s a handy command available to easily [create a new plugin](#create-a-new-plugin) and get
started right away.

## Example 1: entry

[
Section titled “Example 1: entry”](#example-1-entry)

Let’s dive right in. Here’s the entire source code of the Tailwind plugin:

-

```

import type { IsPluginEnabled, Plugin } from '../../types/config.js';

import { hasDependency } from '../../util/plugin.js';

const title = 'Tailwind';

const enablers = ['tailwindcss'];

const isEnabled: IsPluginEnabled = ({ dependencies }) =>

  hasDependency(dependencies, enablers);

const entry = ['tailwind.config.{js,cjs,mjs,ts}'];

export default {

  title,

  enablers,

  isEnabled,

  entry,

} satisfies Plugin;

```

Yes, that’s the entire plugin! Let’s go over each item one by one:

### 1. `title`

[
Section titled “1. title”](#1-title)

The title of the plugin displayed in the [list of plugins](../reference/plugins) and in debug
output.

### 2. `enablers`

[
Section titled “2. enablers”](#2-enablers)

An array of strings to match one or more dependencies in `package.json` so the
`isEnabled` function can determine whether the plugin should be enabled or not.
Regular expressions are allowed as well.

### 3. `isEnabled`

[
Section titled “3. isEnabled”](#3-isenabled)

This function checks whether a match is found in the `dependencies` or
`devDependencies` in `package.json`. The plugin is enabled if the dependency is
listed in `package.json`.

This function can be kept straightforward with the `hasDependency` helper.

### 4. `entry`

[
Section titled “4. entry”](#4-entry)

This plugin exports `entry` file patterns. This means that if the Tailwind
plugin is enabled, then `tailwind.config.*` files are added as entry files. A
Tailwind configuration file does not contain anything particular, so adding it
as an `entry` to treat it as a regular source file is enough.

The next example shows how to handle a tool that has its own particular
configuration object.

## Example 2: config

[
Section titled “Example 2: config”](#example-2-config)

Here’s the full source code of the `nyc` plugin:

```

import { toDeferResolve } from '../../util/input.js';

import { hasDependency } from '../../util/plugin.js';

import type { NycConfig } from './types.js';

import type {

  IsPluginEnabled,

  Plugin,

  ResolveConfig,

} from '../../types/config.js';

const title = 'nyc';

const enablers = ['nyc'];

const isEnabled: IsPluginEnabled = ({ dependencies }) =>

  hasDependency(dependencies, enablers);

const config = [

  '.nycrc',

  '.nycrc.{json,yml,yaml}',

  'nyc.config.js',

  'package.json',

];

const resolveConfig: ResolveConfig<NycConfig> = config => {

  const extend = config?.extends ?? [];

  const requires = config?.require ?? [];

  return [extend, requires].flat().map(id => toDeferResolve(id));

};

export default {

  title,

  enablers,

  isEnabled,

  config,

  resolveConfig,

} satisfies Plugin;

```

Here’s an example `config` file that will be handled by this plugin:

.nycrc.json

```

{

  "extends": "@istanbuljs/nyc-config-typescript",

  "check-coverage": true

}

```

Compared to the first example, this plugin has two new variables:

### 5. `config`

[
Section titled “5. config”](#5-config)

The `config` array contains all possible locations of the config file for the
tool. Knip loads matching files and passes the results (i.e. its default export)
into the `resolveConfig` function:

### 6. `resolveConfig`

[
Section titled “6. resolveConfig”](#6-resolveconfig)

This function receives the exported value of the `config` file, and executes the
`resolveConfig` function with this object. The plugin should return the entry
paths and dependencies referenced in this object.

Knip supports JSON, YAML, TOML, JavaScript and TypeScript config files. Files
without an extension are provided as plain text strings.

Should I implement resolveConfig?

You should implement `resolveConfig` if any of these are true:

The `config` file contains one or more options that represent [entry
points](../explanations/plugins#entry-files-from-config-files)

- The `config` file references dependencies by strings (not import statements)

## Example 3: entry paths

[
Section titled “Example 3: entry paths”](#example-3-entry-paths)

### 7. entry and production

[
Section titled “7. entry and production”](#7-entry-and-production)

Some tools operate mostly on entry files, some examples:

- Mocha looks for test files at `test/*.{js,cjs,mjs}`

- Storybook looks for stories at `*.stories.@(mdx|js|jsx|tsx)`

And some of those tools allow to configure those locations and patterns in
configuration files, such as `next.config.js` or `vite.config.ts`. If that’s the
case we can define `resolveConfig` in our plugin to take this from the
configuration object and return it to Knip:

Here’s an example from the Mocha plugin:

```

const entry = ['**/test/*.{js,cjs,mjs}'];

const resolveConfig: ResolveConfig<MochaConfig> = localConfig => {

  const entryPatterns = localConfig.spec ? [localConfig.spec].flat() : entry;

  return entryPatterns.map(id => toEntry(id));

};

export default {

  entry,

  resolveConfig,

};

```

With Mocha, you can configure `spec` file patterns. The result of implementing
`resolveConfig` is that users don’t need to duplicate this configuration in both
the tool (e.g. Mocha) and Knip.

Use `production` entries to target source files that represent production code.

Tip

Regardless of the presence of `resolveConfig`, add `entry` and `production` to
the default export so they will be displayed in the plugin’s documentation as
default values.

## Example 4: Use the AST directly

[
Section titled “Example 4: Use the AST directly”](#example-4-use-the-ast-directly)

If the `resolveFromConfig` function is implemented, Knip loads the configuration
file and passes the default-exported object to this plugin function. However,
that object might then not contain the information we need.

Here’s an example `astro.config.ts` configuration file with a Starlight
integration:

```

import starlight from '@astrojs/starlight';

import { defineConfig } from 'astro/config';

export default defineConfig({

  integrations: [

    starlight({

      components: {

        Head: './src/components/Head.astro',

        Footer: './src/components/Footer.astro',

      },

    }),

  ],

});

```

With Starlight, components can be defined to override the default internal ones.
They’re not otherwise referenced in your source code, so you’d have to manually
add them as entry files ([Knip itself did this](https://github.com/webpro-nl/knip/blob/6a6954386b33ee8a2919005230a4bc094e11bc03/knip.json#L12)).

In the Astro plugin, there’s no way to access this object containing
`components` to add the component files as entry files if we were to try:

```

const resolveConfig: ResolveConfig<AstroConfig> = async config => {

  console.log(config); //  ¯\_(ツ)_/¯

};

```

This is why plugins can implement the `resolveFromAST` function.

### 7. resolveFromAST

[
Section titled “7. resolveFromAST”](#7-resolvefromast)

Let’s take a look at the Astro plugin implementation. This example assumes some
familiarity with Abstract Syntax Trees (AST) and the TypeScript compiler API.
Knip will provide more and more AST helpers to make implementing plugins more
fun and a little less tedious.

Anyway, let’s dive in. Here’s how we’re adding the Starlight `components` paths
to the default `production` file patterns:

```

import ts from 'typescript';

import {

  getDefaultImportName,

  getImportMap,

  getPropertyValues,

} from '../../typescript/ast-helpers.js';

const title = 'Astro';

const production = [

  'src/pages/**/*.{astro,mdx,js,ts}',

  'src/content/**/*.mdx',

  'src/middleware.{js,ts}',

  'src/actions/index.{js,ts}',

];

const getComponentPathsFromSourceFile = (sourceFile: ts.SourceFile) => {

  const componentPaths: Set<string> = new Set();

  const importMap = getImportMap(sourceFile);

  const importName = getDefaultImportName(importMap, '@astrojs/starlight');

  function visit(node: ts.Node) {

    if (

      ts.isCallExpression(node) &&

      ts.isIdentifier(node.expression) &&

      node.expression.text === importName // match the starlight() function call

    ) {

      const starlightConfig = node.arguments[0];

      if (ts.isObjectLiteralExpression(starlightConfig)) {

        const values = getPropertyValues(starlightConfig, 'components');

        for (const value of values) componentPaths.add(value);

      }

    }

    ts.forEachChild(node, visit);

  }

  visit(sourceFile);

  return componentPaths;

};

const resolveFromAST: ResolveFromAST = (sourceFile: ts.SourceFile) => {

  // Include './src/components/Head.astro' and './src/components/Footer.astro'

  // as production entry files so they're also part of the analysis

  const componentPaths = getComponentPathsFromSourceFile(sourceFile);

  return [...production, ...componentPaths].map(id => toProductionEntry(id));

};

export default {

  title,

  production,

  resolveFromAST,

} satisfies Plugin;

```

## Inputs

[
Section titled “Inputs”](#inputs)

You may have noticed functions like `toDeferResolve` and `toEntry`. They’re a
way for plugins to tell what they’ve found and how Knip should handle those. The
more precision a plugin can provide here, the better results and performance
will be.

Find all the details over at [Writing A Plugin → Inputs](./writing-a-plugin/inputs).

## Argument Parsing

[
Section titled “Argument Parsing”](#argument-parsing)

As part of the [script parser](../features/script-parser), Knip parses command-line arguments. Plugins
can implement the `arg` object to add custom argument parsing tailored to the
tool.

Read more in [Writing A Plugin → Argument Parsing](./writing-a-plugin/argument-parsing).

## Create a new plugin

[
Section titled “Create a new plugin”](#create-a-new-plugin)

The easiest way to create a new plugin is to use the `create-plugin` script:

Terminal window

```

cd packages/knip

pnpm create-plugin --name tool

```

This adds source and test files and fixtures to get you started. It also adds
the plugin to the JSON Schema and TypeScript types.

Run the test for your new plugin using one of the following commands:

Terminal window

```

pnpm tsx --test test/plugins/tool.test.ts

bun test test/plugins/tool.test.ts

```

You’re ready to implement and submit a new Knip plugin! 🆕 🎉

## Wrapping Up

[
Section titled “Wrapping Up”](#wrapping-up)

Feel free to check out the implementation of other similar plugins, and borrow
ideas and code from those!

The documentation website takes care of generating the [plugin list and the
individual plugin pages](../reference/plugins) from the exported plugin values.

Thanks for reading. If you have been following this guide to create a new
plugin, this might be the right time to open a pull request!

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/writing-a-plugin/index.md)

[
Previous
Working with CommonJS ](/guides/working-with-commonjs) [
Next
Inputs ](/writing-a-plugin/inputs)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Argument Parsing

**Source:** https://knip.dev/writing-a-plugin/argument-parsing

---

# Argument Parsing

Some plugins have an `arg` object in their implementation. It’s a way for
plugins to customize how command-line arguments are parsed for their tool’s
executables. Argument parsing in plugins help Knip identify dependencies and
entry files from scripts.

Knip uses [minimist](https://www.npmjs.com/package/minimist) for argument parsing and some options are identical
([alias](#alias), [boolean](#boolean), [string](#string)).

Also see [type definitions](https://github.com/webpro-nl/knip/blob/main/packages/knip/src/types/args.ts) and [examples in existing plugins](https://github.com/search?q=repo%3Awebpro-nl%2Fknip++path%3Apackages%2Fknip%2Fsrc%2Fplugins+%22const+args+%3D%22&type=code).

- [alias](#alias)

- [args](#args)

- [binaries](#binaries)

- [boolean](#boolean)

- [config](#config)

- [fromArgs](#fromargs)

- [nodeImportArgs](#nodeimportargs)

- [positional](#positional)

- [resolve](#resolve)

- [resolveInputs](#resolveinputs)

- [string](#string)

## alias

[
Section titled “alias”](#alias)

Define aliases.

Example:

```

{

  require: ['r'];

}

```

Also see [nodeImportArgs](#nodeimportargs).

## args

[
Section titled “args”](#args)

Modify or filter arguments before parsing. For edge cases preprocessing is
useful, e.g. if minimist has trouble parsing or to modify/discard arguments.

Example:

```

{

  args: (args: string[]) => args.filter(arg => arg !== 'omit');

}

```

## binaries

[
Section titled “binaries”](#binaries)

Executables for the dependency.

Example:

```

{

  binaries: ['tsc'];

}

```

Default: plugin name, e.g. for the ESLint plugin the value is `["eslint"]`

## boolean

[
Section titled “boolean”](#boolean)

Mark arguments as boolean. By default, arguments are expected to have string
values.

## config

[
Section titled “config”](#config)

Define arguments that contain the configuration file path. Usually you’ll want
to set aliases too. Use `true` for shorthand to set `alias` + `string` +
`config`.

Example:

```

{

  config: true;

}

```

The `tsup` plugin has this. Now `tsup --config tsup.client.json` will have
`tsup.client.json` go through `resolveConfig` (also `-c` alias).

Example:

```

{

  config: ['p'];

}

```

This will mark e.g. `tsc -p tsconfig.lib.json` as a configuration file and it
will be handled by `resolveConfig` of the (typescript) plugin.

## fromArgs

[
Section titled “fromArgs”](#fromargs)

Parse return value as a new script. Can be a an array of strings, or function
that returns an array of strings and those values will be parsed separately.

Example:

```

{

  fromArgs: ['exec'];

}

```

Then this script:

Terminal window

```

nodemon --exec "node index.js"

```

Will have `"node index.js"` being parsed as a new script.

## nodeImportArgs

[
Section titled “nodeImportArgs”](#nodeimportargs)

Set to `true` as a shorthand for this [alias](#alias):

```

{

  import: ['r', 'experimental-loader', 'require', 'loader']

}

```

Example:

```

{

  nodeImportArgs: true;

}

```

## positional

[
Section titled “positional”](#positional)

Set to `true` to use the first positional argument as an entry point.

Example:

```

{

  positional: true;

}

```

The `tsx` plugin has this and `"tsx script.ts"` as a script will result in the
`script.ts` file being an entry point.

## resolve

[
Section titled “resolve”](#resolve)

List of arguments to resolve to a dependency or entry file path.

Example:

```

{

  resolve: ['plugin'];

}

```

Now for a script like `"program --plugin package"` this will result in
`"package"` being resolved as a dependency.

## resolveInputs

[
Section titled “resolveInputs”](#resolveinputs)

Return inputs from parsed arguments

```

{

  resolveInputs: (parsed: ParsedArgs) =>

    parsed['flag'] ? [toDependency('package')] : [];

}

```

## string

[
Section titled “string”](#string)

Mark arguments as string. This is the default, but number-looking arguments are
returned as numbers by minimist.

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/writing-a-plugin/argument-parsing.md)

[
Previous
Inputs ](/writing-a-plugin/inputs) [
Next
CLI Arguments ](/reference/cli)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---

# Inputs

**Source:** https://knip.dev/writing-a-plugin/inputs

---

# Inputs

You may have noticed functions like `toDeferResolve` and `toEntry`. They’re a
way for plugins to tell what they’ve found and how Knip should handle those. The
more precise a plugin can be, the better it is for results and performance.
Here’s an overview of all input type functions:

- [toEntry](#toentry)

- [toProductionEntry](#toproductionentry)

- [toProject](#toproject)

- [toDependency](#todependency)

- [toProductionDependency](#toproductiondependency)

- [toDeferResolve](#todeferresolve)

- [toDeferResolveEntry](#todeferresolveentry)

- [toConfig](#toconfig)

- [toBinary](#tobinary)

- [toAlias](#toalias)

- [Options](#options)

## toEntry

[
Section titled “toEntry”](#toentry)

An `entry` input is just like an `entry` in the configuration. It should either
be an absolute or relative path, and glob patterns are allowed.

## toProductionEntry

[
Section titled “toProductionEntry”](#toproductionentry)

A production `entry` input is just like an `production` in the configuration. It
should either be an absolute or relative path, and it can have glob patterns.

## toProject

[
Section titled “toProject”](#toproject)

A `project` input is the equivalent of `project` patterns in the configuration.
It should either be an absolute or relative path, and (negated) glob patterns
are allowed.

## toDependency

[
Section titled “toDependency”](#todependency)

The `dependency` indicates the entry is a dependency, belonging in either the
`"dependencies"` or `"devDependencies"` section of `package.json`.

## toProductionDependency

[
Section titled “toProductionDependency”](#toproductiondependency)

The production `dependency` indicates the entry is a production dependency,
expected to be listed in `"dependencies"`.

## toDeferResolve

[
Section titled “toDeferResolve”](#todeferresolve)

The `deferResolve` input type is used to defer the resolution of a specifier.
This could be resolved to a dependency or an entry file. For instance, the
specifier `"input"` could be resolved to `"input.js"`, `"input.tsx"`,
`"input/index.js"` or the `"input"` package name. Local files are added as entry
files, package names are external dependencies.

If this does not lead to a resolution, the specifier will be reported under
“unresolved imports”.

## toDeferResolveEntry

[
Section titled “toDeferResolveEntry”](#todeferresolveentry)

The `deferResolveEntry` input type is similar to `deferResolve`, but it’s used
for entry files only (not dependencies) and unresolved inputs are ignored. It’s
different from `toEntry` as glob patterns are not supported.

## toConfig

[
Section titled “toConfig”](#toconfig)

The `config` input type is a way for plugins to reference a configuration file
that should be handled by a different plugin. For instance, Angular
configurations might contain references to `tsConfig` and `karmaConfig` files,
so these `config` files can then be handled by the TypeScript and Karma plugins,
respectively.

Example:

```

toConfig('typescript', './path/to/tsconfig.json');

```

For instance, the Angular plugin uses this to tell Knip about its `tsConfig`
value in `angular.json` projects.

## toBinary

[
Section titled “toBinary”](#tobinary)

The `binary` input type isn’t used by plugins directly, but by the shell script
parser (through the `getInputsFromScripts` helper). Think of GitHub Actions
workflow YAML files or husky scripts. Using this input type, a binary is
“assigned” to the dependency that has it as a `"bin"` in their `package.json`.

## toAlias

[
Section titled “toAlias”](#toalias)

The `alias` input type adds path aliases to the core module resolver. They’re
added to `compilerOptions.paths` so the syntax is identical.

## Options

[
Section titled “Options”](#options)

When creating inputs from specifiers, an extra `options` object as the second
argument can be provided.

### dir

[
Section titled “dir”](#dir)

The optional `dir` option assigns the input to a different workspace. For
instance, GitHub Action workflows are always stored in the root workspace, and
support `working-directory` in job steps. For example:

```

jobs:

  stylelint:

    runs-on: ubuntu-latest

    steps:

      - run: npx esbuild

        working-directory: packages/app

```

The GitHub Action plugin understands `working-directory` and adds this `dir` to
the input:

```

toDependency('esbuild', { dir: 'packages/app' });

```

Knip now understands `esbuild` is a dependency of the workspace in the
`packages/app` directory.

### optional

[
Section titled “optional”](#optional)

Use the `optional` flag to indicate the dependency is optional. Then, a
dependency won’t be flagged as unlisted if it isn’t.

### allowIncludeExports

[
Section titled “allowIncludeExports”](#allowincludeexports)

By default, exports of entry files such as `src/index.ts` or the files in
`package.json#exports` are not reported as unused. When using the
`--include-entry-exports` flag or `isIncludeExports: true` option, unused
exports on such entry files are also reported.

Exports of entry files coming from plugins are not included in the analysis,
even with the option enabled. This is because certain tools and frameworks
consume named exports from entry files, causing false positives.

The `allowIncludeExports` option allows the exports of entry files to be
reported as unused when using `--include-entry-exports`. This option is
typically used with the [toProductionEntry](#toproductionentry) input type.

Example:

```

toProductionEntry('./entry.ts', { allowIncludeExports: true });

```

[
Edit page](https://github.com/webpro-nl/knip/edit/main/packages/docs/src/content/docs/writing-a-plugin/inputs.md)

[
Previous
Writing A Plugin ](/writing-a-plugin) [
Next
Argument Parsing ](/writing-a-plugin/argument-parsing)

[ISC License](https://github.com/webpro-nl/knip/blob/main/packages/knip/license) © 2024
[Lars Kappert](https://www.webpro.nl)

---
