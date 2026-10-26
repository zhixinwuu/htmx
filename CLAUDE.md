# htmx

This is htmx 2, a small JS library that lets HTML elements send AJAX requests and swap in the response using hx- attributes. All the library code is in one file, src/htmx.js, and it runs in the browser as is. There's no build step.

## Layout

- src/htmx.js is the only source file. Edit this one.
- Tests are in test/core and test/attributes. Each attribute has its own test file, like test/attributes/hx-swap.js. Shared helpers are in test/util/util.js.
- www/ is the htmx.org docs site. The pages are in www/content.

## Commands

Use Node 20, like CI does. Node 22 versions before 22.12 crash the test runner with `ERR_REQUIRE_ESM`. That error means the Node version is wrong, so don't try to fix node_modules.

```bash
npm ci
npx web-test-runner test/attributes/hx-swap.js --browsers chromium --playwright   # one test file
npm run test:chrome    # all tests
npm run lint
npm run format         # fixes lint style errors
npm run types-check    # checks the JSDoc types in src/htmx.js
npm test               # what CI runs: lint, types-check, then all tests
```

npm test installs the playwright browsers before every run, so it's slow and needs internet. For quick checks use the web-test-runner line.

## Conventions

- Tests are written in old-style JS with `var` and `function() {}`, no arrow functions. src/htmx.js uses const and let.
- Copy the setup from an existing test file. They all use makeServer() for fake responses, make() to put HTML on the page, and clearWorkArea() before and after each test.
- When fixing a bug, put the test in the test file for that attribute. regressions.js is for things that don't fit anywhere else.
- New test files go in test/core or test/attributes, because the runner only looks there. Also add them to test/index.html, which lists the test files by hand.
- Read hx attributes with getAttributeValue or getClosestAttributeValue, not getAttribute. Otherwise the data-hx- versions stop working.
- A new config option needs @type and @default comments in htmx.config, and a line in docs.md, api.md and reference.md in www/content.
- New public API functions follow the pattern at the top of src/htmx.js, where they're set to null first and assigned later. Read the "Tsc madness" comment before changing it.

## Don't touch

- Don't edit dist/. It's the release build and the maintainers rebuild it themselves. Don't run npm run dist or npm run www either.
- www/static/src, www/static/test and the htmx.js in www/themes are copies too. If grep finds four htmx.js files, only src/htmx.js matters.
- dist/ext has old htmx 1 extensions that are only kept so old links work. Leave them alone.
- Leave CHANGELOG.md alone. The maintainers write it when they release.
- Code changes go to the dev branch and docs-only changes go to master. The four and four-dev branches are htmx 4, which is a different codebase, so htmx 4 docs don't apply here.
