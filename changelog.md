# apollo-upload-client changelog

## 18.0.1

### Patch

- Corrected the function `createUploadLink` option `uri` type, fixing [#316](https://github.com/jaydenseric/apollo-upload-client/issues/316).
- Prefixed unused parameters with `_`, fixing [#317](https://github.com/jaydenseric/apollo-upload-client/issues/317).
- Fixed a typo in the changelog entry for v18.0.0.

## 18.0.0

### Major

- Updated Node.js support to `^18.15.0 || >=20.4.0`.
- Updated the [`@apollo/client`](https://npm.im/@apollo/client) peer dependency to `^3.8.0`.
- Updated the [`extract-files`](http://npm.im/extract-files) dependency to v13.

  - React Native is no longer supported out of the box.

    The class `ReactNativeFile` is no longer exported, or matched by the function `isExtractableFile`.

    This class was bloating non React Native environments with an extra module, increasing bundle sizes when building and adding an extra step to ESM loading waterfalls in browsers.

    It’s the responsibility of Facebook to adhere to web standards and implement spec-complaint `Blob`, `File`, and `FormData` globals in the React Native environment.

    To migrate, React Native projects that are unable to use the standard globals can manually implement a class `ReactNativeFile` and match it with a custom function `isReactNativeFile` for use with the function `createUploadLink` option `isExtractableFile`.

  - “Plain” objects in the GraphQL operation that aren’t `Object` instances (e.g. `Object.create(null)`) are now also deep cloned when searching for extractable files.

- Updated dev dependencies, some of which require newer Node.js versions than previously supported.
- Use the Node.js test runner API and remove the dev dependency [`test-director`](https://npm.im/test-director).
- Refactored tests to use the standard `AbortController`, `AbortSignal`, `File`, `FormData`, and `Response` APIs available in modern Node.js and removed the dev dependencies [`abort-controller`](https://npm.im/abort-controller), [`formdata-node`](https://npm.im/formdata-node), and [`node-fetch`](https://npm.im/node-fetch).
- Public modules are now individually listed in the package `files` and `exports` fields.
- Removed the package main index module; deep imports must be used. To migrate:

  ```diff
  - import {
  -   createUploadLink,
  -   formDataAppendFile,
  -   isExtractableFile
  - } from "apollo-upload-client";
  + import createUploadLink from "apollo-upload-client/createUploadLink.js";
  + import formDataAppendFile from "apollo-upload-client/formDataAppendFile.js";
  + import isExtractableFile from "apollo-upload-client/isExtractableFile.js";
  ```

- Shortened public module deep import paths, removing the `/public/`. To migrate:

  ```diff
  - import createUploadLink from "apollo-upload-client/public/createUploadLink.js";
  + import createUploadLink from "apollo-upload-client/createUploadLink.js";

  - import formDataAppendFile from "apollo-upload-client/public/formDataAppendFile.js";
  + import formDataAppendFile from "apollo-upload-client/formDataAppendFile.js";

  - import isExtractableFile from "apollo-upload-client/public/isExtractableFile.js";
  + import isExtractableFile from "apollo-upload-client/isExtractableFile.js";
  ```

- The API is now ESM in `.js` files instead of CJS in `.js` files, [accessible via `import` but not `require`](https://nodejs.org/dist/latest/docs/api/esm.html#require).
- Implemented TypeScript types via JSDoc comments.

  Types published in [`@types/apollo-upload-client`](https://npm.im/@types/apollo-upload-client) should no longer be used.

  Projects must configure TypeScript to use types from the ECMAScript modules that have a `// @ts-check` comment:

  - [`compilerOptions.allowJs`](https://www.typescriptlang.org/tsconfig#allowJs) should be `true`.
  - [`compilerOptions.maxNodeModuleJsDepth`](https://www.typescriptlang.org/tsconfig#maxNodeModuleJsDepth) should be reasonably large, e.g. `10`.
  - [`compilerOptions.module`](https://www.typescriptlang.org/tsconfig#module) should be `"node16"` or `"nodenext"`.

- Internally, use the function `selectHttpOptionsAndBodyInternal` that was added in [`@apollo/client`](https://npm.im/@apollo/client) [v3.5.5](https://github.com/apollographql/apollo-client/releases/tag/v3.5.5).

### Minor

- Added a new option `print` for the function `createUploadLink`, to customize how the GraphQL query or mutation AST prints to a string for transport. It that works like the same option for [`HttpLink`](https://www.apollographql.com/docs/react/api/link/apollo-link-http).

### Patch

- Updated dev dependencies.
- Simplified dev dependencies and config for ESLint.
- Integrated the ESLint plugin [`eslint-plugin-optimal-modules`](https://npm.im/eslint-plugin-optimal-modules).
- Check TypeScript types via a new package `types` script.
- Removed the [`jsdoc-md`](https://npm.im/jsdoc-md) dev dependency and the related package scripts, replacing the readme “API” section with a manually written “Exports” section.
- Updated the `package.json` field `repository` to conform to new npm requirements.
- Updated GitHub Actions CI config:
  - The workflow still triggers on push, but no longer on pull request.
  - The workflow can now be manually triggered.
  - Run tests with Node.js v18, v20, v21.
  - Updated `actions/checkout` to v4.
  - Updated `actions/setup-node` to v3.
- Use the `node:` URL scheme for Node.js builtin module imports.
- Reorganized the test file structure.
- In tests, for objects with the property `headers` that as of [`@apollo/client`](https://npm.im/@apollo/client) [v3.7.0](https://github.com/apollographql/apollo-client/releases/tag/v3.7.0) is a null-prototype object, use the assertion `deepEqual` instead of `deepStrictEqual`.
- Tweaked code for type safety.
- Updated documentation, including link URLs.
- Refactored example code in the readme.
- Removed the readme badges.

## 17.0.0

### Major

- Updated Node.js support to `^12.22.0 || ^14.17.0 || >= 16.0.0`.
- Updated dev dependencies, some of which require newer Node.js versions than previously supported.
- Removed `./package` from the package `exports` field; the full `package.json` filename must be used in a `require` path.

### Patch

- Also run GitHub Actions CI with Node.js v17.
- Updated the [`graphql`](https://npm.im/graphql) peer dependency to `14 - 16`.
- Refactored tests to remove the [`fetch-blob`](https://npm.im/fetch-blob) dev dependency.
- Simplified package scripts.
- Use a new `assertBundleSize` function to assert module bundle size in tests:
  - Failure message contains details about the bundle size and how much the limit was exceeded.
  - Errors when the surplus is greater than 25% of the limit, suggesting the limit should be reduced.
  - Resolves the minified bundle and its gzipped size for debugging in tests.
- Tweaked the test function `timeLimitPromise` error messages.
- Configured Prettier option `singleQuote` to the default, `false`.
- Documentation tweaks.
- Amended the changelog entry for v16.0.0.

## 16.0.0

### Major

- Updated the [`extract-files`](https://npm.im/extract-files) dependency to [v11](https://github.com/jaydenseric/extract-files/releases/tag/v11.0.0).

### Patch

- Updated dev dependencies.
- Reverted the more specific package `main` field path.
- Renamed imports in the test index module.
- Increased the bundle size test maximum allowed bundle size.
- Updated code examples to use deep imports.
- Amended the changelog entries for v14.0.0 and v15.0.0.

## 15.0.0

### Major

- Updated Node.js support to `^12.20 || >= 14.13`.
- Stopped supporting Internet Explorer.
- Changed [`@apollo/client`](https://npm.im/@apollo/client) from a dependency to a peer dependency, fixing [#251](https://github.com/jaydenseric/apollo-upload-client/issues/251) via [#252](https://github.com/jaydenseric/apollo-upload-client/pull/252).
- Updated dependencies, some of which require newer Node.js versions than previously supported.
- Replaced the the `package.json` `exports` field public [subpath folder mapping](https://nodejs.org/api/packages.html#packages_subpath_folder_mappings) (deprecated by Node.js) with a [subpath pattern](https://nodejs.org/api/packages.html#packages_subpath_patterns). Deep `require` paths within `apollo-upload-client/public/` must now include the `.js` file extension.
- Removed Babel related dependencies, config, and scripts. Published modules now contain more modern ES syntax.
- Published modules now contain JSDoc comments, which might affect TypeScript projects.
- The tests are now ESM in `.js` files instead of CJS in `.js` files.

### Patch

- Stop using [`hard-rejection`](https://npm.im/hard-rejection) to detect unhandled `Promise` rejections in tests, as Node.js v15+ does this natively.
- Test the bundle size manually using [`esbuild`](https://npm.im/esbuild) and [`gzip-size`](https://npm.im/gzip-size), removing [`size-limit`](https://npm.im/size-limit) related dev dependencies, config, and scripts.
- Updated GitHub Actions CI config:
  - Run tests with Node.js v12, v14, v16.
  - Updated `actions/checkout` to v2.
  - Updated `actions/setup-node` to v2.
  - Don’t specify the `CI` environment variable as it’s set by default.
- More specific package `main` field path.
- Simplified JSDoc related package scripts now that [`jsdoc-md`](https://npm.im/jsdoc-md) v10 automatically generates a Prettier formatted readme.
- Added a package `test:jsdoc` script that checks the readme API docs are up to date with the source JSDoc.
- Use the `.js` file extension in internal `require` paths.
- Clearer package and function `createUploadLink` description, fixing [#247](https://github.com/jaydenseric/apollo-upload-client/issues/247).
- Fixed function `createUploadLink` option `fetchOptions.signal` bugs:
  - If the given abort controller signal is already aborted, immediately abort the fetch.
  - Use `once: true` when adding the `abort` event listener on the given abort controller signal to avoid a possible memory leak.
- Updated a URL in the changelog entry for v14.0.0.
- Documentation updates.
- The file `changelog.md` is no longer published.

## 14.1.3

### Patch

- Removed the [`subscriptions-transport-ws`](https://npm.im/subscriptions-transport-ws) peer dependency, via [#235](https://github.com/jaydenseric/apollo-upload-client/pull/235).
- Updated dependencies.
- Also run GitHub Actions with Node.js v15.
- Updated tests to account for the `AbortController` global being defined in Node.js v15+.

## 14.1.2

### Patch

- Updated dependencies.
- Lint fixes for updated Prettier.
- Rewrote the tests to use `execute` from [`apollo-link`](https://npm.im/apollo-link) instead of `ApolloClient` `query` and `mutate` methods.
- Ensure the Apollo Link observable terminates with an error when there are both errors and data, fixing [#222](https://github.com/jaydenseric/apollo-upload-client/issues/222).

## 14.1.1

### Patch

- Use [`revertable-globals`](https://npm.im/revertable-globals) for tests.
- Removed no longer necessary [`formdata-node`](https://npm.im/formdata-node) workarounds in tests.
- Removed `npm-debug.log` from the `.gitignore` file as npm [v4.2.0](https://github.com/npm/npm/releases/tag/v4.2.0)+ doesn’t create it in the current working directory.
- Support `clientAwareness` being undefined in Apollo Link context, via [#212](https://github.com/jaydenseric/apollo-upload-client/pull/212).

## 14.1.0

### Minor

- Support GET requests, fixing [#151](https://github.com/jaydenseric/apollo-upload-client/issues/151).

### Patch

- Updated the [`extract-files`](https://npm.im/extract-files) dependency to v9, updating relevant deep require paths.
- Added API tests, fixing [#204](https://github.com/jaydenseric/apollo-upload-client/issues/204).
- Properly support the `signal` fetch option, fixing [#209](https://github.com/jaydenseric/apollo-upload-client/issues/209).
- Updated `createUploadLink`:
  - Alphabetically sorted destructured imports.
  - Removed a redundant fallback value when destructuring `clientAwareness` from context. It was an obstacle to 100% code coverage because `ApolloClient` defaults it to an empty object.
  - Unnested some code from the `Observable` function scope.
  - Fixed the JSDoc default value type for `options.uri`.
  - Improved code comments.
- Replaced references to “Apollo Graph Manager” with “Apollo Studio” and updated related URLs.
- Better npm link in the readme setup instructions.

## 14.0.1

### Patch

- Use deep [`@apollo/client`](https://npm.im/@apollo/client) imports to support non React projects, fixing [#207](https://github.com/jaydenseric/apollo-upload-client/issues/207).

## 14.0.0

### Major

- Updated Node.js support to `^10.17.0 || ^12.0.0 || >= 13.7.0`.
- Updated dependencies, some of which (only dev dependencies) require newer Node.js versions than previously supported.
- Added a package [`exports`](https://nodejs.org/api/packages.html#packages_exports) field with [conditional exports](https://nodejs.org/api/packages.html#packages_conditional_exports) to support native ESM in Node.js and keep internal code private, whilst avoiding the [dual package hazard](https://nodejs.org/api/packages.html#packages_dual_package_hazard). Published files have been reorganized, so previously undocumented deep imports will need to be rewritten according to the newly documented paths.
- Support [`@apollo/client`](https://npm.im/@apollo/client) v3, fixing [#174](https://github.com/jaydenseric/apollo-upload-client/issues/174) via [#175](https://github.com/jaydenseric/apollo-upload-client/pull/175/files).

### Patch

- Added the [`graphql`](https://npm.im/graphql) peer dependency to support a wider range of package managers, via [#196](https://github.com/jaydenseric/apollo-upload-client/pull/196).
- Removed Node.js v13 and added v14 to the versions tested in GitHub Actions.
- Simplified the GitHub Actions CI config with the [`npm install-test`](https://docs.npmjs.com/cli/v7/commands/npm-install-test) command.
- Use Babel config `overrides` to ensure `.js` files are parsed as scripts, eliminating Babel `interopRequireDefault` helpers from transpilation output.
- Prettier code examples in source JSDoc.
- Improved the type `ReactNativeFileSubstitute` code example.
- Updated EditorConfig.
- Improved the documentation about gotchas when inspecting network requests in React Native, via [#193](https://github.com/jaydenseric/apollo-upload-client/pull/193).
- Changed code examples to use `import` instead of `require`.

## 13.0.0

### Major

- Updated Node.js support from v8.10+ to v10+.
- Updated dependencies, some of which require Node.js v10+.

### Minor

- Support uploading files from a server environment, fixing [#172](https://github.com/jaydenseric/apollo-upload-client/issues/172) via [#179](https://github.com/jaydenseric/apollo-upload-client/pull/179) and [#184](https://github.com/jaydenseric/apollo-upload-client/pull/184).
  - Added `createUploadLink` options:
    - `isExtractableFile`
    - `FormData`
    - `formDataAppendFile`
  - Added exports for the new `createUploadLink` option defaults:
    - `isExtractableFile`
    - `formDataAppendFile`

### Patch

- Removed the now redundant [`eslint-plugin-import-order-alphabetical`](https://npm.im/eslint-plugin-import-order-alphabetical) dev dependency.
- Added a [`size-limit`](https://npm.im/size-limit) dev dependency.
- Stop using [`husky`](https://npm.im/husky) and [`lint-staged`](https://npm.im/lint-staged).
- Ensure GitHub Actions CI runs for pull requests.
- Use strict mode for scripts.
- Move Babel config from `babel.config.js` to `src/.babelrc.json`.
- Improved the package `prepare:prettier` and `test:prettier` scripts.
- Configured Prettier option `semi` to the default, `true`.
- Removed `package-lock.json` from `.gitignore` and `.prettierignore` as it’s disabled in `.npmrc` anyway.
- Updated external documentation link URLs.
- Replaced “Apollo Engine” with “Apollo Graph Manager” in comments.
- Improved the examples in the readme.

## 12.1.0

### Minor

- Setup [GitHub Sponsors funding](https://github.com/sponsors/jaydenseric):
  - Added `.github/funding.yml` to display a sponsor button in GitHub.
  - Added a `package.json` `funding` field to enable npm CLI funding features.

## 12.0.0

### Major

- Updated Node.js support from v8.5+ to v8.10+, to match what the [`eslint`](https://npm.im/eslint) dev dependency now supports. This is unlikely to be a breaking change for the published package.

### Patch

- Updated dev dependencies.
- Added the [`eslint-plugin-jsdoc`](https://npm.im/eslint-plugin-jsdoc) dev dependency.
- Replaced the [`size-limit`](https://npm.im/size-limit) dev dependency with [`@size-limit/preset-small-lib`](https://npm.im/@size-limit/preset-small-lib).
- Only create a default [`AbortController`](https://developer.mozilla.org/en-US/docs/Web/API/AbortController) instance if `signal` is not already set in fetch options, fixing [#162](https://github.com/jaydenseric/apollo-upload-client/issues/162) via [#169](https://github.com/jaydenseric/apollo-upload-client/pull/169).
- Use GitHub Actions instead of Travis for CI.
- Clarified that Opera Mini isn’t supported in the Browserslist queries and readme “Support” section.
- Documented polyfills to consider in the readme “Support” section.
- Updated examples to use [`@apollo/react-hooks`](https://npm.im/@apollo/react-hooks).

## 11.0.0

### Major

- Updated Node.js support from v6+ to v8.5+.

### Minor

- Support [Apollo Studio client awareness](https://apollographql.com/docs/studio/client-awareness), via [#143](https://github.com/jaydenseric/apollo-upload-client/pull/143).

### Patch

- Updated dependencies.
- Ensure Babel helpers are imported and not inlined, using the [`@babel/runtime`](https://npm.im/@babel/runtime) dependency and [`@babel/plugin-transform-runtime`](https://npm.im/@babel/plugin-transform-runtime) dev dependency.
- Nicer Browserslist syntax for supported Node.js versions.

## 10.0.1

### Patch

- Updated dependencies.
- Reduced the size of the published `package.json` by moving dev tool config to files. This also prevents editor extensions such as Prettier and ESLint from detecting config and attempting to operate when opening package files installed in `node_modules`.
- Simplified the `prepublishOnly` script.
- Add tips for React Native gotchas, via [#135](https://github.com/jaydenseric/apollo-upload-client/pull/135).
- Updated the package description to mention that the upload link is terminating and clarified in the setup instructions that there can only be 1 terminating Apollo Link, via [#147](https://github.com/jaydenseric/apollo-upload-client/pull/147).
- Improve setup instructions for Apollo Boost.

## 10.0.0

### Major

- Updated the [`extract-files`](https://npm.im/extract-files) dependency to v5:
  - The original operation object is no longer modified when it contains files, fixing [#81](https://github.com/jaydenseric/apollo-upload-client/issues/81).
  - If the same file is used in multiple locations of an operation it is only uploaded once.

### Patch

- Updated dependencies.
- Updated package scripts and config for the new [`husky`](https://npm.im/husky) version.
- Updated Node.js and browser support documentation in the readme.
- Use [jsDelivr](https://jsdelivr.com) for the readme logo instead of [RawGit](https://rawgit.com) as they are shutting down.

## 9.1.0

### Minor

- Support more browsers by changing the [Browserslist](https://github.com/browserslist/browserslist) query from [`> 1%`](https://browserl.ist/?q=%3E+1%25) to [`> 0.5%, not dead`](https://browserl.ist/?q=%3E+0.5%25%2C+not+dead).

### Patch

- Updated dev dependencies.
- Fix Babel not reading from the package `browserslist` field due to [a sneaky `@babel/preset-env` breaking change](https://github.com/babel/babel/pull/8509), fixing [#124](https://github.com/jaydenseric/apollo-upload-client/issues/124).

## 9.0.0

### Major

- Made [`apollo-link`](https://npm.im/apollo-link) a dependency, instead of a peer dependency.
- Removed the package `module` entry and the "ESM" build, which was `.js` and not proper native ESM for Node.js via `.js` as Apollo dependencies don’t support it.

### Minor

- Updated Babel, removing the [`@babel/runtime`](https://npm.im/@babel/runtime) dependency.
- Package [marked side-effect free](https://webpack.js.org/guides/tree-shaking#mark-the-file-as-side-effect-free) for bundlers and tree-shaking.

### Patch

- Updated dependencies.
- Use the new [`extract-files`](https://npm.im/extract-files) API.
- Use [`jsdoc-md`](https://npm.im/jsdoc-md) to generate readme API docs from source JSDoc, which has been improved.
- Readme examples updated to use the [`react-apollo`](https://npm.im/react-apollo) `Mutation` component instead of the `graphql` decorator.
- Readme examples use CJS instead of ESM as this project does not support native ESM (due to a lack of support in Apollo dependencies) and we shouldn’t assume everyone uses Babel.
- Updated package description.
- Added package tags.
- Added a package `test:size` script, using [`size-limit`](https://npm.im/size-limit) to guarantee < 1 KB CJS bundle sizes.
- Lint `.yml` files.
- Refactored package scripts and removed the [`npm-run-all`](https://npm.im/npm-run-all) dev dependency.
- Removed a temporary workaround for [a fixed Babel CLI bug](https://github.com/babel/babel/issues/8077).
- Ensure the readme Travis build status badge only tracks `master` branch.
- Use [Badgen](https://badgen.net) for the readme npm version badge.

## 8.1.0

- Updated dependencies.
- Use `.prettierignore` to defer `package.json` formatting to npm.
- Renamed `.babelrc.js` to `babel.config.js` and simplified ESLint ignore config.
- Improved linting with [`eslint-config-env`](https://npm.im/eslint-config-env).
- Use the `.js` extension for source.
- Added JSDoc comments to source.
- Refactored package scripts:
  - Use `prepare` to support installation via Git (e.g. `npm install jaydenseric/apollo-upload-client`).
  - Remove `rimraf` and `cross-env` dev dependencies. Only \*nix environments will be supported for contributing.
  - Removed `watch` and `fix` scripts.
- Compact package `repository` field.
- Setup Travis CI.
- Readme badge changes to deal with [shields.io](https://shields.io) unreliability:
  - Removed the licence badge. The licence can be found in `package.json` and rarely changes.
  - Removed the Github issues and stars badges. The readme is most viewed on Github anyway.
  - Added the more reliable build status badge provided by Travis and placed it first as it loads the quickest.

## 8.0.0

- Abandon `.js` [until Apollo provides native ESM](https://github.com/apollographql/apollo-link/issues/537), fixing [#72](https://github.com/jaydenseric/apollo-upload-client/issues/72).
- New readme logo URL that doesn’t need to be updated every version.

## 7.1.0

- Updated dependencies.
- Stop using named imports from CJS dependencies in ESM, fixing [#72](https://github.com/jaydenseric/apollo-upload-client/issues/72).
- Match an [error handling tweak](https://github.com/apollographql/apollo-link/pull/524/files#diff-f34b7d587510bfca51f1dd1dd03dfc98R130) in the official HTTP links.

## 7.1.0-alpha.2

- Updated dependencies.
- Using new `apollo-link-http-common` API.
- Corrected aborting fetch, fixing [#70](https://github.com/jaydenseric/apollo-upload-client/issues/70).

## 7.1.0-alpha.1

- Updated dependencies.
- Using [`apollo-link-http-common`](https://npm.im/apollo-link-http-common) for commonality with the official HTTP links:
  - Removed `graphql` peer dependency.
  - Aborting `fetch` supported.
  - Fixes [#47](https://github.com/jaydenseric/apollo-upload-client/issues/47) and [#61](https://github.com/jaydenseric/apollo-upload-client/issues/61).
- More robust npm scripts.
- HTTPS `package.json` author URL.

## 7.0.0-alpha.4

- Updated dependencies.
- Added support for [`Blob`](https://developer.mozilla.org/en/docs/Web/API/Blob) types, via [#58](https://github.com/jaydenseric/apollo-upload-client/pull/58).
- Readme updates:
  - Added a [GraphQL multipart request spec server implementation list](https://github.com/jaydenseric/graphql-multipart-request-spec#server) link to the intro.
  - Misc. tweaks.

## 7.0.0-alpha.3

- Updated dependencies.
  - [`extract-files` v3](https://github.com/jaydenseric/extract-files/releases/tag/v3.0.0) replaces files extracted from properties with `null` instead of deleting the property; see [jaydenseric/extract-files#4](https://github.com/jaydenseric/extract-files/issues/4). This improves compliance with the [GraphQL multipart request spec](https://github.com/jaydenseric/graphql-multipart-request-spec). It’s not a breaking change for `apollo-upload-server`, but it might be for other implementations.

## 7.0.0-alpha.2

- Updated dependencies.
- Updated peer dependencies to support `graphql@0.12`.
- Added a clean step to builds.
- Smarter Babel config with `.babelrc.js`.
- Modular project structure that works better for native ESM.
- Target Node.js v6.10+ for transpilation and polyfills via `package.json` `engines`, matching the version supported by [`apollo-upload-server`](https://github.com/jaydenseric/apollo-upload-server).
- Support [browsers with >1% global usage](http://browserl.ist/?q=%3E1%25) (was >2%).
- Updated the readme support section.

## 7.0.0-alpha.1

- Conform to the [GraphQL multipart request spec v2.0.0-alpha.2](https://github.com/jaydenseric/graphql-multipart-request-spec/releases/tag/v2.0.0-alpha.2).
- Don’t set empty request `operationName` or `variables`.

## 6.0.3

- Response is set on the context, via [#40](https://github.com/jaydenseric/apollo-upload-client/pull/40).
- Configured `lint-staged` for `.js`.

## 6.0.2

- Fix broken exports. See [babel/babel#6805](https://github.com/babel/babel/issues/6805).

## 6.0.1

- Updated dependencies. Fixes broken `core-js` imports due to `@babel/polyfill@7.0.0-beta.32`.
- Configured prettier to no longer hard wrap markdown prose.
- Fixed an Apollo link in the readme.
- Misc. readme and changelog typo fixes.
- Externally host the readme logo again to fix display in npm. See [npm/www#272](https://github.com/npm/www/issues/272).

## 6.0.0

- Updated `prettier`.
- No longer publish the `src` directory.
- Readme API documentation fixes:
  - Corrected React Native example code import, via [#39](https://github.com/jaydenseric/apollo-upload-client/pull/39).
  - Updated `createUploadLink` options.

## 6.0.0-beta.3

- Corrected network error handling, fixing [#38](https://github.com/jaydenseric/apollo-upload-client/issues/38).

## 6.0.0-beta.2

- Match the `apollo-link-http` API and support setting `credentials` and `headers` directly on the link and via context, fixing [#36](https://github.com/jaydenseric/apollo-upload-client/issues/36).
- Fixed [a bug](https://github.com/jaydenseric/apollo-upload-client/pull/37#issuecomment-343005839) that can cause the wrong `content-type: application/json` header to be used when uploading.
- Updated changelog Apollo documentation links.
- changelog is now prettier.

## 6.0.0-beta.1

- Updated dependencies.
- Apollo Client v2 compatibility:
  - Export a terminating Apollo Link instead of custom network interfaces.
  - New `apollo-link` and `graphql` peer dependencies.
- Rejigged package scripts.
- Updated Prettier and ESLint config.
- Lint errors when attempting to commit partially staged files no longer commits the whole file.
- Using Babel v7 directly instead of Rollup.
- Using `babel-preset-env` to handle polyfills so only required ones are included for our level of browser support.
- Using `prettier` to format distribution code as well as source code.
- No more source maps, as Prettier does not support them.
- Renamed `dist` directory to `lib`.
- Module files now have `.js` extension.
- Removed `babel-eslint` as the vanilla parser works fine.
- Readme improvements:
  - Relative logo path.
  - Added links to badges.
  - Simplified code examples.
  - Mark relevant example code blocks as JSX instead of JS.
  - Removed the inspiration links; they are less relevant to the evolved codebase.

## 5.1.1

- Updated dependencies.
- Readme fixes:
  - Fixed usage example code for `ReactNativeFile.list`.
  - Fixed capitalization of ‘npm’.

## 5.1.0

- Updated dependencies.
- Readme tweaks including a new licence badge.
- Fixed Rollup build warnings.
- Fixed an npm v5 warning by using `prepublishOnly` instead of `prepublish`.
- Refactored network interfaces; moved file extraction logic and `ReactNativeFile` to a separate [`extract-files`](https://npm.im/extract-files) package.

## 5.0.0

- Removed `package-lock.json`. Lockfiles are [not recommended](https://github.com/sindresorhus/ama/issues/479#issuecomment-310661514) for packages.
- Readme tweaks and fixes:
  - Renamed the `File` input type `Upload` for clarity.
  - Wording and formatting improvements.

## 5.0.0-alpha.1

- Updated dependencies.
- Simplified React Native setup by moving Babel config out of `package.json`, fixing [#19](https://github.com/jaydenseric/apollo-upload-client/issues/19) via [#23](https://github.com/jaydenseric/apollo-upload-client/pull/23).
- Export a new `ReactNativeFile` class to more reliably identify files for upload in React Native, via [#17](https://github.com/jaydenseric/apollo-upload-client/pull/17).
- Renamed several exports for consistency with `apollo-client`, via [#18](https://github.com/jaydenseric/apollo-upload-client/pull/18).
  - `HTTPUploadNetworkInterface` renamed `UploadHTTPFetchNetworkInterface`.
  - `HTTPUploadBatchNetworkInterface` renamed `UploadHTTPBatchedNetworkInterface`.
  - `createBatchNetworkInterface` renamed `createBatchingNetworkInterface`.

## 4.1.1

- Updated dependencies.
- Compatibility changes for `apollo-client@1.5.0`:
  - Prevent a query batching error caused by an API change, fixing [#20](https://github.com/jaydenseric/apollo-upload-client/issues/20).
  - Support the new [`batchMax`](https://github.com/apollographql/core-docs/pull/302/files) option in `createBatchNetworkInterface`.

## 4.1.0

- Documented React Native.

## 4.1.0-alpha.2

- Fixed error when `File` and `FileList` are undefined globals in React Native, see [comment](https://github.com/jaydenseric/apollo-upload-client/issues/10#issuecomment-310336487).

## 4.1.0-alpha.1

- Support React Native, fixing [#10](https://github.com/jaydenseric/apollo-upload-client/issues/10).

## 4.0.7

- Prevent error caused by `null` values in query/mutation variables, fixing [#15](https://github.com/jaydenseric/apollo-upload-client/issues/15).

## 4.0.6

- Corrected `package-lock.json`.
- Source comment typo fix.

## 4.0.5

- Removed 2 dependencies by refactoring `extractRequestFiles` with bespoke recursion logic, shaving several KB off the bundle size and fixing [#13](https://github.com/jaydenseric/apollo-upload-client/issues/13).

## 4.0.4

- Updated dependencies.
- Added a changelog.
- Dropped Yarn in favor of npm@5. Removed `yarn.lock` and updated install instructions.
- New ESLint config. Dropped [Standard Style](https://standardjs.com) and began using [Prettier](https://github.com/prettier/eslint-plugin-prettier).
- Using [lint-staged](https://github.com/okonet/lint-staged) to ensure contributors don't commit lint errors.
- Removed `build:watch` script. Use `npm run build -- --watch` directly.

## 4.0.3

- Updated dependencies.
- Fixed fetch options not applying correctly, see [#9](https://github.com/jaydenseric/apollo-upload-client/pull/9).

## 4.0.2

- Updated readme examples:
  - Removed `PropTypes`. React no longer exports them and they are a distraction anyway.
  - Importing `gql` from `react-apollo`.
  - No longer using decorator syntax.
  - Using functional components in place of classes.

## 4.0.1

- Updated dependencies.
- No longer errors when network interface `opts` are not configured, fixing [#8](https://github.com/jaydenseric/apollo-upload-client/issues/8).
- Fixed the batch network interface always thinking there are files to upload, preventing the use of the fallback vanilla Apollo transport method when there are none.
- Simplified Babel config.

## 4.0.0

- Corrected the API for configuring fetch options, fixing [#6](https://github.com/jaydenseric/apollo-upload-client/issues/6) ([#7](https://github.com/jaydenseric/apollo-upload-client/pull/7)).

## 3.0.3

- The `extractRequestFiles` helper no longer converts the query AST to string as a side-effect, fixing [#5](https://github.com/jaydenseric/apollo-upload-client/issues/5).

## 3.0.2

- Updated dependencies.
- Fall back to regular network interface fetch methods if SSR or no files to upload, fixing [#3](https://github.com/jaydenseric/apollo-upload-client/issues/3).

## 3.0.1

- Better transpilation with `babel-runtime`. This should improve IE 11 support.

## 3.0.0

- Support `apollo-upload-server` v2 and [query batching](https://apollographql.com/docs/apollo-server/requests/#batching).
- Removed the seemingly redundant `Accept` header from requests.
- Clearer package description.

## 2.0.2

- Updated dependencies.
- Internal refactor for a cleaner ES6 class extension and method override.

## 2.0.1

- Removed two unversioned files prematurely published to npm.

## 2.0.0

- Updated dependencies.
- New API:
  - Now exporting the custom network interface, which has been renamed `HTTPUploadNetworkInterface`. This enables it to be extended externally.
  - In preparation for adding another batched network interface, `createNetworkInterface` is now a named and not default export.
- Fixed the `uri` argument for `createNetworkInterface` ending up in the request options.
- Internally simplified `apollo-client` imports.
- Simpler linting setup.

## 1.0.2

- Fixed broken Github deep links in the readme.
- Readme rewording.
- Simplified package.json description.

## 1.0.1

- Added missing metadata to `package.json`.

## 1.0.0

- Initial release.
