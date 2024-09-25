# @ayan4m1/gatsby-plugin-root-import

Set Webpack to resolve modules and aliases, allowing you to import modules from an absolute project path rather than relative `../../` paths.

### Install

1. Install the `@ayan4m1/gatsby-plugin-root-import` plugin:

   `npm install --save-dev @ayan4m1/gatsby-plugin-root-import`

   or

   `yarn add --dev @ayan4m1/gatsby-plugin-root-import`

### Usage

Add into `gatsby-config.js`.

```javascript
// gatsby-config.js

module.exports = {
  plugins: ['@ayan4m1/gatsby-plugin-root-import']
};
```

### Default Settings

If no options are specified, the plugin allows you access to the `src` folder and also it's contents automatically.

This means you should be able to import modules like such:

```javascript
// importing gatsbyProject/src/someFolder/SomeComponent.js

import SomeComponent from 'src/someFolder/SomeComponent';
// or more succinctly
import SomeComponent from 'someFolder/SomeComponent';
```

### Plugin Options

Plugin Options allows you to:

1. Specify additional webpack [`resolve.modules`](https://webpack.js.org/configuration/resolve/#resolvemodules) search locations with the `resolveModules` key.
2. All other option keys will become a Webpack [`alias`](https://webpack.js.org/configuration/resolve/#resolvealias).

```javascript
// gatsby-config.js
const path = require('path');

module.exports = {
  plugins: [
    {
      resolve: '@ayan4m1/gatsby-plugin-root-import',
      options: {
        resolveModules: [path.join(__dirname, 'libs')],
        utils: path.join(__dirname, 'src', 'components', 'utilities')
      }
    }
  ]
};
```

This means you should be able to import modules like such:

```javascript
// gatsbyProject/libs is now a searched resolve.modules directory
// importing gatsbybProject/libs/someLib/SomeLibComponent.js
import SomeLibComponent from 'someLib/SomeLibComponent';

// from the 'utils' alias
// importing gatsbyProject/src/components/utilities/UtilityComponent.js
import UtilityComponent from 'utils/UtilityComponent';
```

### Jest testing

The new aliased paths lets Webpack correctly compile your app. However this does not mean that Jest, or other test runners will understand where those aliases point to.

When setting up testing with Jest, see [`moduleNameMapper`](https://jestjs.io/docs/en/configuration.html#modulenamemapper-objectstring-string--arraystring) to correctly map your your aliases to a path that Jest can understand.

For example:

```javascript
// gatsby-config.js
const path = require('path');

module.exports = {
  // ...other configs
  plugins: [
    {
      resolve: '@ayan4m1/gatsby-plugin-root-import',
      options: {
        components: path.join(__dirname, 'src', 'web', 'components')
      }
    }
  ]
};
```

could work with a Jest map like:

```javascript
// jest.config.js
module.exports = {
  // ...other configs
  moduleNameMapper: {
    '^components/(.*)': '<rootDir>/src/web/components/$1'
  }
};
```

Please see Jest's [`moduleNameMapper`](https://jestjs.io/docs/en/configuration.html#modulenamemapper-objectstring-string--arraystring) documentation for specific implementation details regarding format and options.
