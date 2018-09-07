# Add ESLint and Prettier support in VS Code and CRA

This guide follows closely the great guide from the [Manorisms youtube channel](https://www.youtube.com/watch?v=bfyI9yl3qfE&feature=youtu.be).

1. First thing to do: create a new CRA app

```bash
create-react-app new-app
```

2. Install the [ESLint extension for vscode](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

3. Create a `.eslintrc` file:

```js
{
  "extends": ["react-app", "plugin:prettier/recommended"]
}
```

4. Install the [Prettier extension for vscode](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

5. Install some npm packages for prettier support

```bash
npm i prettier eslint-config-prettier eslint-plugin-prettier -D
```

6. Finally, change some VS Code settings:

```json
{
  "editor.formatOnSave": true,
  "[javascript]": {
    "editor.formatOnSave": false
  },
  "eslint.autoFixOnSave": true,
  "eslint.alwaysShowStatus": true,
  "prettier.disableLanguages": ["js"],
  "files.autoSave": "onFocusChange"
}
```
