I started by creating a directory and then used **yarn** as a package manager inside the folder/directory to start the application.

- `yarn init`

This came with various options after this command, I left blank/defaults to all of it.
Then, I installed the followings as dev dependencies:

- webpack
- webpack-cli
- webpack-dev-server

with the command: `yarn add -D webpack webpack-cli webpack-dev-server`

As I installed these, now I can create **webpack.config.js** to configure the webpack.

1. I imported `path` to get the path of the config file
2. The **entry** key specifies where to start the application. I mentioned **main.js** file as the entry point to start the application.
3. The **output** key would specify where the build file would reside after webpack makes the bundle. The keys inside it are self explanatory.
4. The **devServer** key will be used by **webpack-dev-server** while developing the application to serve the file. Similarly, the key inside of it are self explanatory. It will serve the static file in a local server.
5. The **mode** key is required as it gives error and fallback to production if not specified.
6. Now, adding a **script** key in the **package.json** file and adding `"dev": "webpack serve"` will help us in serving the file locally through command.
7. Before, starting the server, add a **main.js** file as per the path specified in **step #2**. Write something, a console.log would suffice.
8. Add an **index.html** file inside the **dist** folder as per **step #3**. In that html file, add a script tag with `src="./bundle.js"`. This is the file which will be made by webpack and served through server using the html file.

Until this... a simple module bundler with webpack and can be served using html file where the bundled file can be attached.

Now, it's time fo **React** to introduce.
For, React to work we need **babel**, which is a transpiler that transpiles **newer JS to older JS** which the Browser can understand. Also, **JSX to normal JS** bcoz this is also need to be understood by Browser. As the **import** statements cannot be understood by Browser. For this, I installed these:

- **@babel/core**: This is the main library which transpiles the code.
- **babel-loader**: This is used to attach the library with webpack. It helps webpack to recognize babel and use it to transpile. You can say it as a interface between babel and webpack
- **@babel/preset-react**: This is used to convert JSX to JS

After this we need to configure babel with webpack in **webpack.config.js**. We need to configure **babel-loader** with webpack. We can do it by adding it to the **rules** in the **module** of the webpack config file.

- We can specify which files are can be used by webpack to transpile using babel through the **test** key which uses regular expression to filter the files that are ending with **.js or .jsx**.
- **exclude** key is self explanatory, as to where we do not need to use babel for transpiling
- **use** key is also self explanatory as to use this loader to communicate with babel for transpiling
- **resolve** is used to remove the extensions used while importing files using **import** statement like rather than using `App.jsx`, we can use `App` only while importing.

We have also to let know the babel that we need to transpile JSX to JS. And for that we had to add **@babel/preset-react** in the **babel.config.json** file as Babel will only convert the newer JS to older one unless we tell it to convert JSX to JS as well. And, we can do that by adding this package to the **presets** key in the config.json file of babel.

Now, we can write the basic React imports in **main.js** file and import **App.jsx** in it. The same old React imports. The only thing that we are replacing is the **index.js** file that every React app have with **main.js**. Because we have defined main.js to be the entry point using webpack. If we change it to index.js also, it would be fine. Now, we can run the React app succesfully using Webpack and Babel.

As we are going to write in the new version of JS, specifically ES6+ code, we need to convert it to older version of JS(ES5-). As some of the older browsers may not support. So, I added the following packages:

- **@babel/preset-env**: Most of the polyfills are supported by this package, which means most of the new JS code can be converted by this.
- **core-js**: This is also a polyfill generator which converts newer js. This is added for additional support as the above package supports most of the newer JS but not all so, whichever is missed by it, this package may have that. If both of them does not have polyfill for some code then we have to depend on some plugins.
- **regenerator-runtime**: This is for Promise

We can add configurations in the **babel.config.json** file for these packages.
