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


Using webpack.copnfig.js : As for now we have written build scripts in package.json file which is good if we have basic configuration, but when project goes large and we need some other files too then we can use webpack.config.js and we write configuration about webpack and webpack server in this config file.
add a file "webpack.copnfig.js" in project root folder and add below lines in this.

``` javascript

    var path = require('path');

    module.exports = {
        entry: './src/js/app.js',
        output: {
            path: path.resolve(__dirname, 'dist'),
            filename: 'bundle.js',
            publicPath: '/dist'
        },
    };

```

here config object is exported. and this is basic config.
- entry : entry point of app.
- output.path : the output path of app.
- output.filename : filename of output app file.
- output.publicPath : directory for all assest in app/project.
- var path : it is pre-defined package comes with node_modules which provides various functions regarding directory.

Now again run command "npm run build" and see if its working.

So project is getting its js file from bundle.js built by webpack. Now we will add css in project using webpack. To do this we need some packages installed. "css-loader" and "style-loader"

- npm install css-loader style-loader --save-dev 

these loaders load css file and bundle them with exported js file.

Edit webpack.config.js file for importing css using these loaders.
Add below object after output key.
``` javascript
    module: {
        rules: [{
            test: /\.css$/,
            use: [
                'style-loader',
                'css-loader'
            ]
        }]
    }
```


Loaders : loader do the job of loading different extensions files like scss, css, js, html etc. Webpack loads the last defined loader first then the first one (In Array form). 

> Note : webpack loads its loader from last to first in order. For eg. webpack will first load "css-loader" and then "style-loader".

We can also minify/uglify our js for better performance and optimization.

- npm i -D --save-dev uglifyjs-webpack-plugin

init UglifyJsPlugin variable in webpack.config.js

``` javascript
    const UglifyJsPlugin = require('uglifyjs-webpack-plugin');
```

and after "module" key add below lines

``` javascript
     plugins: [
        new UglifyJsPlugin()
    ]
```

Using SCSS/SASS file in project 
We can also use scss files for our css styles and import them in project. and For export CSSs in seperate file we need extract-text plugin. For this install below package

- npm install babel-core babel-loader babel-preset-env sass-loader node-sass scss-loader extract-text-webpack-plugin --save-dev

Then initialize these loader in rules key in webpack.config.js

``` javascript
    var ExtractTextPlugins = require('extract-text-webpack-plugin');

    var extractPlugin = new ExtractTextPlugins({
        filename: 'main.css'
    });
```

``` javascript
    {
        test: /\.scss$/,
        use: extractPlugin.extract({
            use: [
                'css-loader',
                'sass-loader'
            ]
        })
    }
```

``` javascript
    plugins: [
        extractPlugin,
        new UglifyJsPlugin()
    ]
```

and now import "main.scss" in app.js file and run command "npm run build"