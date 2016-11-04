# Angular2 Webpack With Arcgis
   this repo is a demo to show how to load arcgis js api based on Angular2 Webpack Starter

   thanks for @tomwayson and @hassanqaiser.

# Angular2 Webpack Starter (https://github.com/AngularClass/angular2-webpack-starter)



# License
 [MIT](/LICENSE)

# steps of loading arcgis js api

1. npm install @types/arcgis-js-api
2. npm install @types/dojo


3. Remove the following script from index.html
  <% if (webpackConfig.metadata.isDevServer && webpackConfig.metadata.HMR !== true) { %>
  <!-- Webpack Dev Server reload -->
  <script src="/webpack-dev-server.js"></script>
  <% } %>

4. Add the following in the index.html

  ```javascript  
   <script>
    window.dojoConfig = {
      async: true,
      isDebug: true
    };
  </script>
  <script src="https://js.arcgis.com/3.18/"></script>
  <script>
    setTimeout(function(){ 
      require([
        "polyfills.bundle.js", 
        "vendor.bundle.js",
        "main.bundle.js"
      ], function (polyfills, vendor, main) {}); 
    }, 4000);
    
  </script>  
  
  ```

5. Update webpack.common.js
   ```javascript  
      new HtmlWebpackPlugin({
        template: 'src/index.html',
        chunks: [''],
        excludeChunks: ['polyfills', 'vendor', 'main']
      }),
   ```

6. Add in webpack.common.js
   ```javascript  
      externals: [
       
        function(context, request, callback) {
            if (/^dojo/.test(request) ||
                /^dojox/.test(request) ||
                /^dijit/.test(request) ||
                /^esri/.test(request)
            ) {
                return callback(null, "amd " + request);
            }
            callback();
        }
    ],
    devtool: 'source-map'
   ```
   
7. In webpack.dev.js

      // library: 'ac_[name]',
      libraryTarget: 'umd',    
