# rollup-watch-bug

> A repo demonstrating an issue with the watch functionality of [rollup.js](https://rollupjs.org/)
>

1. Clone the repo
2. Install dependencies: `npm i`
3. Run `npm watch:js`
   - **Description**: The `build:js` script is executed, running rollup in watch mode.
   - **Expected**: Modifying files in `src/js` causes rollup to recompile.
   - **Actual**: Works as expected.
4. Run `npm watch-bug`
   - **Description**: The `build:js` and `build:pcss` scripts are run in parallel, each in watch mode.
   - **Expected**: Modifying files in the `src` will cause the appropriate task to recompile.
   - **Actual**: Both tasks complete their initial run. `build:js` fails to recompile when changes are made to `src/js` files. `build:pcss` functions as expected.
5. Run `npm watch-fix`
   - **Description**: The `build:pcss` and `build:js` scripts are run in parallel (reverse order from `watch-bug` script), each in watch mode.
   - **Expected**: Modifying files in the `src` will cause the appropriate task to recompile.
   - **Actual**: Works as expected.

**Summary:** The order in which parallel scripts are executed matters with rollup (but it shouldn't). This simple test demonstrates that unless rollup is executed last, it is unable to watch files as expected.

It's worth mentioning that in this simple scenario rollup was able to complete its initial compilation regardless of order. In projects with more complex rollup configurations or several parallel tasks being started, rollup will often fail to recompile on the intial run as well. This appears to happen when rollup is unable to finish compiling before being interrupted by another parallel task.
