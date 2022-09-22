# Eslint - Prettier - TS - React

- Create react app
```sh
yarn create react-app boilerplate-react-typescript --template typescript
```
- Install eslint - ESlint is a JS linting open source
```sh
yarn add -D eslint
yarn add -D eslint-plugin-react@latest @typescript-eslint/eslint-plugin@latest @typescript-eslint/parser@latest
yarn add -D eslint-plugin-import @typescript-eslint/parser eslint-import-resolver-typescript
```
- Install Prettier
```sh
yarn add -D prettier eslint-config-prettier eslint-plugin-prettier eslint-plugin-react-hooks

touch .prettierrc
```
- Open your `.eslintrc`

```
...json
"env": {
  "browser": true,
  "es2021": true,
	"jest": true // Add this line!
},
"extends": [
  "eslint:recommended",
  "plugin:react/recommended",
  "plugin:@typescript-eslint/recommended",
  "prettier" // Add this line!
],
"rules": {
  "react/react-in-jsx-scope": "off",
  "camelcase": "error",
  "spaced-comment": "error",
  "quotes": ["error", "single"],
  "no-duplicate-imports": "error"
},
"plugins": ["react", "react-hooks", "@typescript-eslint", "prettier"],
"settings": {
  "import/resolver": {
    "typescript": {}
  }
}
...
```

- Open your `.prettierrc`
```json
{
  "semi": false,
  "tabWidth": 2,
  "printWidth": 100,
  "singleQuote": true,
  "trailingComma": "all",
  "jsxSingleQuote": true,
  "bracketSpacing": true
}
```

- Open your `package.json`

```json
...
"scripts": {
  ...
  "lint": "eslint src/**/*.{js,jsx,ts,tsx,json}",
  "lint:fix": "eslint --fix 'src/**/*.{js,jsx,ts,tsx,json}'",
  "format": "prettier --write 'src/**/*.{js,jsx,ts,tsx,css,md,json}' --config ./.prettierrc"
},
```

# References
- https://medium0.com/m/global-identity?redirectUrl=https%3A%2F%2Fblog.devgenius.io%2Feslint-prettier-typescript-and-react-in-2022-e5021ebca2b1
