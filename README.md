Webpack : webpack is used to bundle assets in one or more file. we can create different modules and then add them in one file and serve to server.

Installtion & Setup : 
First create a folder for project then add src folder in it. add some css and js files in src. Make a index.html relative to src folder. Then follow below commands.

- Init npm : npm init
- Installing webpack : npm install webpack --save-dev

Then add below lines in package.json's "scripts" key/ These lines will init a build script that tells webpack get app.js and build it to bundle.js in dist folder. dist folder is generated automatically by webpack.

``` json
    "scripts": {
        "build": "webpack src/js/app.js dist/bundle.js",
        "build:prod": "webpack src/js/app.js dist/bundle.js -p"
    },
```

- Run webpack build : npm run build

If all goes well then webpack will build your app and contents of dist folder will be created. "build:prod" is same as "build" but it creates production build and will minify your bundle.js and other assests like css, scss etc.

Now add "dist/bundle.js" in index.html file and run this file in browser, now you will see that contens are getting loaded from "bundle.js"

This file is getting served from file protocol not from http protocol now. so we need to add server who will serve these files.
Here we will install webpack-dev-server.

- npm install --save-dev webpack-dev-server

Now edit package.json

``` json
    "scripts": {
        "build": "webpack-dev-server --entry ./src/js/app.js --output-filename ./dist/bundle.js",
        "build:prod": "webpack src/js/app.js dist/bundle.js -p"
    },
```

Now again run "npm run build". This time you will se a message in terminal/cmd "webpack: Compiled successfully." and Project is running at http://localhost:8080. Copy this line and fire in browser's address bar. Then you see project running same as previous but this time it is getting served from a server and if you check in network tab of chrome dev tool/firebug you can see all files are loading and their sizes are showing up.






Loaders : loader do the job of loading different extensions files like scss, css, js, html etc. Webpack loads the last defined loader first then the first one (In Array form). 

babel-loader
node-sass
css-loader
sass-loader
style-loader
babel-core
babel-preset-es2015