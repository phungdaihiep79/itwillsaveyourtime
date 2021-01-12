# How to setup module resolution and path aliases with react-native ---typescript

Purpose : make something like this\
`import { Button } from '../../../../../components/button'`\
to\
`import { Button } from '@components/button'`


### Getting started
#### STEP1: 
  Add babel plugin to enable the module resolution:\
  `yarn add --dev babel-plugin-module-resolver`\
  or npm\
  `npm install --save-dev babel-plugin-module-resolver`
#### STEP2 : 
  Afetr the plugin was installed we need to update `babel.config.js`. Add `module-resolver` to the list of Babel plugin :
  ```
  module.exports = {
  presets: ['module:metro-react-native-babel-preset'],
  plugins: [
    [
      'module-resolver',
      {
        root: ['./src'],
        extensions: [
          '.ios.ts',
          '.android.ts',
          '.ts',
          '.ios.tsx',
          '.android.tsx',
          '.tsx',
          '.jsx',
          '.js',
          '.json',
        ],
        alias: {
          '@components': './src/components'
        },
      },
    ],
  ],
}
```
**'root'** specifies your project main directory. Usually, it is called ‘src’.\
**'extensions'** allow you to limit the plugin to specific file types.\
**'alias'** lets you specify all the folders you need shortcuts for your module imports.\

#### STEP3:
Now it's time to config the typescript in **tsconfig.json**\
```
{
  "compilerOptions": {
    ...
    "baseUrl": "./src",
    /* Base directory to resolve non-absolute module names. */
    "paths": {
      "@components": ["./components"],
    },
    ...
  },
  ...
}
```

#### Troubleshooting
If you followed the previous steps and still have issues it might be related to old cache still being used to resolve file pathing. If that happens make sure to start your React Native project with:\
```yarn start --reset-cache```\
If the line above still doesn’t solve the issue, try more radical measures:\
```watchman watch-del-all && rm yarn.lock && rm -rf node_modules && rm -rf $TMPDIR/metro-* && rm -rf $TMPDIR/haste-map-* && yarn && yarn start --reset-cache```
