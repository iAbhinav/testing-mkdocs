# Adding SSR to Nx Workspace

> Folowing From [here](https://blog.nrwl.io/server-side-rendering-ssr-with-angular-for-nx-workspaces-14e2414ca532)


* Install dependencies
```
npm i  @nguniversal/express-engine
npm i --savedev @nguniversal/builders
```

* Add SSR schematics to your app

```
nx generate @schematics/angular:universal --project=[PROJECT NAME]
```

This will do the following

```
CREATE apps/[PROJECT NAME]/src/main.server.ts
CREATE apps/[PROJECT NAME]/src/app/app.server.module.ts
CREATE apps/[PROJECT NAME]/tsconfig.server.json
UPDATE package.json
UPDATE angular.json
UPDATE apps/[PROJECT NAME]/src/main.ts
UPDATE apps/[PROJECT NAME]/src/app/app.module.ts
```

* Follow steps from [here](https://blog.nrwl.io/server-side-rendering-ssr-with-angular-for-nx-workspaces-14e2414ca532)
  * Create a new file [PROJECT]/src/ssr.server.ts
  * Paste the code from the link
  * Open angular.json
  * and Create a new target "serve-ssr"
  ```
   "serve-ssr": {
          "builder": "@nguniversal/builders:ssr-dev-server",
          "configurations": {
            "development": {
              "browserTarget": "softrade:build:development",
              "serverTarget": "softrade:server:development"
            },
            "production": {
              "browserTarget": "softrade:build:production",
              "serverTarget": "softrade:server:production"
            }
          },
          "defaultConfiguration": "development"
        }
  ```
  > Had to use `"builder"` instead of executor as suggested in the article
  
  > `"server"` target is made while adding the schematic

* Build SSR Server

```
 nx server [PROJECT NAME]
 ```

* Serve the app

```
nx serve-ssr [PROJECT-NAME]
```
