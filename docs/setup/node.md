

# Nodejs Installation

We use nodejs LTS 18 version. Install it using nvm. https://heynode.com/tutorial/install-nodejs-locally-nvm/

### Steps to follow for Node JS setup for any backend server

1. **LINTING:** 
   1. Install eslint – npm init @eslint/config. Choose the following options
   1. To check syntax, find problems, and enforce code style
   1. Javascript modules (import/export) 
   1. Framework – React
   1. Does your project use typescript – yes
   1. Where does your code run – None
   1. Use popular style guide -> Standard 
   1. Config for file – Javascript
   1. Like to install - > yes
   1. Use npm or yarn or pnpm depending
1. **COMMIT QUALITY:** Commitzen and commitlint
   1. Prefer pnpm instead of npm
   2. **```pnpm i -D husky cz-customizable commitlint commitlint-config-gitmoji lint-staged```**
   3. **```npx husky install```**
   4. **```npx husky add .husky/pre-commit “npm run lint-staged”```**
   5. Create a file **commitlint.config.js**.  Add the following in the file 
```json
      module.exports = {
        extends: ['gitmoji'],
        rules: {
          'header-max-length': [0, 'always', 100],
          'scope-case': [0, 'always', 'pascal-case'],
        },
      }
```
1. Create file . lintstagedrc in the root. Add the following to the file
```js
    {
      "./\*\*/\*.{js,ts,json,css,scss,md}": ["prettier --write"],
      "src/\*\*/\*.+(js,ts,json,css,scss,md)": ["tslint"]
    }
```
1. Create file .cz-config.js. Add the following
```js
module.exports = {
  types: [
    {value: ':sparkles: feat', name: '✨ feat:\tAdding a new feature'},
    {value: ':bug: fix', name: '🐛 fix:\tFixing a bug'},
    {value: ':memo: docs', name: '📝 docs:\tAdd or update documentation'},
    {
      value: ':lipstick: style',
      name: '💄 style:\tAdd or update styles, ui or ux',
    },
    {
      value: ':recycle: refactor',
      name: '♻️  refactor:\tCode change that neither fixes a bug nor adds a feature',
    },
    {
      value: ':zap: perf',
      name: '⚡️ perf:\tCode change that improves performance',
    },
    {
      value: ':white\_check\_mark: test',
      name: '✅ test:\tAdding tests cases',
    },
    {
      value: ':truck: chore',
      name: '🚚 chore:\tChanges to the build process or auxiliary tools\n\t\tand libraries such as documentation generation',
    },
    {value: ':rewind: revert', name: '⏪️ revert:\tRevert to a commit'},
    {value: ':construction: wip', name: '🚧 wip:\tWork in progress'},
    {
      value: ':construction\_worker: build',
      name: '👷 build:\tAdd or update regards to build process',
    },
    {
      value: ':green\_heart: ci',
      name: '💚 ci:\tAdd or update regards to build process',
    },
  ],
  scopes: [],

  scopeOverrides: {
    fix: [{name: 'merge'}, {name: 'style'}, {name: 'test'}, {name: 'hotfix'}],
  },

  allowCustomScopes: true,
  allowBreakingChanges: ['feat', 'fix'],
  // skip any questions you want
  skipQuestions: ['footer', 'breaking'],
  subjectLimit: 100,
}
```
1. Add the following lines to **package.json “scripts”**
```js
“lint-staged”: “lint-staged — config .lintstagedrc”,
“commit”: “./node_modules/cz-customizable/standalone.js”
"prepare": "husky install",
```

1. Add the following at the root at package.json
```js
"husky": {
  "hooks": {
  "pre-commit": "lint-staged",
  "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
  }
},
```

1. Add **git alias** for commit if needed ( optional). 
   1. **```git config alias.cz ‘!npm run commit’```**
1. Now either run ***```npm run commit```*** or **```git cz```** for committing files. 