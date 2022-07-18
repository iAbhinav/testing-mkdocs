# Adding SSR to Nx Workspace

> Folowing From [here](https://blog.nrwl.io/server-side-rendering-ssr-with-angular-for-nx-workspaces-14e2414ca532)


* Create a new project 

```
nx generate @nxtend/ionic-angular:application
```

> Or use an existing Ionic Angular Project


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


* Build server dist using

```
 nx server [PROJECT NAME]
```

* Build browser dist

```
nx build [PROJECT NAME]
```


## Serve the SSR Project

* Follow steps from [here](https://blog.nrwl.io/server-side-rendering-ssr-with-angular-for-nx-workspaces-14e2414ca532)
  * Create a new file [PROJECT]/server.ts


```
import 'zone.js/dist/zone-node';

import { ngExpressEngine } from '@nguniversal/express-engine';
import * as express from 'express';
import { join } from 'path';

import { AppServerModule } from './src/main.server';
import { APP_BASE_HREF } from '@angular/common';
import { existsSync } from 'fs';

// The Express app is exported so that it can be used by serverless Functions.
export function app(): express.Express {
    const server = express();
    const distFolder = join(process.cwd(), 'dist/[PROJECT NAME]/browser');
    const indexHtml = existsSync(join(distFolder, 'index.original.html')) ? 'index.original.html' : 'index';

    // Our Universal express-engine (found @ https://github.com/angular/universal/tree/master/modules/express-engine)
    server.engine('html', ngExpressEngine({
        bootstrap: AppServerModule,
    }));

    server.set('view engine', 'html');
    server.set('views', distFolder);

    // Example Express Rest API endpoints
    // server.get('/api/**', (req, res) => { });
    // Serve static files from /browser
    server.get('*.*', express.static(distFolder, {
        maxAge: '1y'
    }));

    // All regular routes use the Universal engine
    server.get('*', (req, res) => {
        res.render(indexHtml, { req, providers: [{ provide: APP_BASE_HREF, useValue: req.baseUrl }] });
    });

    return server;
}

function run(): void {
    const port = process.env['PORT'] || 4000;

    // Start up the Node server
    const server = app();
    server.listen(port, () => {
        console.log(`Node Express server listening on http://localhost:${port}`);
    });
}

// Webpack will replace 'require' with '__webpack_require__'
// '__non_webpack_require__' is a proxy to Node 'require'
// The below code is to ensure that the server is run only when not requiring the bundle.
declare const __non_webpack_require__: NodeRequire;
const mainModule = __non_webpack_require__.main;
const moduleFilename = mainModule && mainModule.filename || '';
if (moduleFilename === __filename || moduleFilename.includes('iisnode')) {
    run();
}

export * from './src/main.server';

```

> Change [PROJECT NAME]

  * Open angular.json

* In the "server" target change `"main": "apps/[PROJECT NAME]/src/main.server.ts",` to `"main": "apps/[PROJECT NAME]/server.ts",`

  * and Create a new target "serve-ssr"
  ```
   "serve-ssr": {
          "builder": "@nguniversal/builders:ssr-dev-server",
          "configurations": {
            "development": {
              "browserTarget": "[PROJECT NAME]:build:development",
              "serverTarget": "[PROJECT NAME]:server:development"
            },
            "production": {
              "browserTarget": "[PROJECT NAME]:build:production",
              "serverTarget": "[PROJECT NAME]:server:production"
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
 
* Host 

```
 node dist/[PROJECT NAME]/server/main.js 
```

> Check dist location from "ProjectName":"server" target in angular.json



* Serve the app using SSR

```
nx serve-ssr [PROJECT-NAME]
```


https://stackoverflow.com/a/65973247/3188225

