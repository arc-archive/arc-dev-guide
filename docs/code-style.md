## Code style

Use `@open-wc` to bootstrap your project. It adds linting rules and commit rules into your project's configuration.
However, most of the components does not follow the exact same rules as `@open-wc`. Therefore remove `eslint.config.js` file from your project
and create EsLint configuration file on your workspace main directory.


__.eslintrc.json__
```json
{
  "extends": "google",
  "parserOptions": {
    "ecmaVersion": 8,
    "sourceType": "module"
  },
  "env": {
    "browser": true,
    "node": true
  },
  "rules": {
    "require-jsdoc": 0,
    "comma-dangle": 0,
    "new-cap": ["error", { "properties": false, "capIsNew": false }],
    "brace-style": ["error", "1tbs", { "allowSingleLine": true }],
    "max-len": ["error", { "code": 120 }],
    "object-curly-spacing": ["error", "always"]
  }
}
```

### Editor config

#### The editor

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

#### JavaScript Code Style

__.jscsrc__
```json
{
  "preset": "google",
  "disallowSpacesInAnonymousFunctionExpression": null,
  "disallowTrailingWhitespace": null,
  "maximumLineLength": 120,
  "excludeFiles": ["node_modules/**"],
  "jsDoc": {
    "checkAnnotations": {
      "preset": "jsdoc3",
      "extra": {
        "polymerBehavior": true,
        "polymer": true,
        "customElement": true,
        "appliesMixin": true,
        "memberof": true,
        "demo": true,
        "mixinFunction": true,
        "mixinClass": true
      }
    }
  }
}
```

#### JSHint

__.jshintrc__

```json
{
  "esversion": 8,
  "node": true,
  "browser": true,
  "bitwise": false,
  "camelcase": false,
  "curly": true,
  "eqeqeq": true,
  "immed": true,
  "indent": 2,
  "latedef": true,
  "noarg": true,
  "quotmark": "single",
  "undef": true,
  "unused": true,
  "newcap": false,
  "mocha": true,
  "globals": {
    "assert": true
  }
}
```
