# Node-ts Backend Template

![image](https://github.com/adrianchinjen/backend-template-node/assets/81837952/cd084b71-e9e7-430f-93e3-9258dc35bc0a)

> Starting my own template for node-backend consisting of what I think are the developer tools needed.

To re-create this template, you can follow the instructions below.

## Project initialization
Initialize the node-ts project by running the code below:

```shell
npm init -y
```

## Typescript

We can start off this project by installing `typescript`
```shell
yarn add -D typescript
```

After installing typescript, create a `tsconfig.json` file with the configuration below:
```json
{
  "compilerOptions": {
    "target": "es2018",
    "module": "commonjs",
    "outDir": "dist",
    "sourceMap": true
  },
  "include": ["src/**/*.ts"],
  "exclude": ["node_modules"]
}
```

Install `nodemon` and `ts-node`
```shell
yarn add-D nodemon ts-node
```

After the installation, add a configuration file `nodemon.json` and copy and paste the config below
```json
{
  "watch": ["src"],
  "ext": ".ts,.js",
  "exec": "ts-node ./src/index.ts"
}
```

Add the following script below on `package.json` file
```json
"scripts":{
    "dev": "nodemon",
    "build": "tsc"
  }
```

Try building the project by running the command:

```shell
yarn build
```

## ESLint
Let's install first these two packages because `ESLint` by default does not support typescript.
```shell
yarn add -D @typescript-eslint/eslint-plugin @typescript-eslint/parser
```

Now, let's install `ESLint`, i'm using version `8.57.0` because for some reason, i'm having trouble running `eslint` command when installing the latest version (9.0.0)
```shell
yarn add -D eslint@8.57.0
```

Next thing is add a file `eslintrc` and copy and paste the configuration below:
```json
{
  "parser": "@typescript-eslint/parser",
  "extends": ["plugin:@typescript-eslint/recommended"],
  "parserOptions": { "ecmaVersion": 2018, "sourceType": "module" },
  "rules": {}
}
```

Add the following script to `package.json`
```json
"scripts": {
    "lint": "eslint src/**/*.ts",
    "format": "eslint src/**/*.ts --fix"
  }
```

Try running lint by running this command
```shell
yarn lint
```

To format the code,
```shell
yarn format
```


## Prettier

Let's now install `prettier`
```shell
yarn add -D prettier eslint-config-prettier eslint-plugin-prettier
```

Add a configuration file `.prettierrc` to the root folder of the project just like `.eslintrc`
```json
{
  "semi": true,
  "trailingComma": "none",
  "arrowParens": "always",
  "singleQuote": true,
  "printWidth": 120,
  "tabWidth": 2
}
```

Add `prettier plugin` to `.eslintrc` configuration, which should look like this
```json
{
  "parser": "@typescript-eslint/parser",
  "extends": ["plugin:@typescript-eslint/recommended", "plugin:prettier/recommended"],
  "parserOptions": { "ecmaVersion": 2018, "sourceType": "module" },
  "rules": {}
}
```

You can configure also to format your code on save on Visual Studio Code under settings.

## Husky
Husky is a popular tool that simplifies the setup and management of pre-commit hooks.

It is used in the JavaScript ecosystem and integrates seamlessly with Git repositories.

Husky empowers developers to automate pre-commit checks, enhancing code quality and productivity.

```shell
yarn add -D husky
```

After installation of `husky`, run the command below
```shell
npx husky-init
```

After running the script above, it will generate a hook on the folder `.husky` named `pre-commit`, inside the file should look something like this:
```
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"
npm test
```

Add a new script to `package.json`
```json
"scripts": {
    "prepare": "husky install"
  }
```


## Lint-staged
The concept of lint-staged is to run configured linter tasks (or other tasks) on files that are staged in git.

Lint-staged will always pass a list of all staged files to the task, and ignoring any files should be configured in the task itself.

Let's now install `lint-staged`
```shell
yarn add -D lint-staged
```

On the hook `pre-commit` under `.husky`, change the `npm test` to `npx lint-staged`. It should look something like this
```
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

npx lint-staged
```

Now, create a config on `package.json`
```json
"lint-staged": {
    "**/*.{ts,tsx}": [
      "prettier --write",
      "eslint --fix"
    ],
    "*.json": [
      "prettier --write"
    ]
  },
```

Now, trythe following command
```shell
git add .
```

After that,
```shell
 git commit -m "Initial commit"
```


## Commitlint
Commitlint maintains consistent formatting and style for commit messages in version control systems like Git.

It helps ensure that developers follow a specific set of rules or conventions when writing commit messages,
making it easier to manage and maintain a project's commit history and generate changelogs.

Let's install `commitlint` as dev dependency too
```shell
yarn add -D @commitlint/cli @commitlint/config-conventional
```

Create a configuration file named `commitlint.config.js` and copy and paste the following configuration
```json
module.exports = {
  extends: ['@commitlint/config-conventional'],
  rules: {
    //   TODO Add Scope Enum Here
    // 'scope-enum': [2, 'always', ['yourscope', 'yourscope']],
    'type-enum': [
      2,
      'always',
      ['feat', 'fix', 'docs', 'chore', 'style', 'refactor', 'ci', 'test', 'revert', 'perf', 'vercel']
    ],
    'subject-case': [
      2,
      'always',
      [
        'lower-case', // default
        'upper-case', // UPPERCASE
        'camel-case', // camelCase
        'kebab-case', // kebab-case
        'pascal-case', // PascalCase
        'sentence-case', // Sentence case
        'snake-case', // snake_case
        'start-case' // Start Case
      ]
    ]
  }
};
```

Now let's create a new hook under `.husky` folder named `commit-msg`, and copy and paste the code below
```
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx commitlint --edit $1
```

Let's try running the code below
```shell
git add .
```

Then,
```shell
git commit -m "foo: this will fail"
```


## Conclusion
These are the basic tools which for me is needed to achieve consistency and standard coding pattern within the team you are working with to ensure code readability.

You can add some more tools which will help you be more productive.



## Thanks!

Thanks to silver-xu of github for his article, it helps me a lot on setting up my configurations for typescript, eslint and prettier.

You can find and follow his article on the link below,
[Github Article](https://gist.github.com/silver-xu/1dcceaa14c4f0253d9637d4811948437#file-ts-boilerplate-md)

Also, thanks to Evandro Gibicoski of LinkedIn for his wonderful article, which helps me
on setting up my lint-staged and commitlint.

You can find and follow his article on the link below,
[LinkedIn Article](https://www.linkedin.com/pulse/nextjs-eslint-prettier-husky-lint-staged-commitizen-evandro-gibicoski/)



Happy coding! :wink:
