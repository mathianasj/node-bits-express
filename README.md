# node-bits-express
node-bits-express wraps the popular web framework [express](http://expressjs.com/). It iterates through the routes provided by other node-bits packages and exposes them as routes in express. It also exposes a couple of helpers to make setting up your server extremely simple.

## Install
```
npm install node-bits-express --save
```

or

```
yarn add node-bits-express
```

## Configuration
node-bits-express accepts an object to detail its configuration. Each property is detailed below.

```
import nodeBits, { GET,POST,PUT,DELETE,OPTIONS } from 'node-bits';
import nodeBitsExpress, { cors, bodyParser } from 'node-bits-express';

nodeBits([
  nodeBitsExpress({
    port: 3000,
    configurations: [cors({ methods: [GET,POST,PUT,DELETE,OPTIONS] }), bodyParser],
    hooks: []
  })
];
```

### Port
This is the port the server should listen on. Often this is expressed as the following example to allow runtime configuration:

```
port: process.env.port || 3000,
```

### Configurations
Configurations are simple functions that setup an attribute of the express server at boot. They are specified as to node-bits-express as items in an array.

```
configurations: [cors({ methods: [GET,POST,PUT,DELETE,OPTIONS] }), bodyParser],
```

The signature for these functions is: ``` (app, config) => {} ```. The first parameter is the express application. The second parameter is the overall config of node-bits.

node-bits-express exposes two configurations that are often used by most servers.
#### Cors
Cors uses the [cors](https://www.npmjs.com/package/cors) npm package. It accepts an object that matches the cors config you can find in their documentation. Most often it is simply passing in the methods to which you want cors to apply (as seen in the example above).

#### Body Parser
[body-parser](https://www.npmjs.com/package/body-parser) is a middleware for express that will parse the body of a http call and place it at req.body.

* note: body-parser does NOT handle multipart data. See their documentation for suggestions.

### Hooks
Hooks allow you to hook into a certain points in the server lifecycle. Right now the two options are BEFORE_CONFIGURE_ROUTES and AFTER_CONFIGURE_ROUTES.

The main use of the hook is the node-bits-jwt bit.
