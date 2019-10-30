# Code style

## JavaScript Code Style

Install the following packages as dev depdendencies:

-   @advanced-rest-client/eslint-config
-   @advanced-rest-client/prettier-config

```sh
npm i -D @advanced-rest-client/eslint-config @advanced-rest-client/prettier-config
```

And use the following configuration files

`prettier.config.js`

```javascript
module.exports = require('@advanced-rest-client/prettier-config');
```

The `.eslintrc.js` is installed automatically with `@advanced-rest-client/eslint-config` package.

## Editor config

### The editor

Put this file into the root of your workspace.

__.editorconfig__

```ini
root = true

[*]

indent_style = space
indent_size = 2

end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true

[*.md]
trim_trailing_whitespace = false
```
