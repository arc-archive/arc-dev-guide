# API components development process

## Bootstrapping a project

API Components uses [open-wc recommendations](https://open-wc.org/). Be sure to read this documentation first.

To bootstrap the component's project use the following command:

```sh
npm init @open-wc
```

Select `demoing` and `testing` option when creating the project.

This will create a new project with predefined configuration for linter, git, commits, and testing.

API components uses Sauce Labs instead of recommended BrowserStack. Because of that manual copy of SL configuration files is required.
Also `open-wc` do not include accessibility testing. API Components are also required to build TS interface.

Copy the following files to your project:

-   [karma.sl.config.js][]
-   [.travis.yml][]

Add the following entries to your `package.json` file

```json
{
  "devDependencies": {
    "@advanced-rest-client/a11y-suite": "^1.0.0",
    "@advanced-rest-client/testing-karma-sl": "^1.0.2",
    "@polymer/gen-typescript-declarations": "^1.6.1"
  },
  "scripts": {
    "update-types": "gen-typescript-declarations --deleteExisting --outDir .",
    "test:sl": "karma start karma.sl.config.js --legacy --coverage"
  }
}
```

The `test:sl` command is executed when push to any branch is executed. You have to generate own keys for SauceLabs and put them into `.travis.yml`. Use [Travis CLI](https://docs.travis-ci.com/user/encryption-keys/) to do this.

## Developing

Well, it's up to you how you going to develop the component. However follow the following rules.

__Use the same code style__. See [code-style.md](code-style.md) documentation for more information. It makes it easier for everyone to use the same code styling. Especially it is important for linters. Commit may be accepted in your development environment but rejected in another if linter configuration is different. That makes collaboration very hard to do.

__Check if similar component already exists__. That should go without saying but we do not need duplicates. Take a not in __Versioning__ section for exceptions.

__Note events design patterns__. As described in [event design][] recommendation document, when creating an event inside the component also create `oneventname` setter that registers the event. See referenced document for more information.

__Always add documentation, demo, and tests__. No exceptions here. Each document must have demo page. See corresponding sections below for more information.

__Add @customElement to component's class documentation__. This is not required buy the documentation tooling is using this property to recognize a custom element definition. This is used for generating documentation page.

## Accessibility

It is your responsibility to add support for a11y in your component. This means, if applicable, add `role` and `aria-*` attributes to your components.
If your components is interactive make sure it is focusable. If your component support states like `checked` or `disabled` use an appropriate aria attributes.
Finally tests the components for accessibility in your test cases (described below).

Read this [Accessibility Principles][] to get started with accessible web.

## Demoing

The demo page is a part of the development process. It is required for all API Components. There are 2 main reasons to do this.

First reason is the you as the author can design a better component. You can visually inspect the component and debug it. Try to create different demo states like with different options enabled or with different composition of the components. If the element has a label or text input then create a demo case that has `dir="ltr"` attribute.

Ideally would be having a demo page that applies different styles via CSS variables and CSS parts. Try to create a dark theme of the component. This is helpful to predict which styling API to expose before publishing the element.

Second reason is that the catalogue application, that lists all API Components, renders a demo page for the component. This allows authors to pick a component they need for their task.

Inside the component's class documentation add `@demo` tag and a lint to the demo page. It may contain more than one demo pages. The documentation builder in the CI pipeline with pick this tags and will use them to build documentation page.

```javascript

/**
 * @customElement
 * @demo demo/index.html
 * @demo Dark theme demo/theme.html
 */
class MyComponent extends HTMLElement {
  ...
}
```

Components without demo page won't be accepted for publication.

See [api-navigation][] element for an example.

## Documenting

You won't be the only person developing the component. There will be others that will introduce new functionality or fix a bug. Make their life easier and document the code. Do it also for yourself. In few weeks you won't be remember what the code does.

Public functions (not staring with `_`) must be documented. This is your component's public API and authors must know how to use it.

```javascript
/**
 * Selects an item for given index.
 * @param {Number} index An index of an item to be selected
 * @return {HTMLElement} Reference to newly selected element
 * @throws {TypeError} When passed argument is not a number.
 * @throws {RangeError} When index is lower than zero or higher than number of items.
 */
select(index) {
  if (isNaN(index)) {
    throw new TypeError(`Expected number, ${typeof index} given.`);
  }
  if (index > this.items.length - 1 || index < 0) {
    throw new RangeError('Index out of bounds.');
  }
  this.selected = index;
  this._updateSelection();
  return this.selectedItem;
}
```

Please, add `keywords` to `package.json` file. This keywords are used by the components catalogue.

```json
{
  "keywords": [
    "web-components",
    "button",
    "radio-button",
    "radio",
    "form-elements"
  ]
}
```

Currently is mandatory to call `npm run update-types` before publishing the component manually. The CI pipeline will handle this in the future. This command generates typpings for the component so it can be used in TS projects.

## Testing

Follow rules from `@open-wc` for testing. Projects created with `@open-wc` uses Karma to run tests.

### a11y

To test for accessibility use [a11ySuite][]. This will detect some accessibility issues, if any.
Try to test for different states, like focused, disabled, active, etc.
Don't forget to do manual testing if your `aria` or `role` attributes are set correctly.

```javascript
import { fixture, assert } from '@open-wc/testing';
import { a11ySuite } from '@advanced-rest-client/a11y-suite/index.js';

describe('a11y', () => {
  async function basicFixture() {
    return (await fixture(`<anypoint-radio-button>Label</anypoint-radio-button>`));
  }

  async function roleFixture() {
    return (await fixture(`<anypoint-radio-button role="checkbox"></anypoint-radio-button>`));
  }

  async function tabindexFixture() {
    return (await fixture(`<anypoint-radio-button tabindex="-1"></anypoint-radio-button>`));
  }

  a11ySuite('Normal state', '<anypoint-radio-button>Label</anypoint-radio-button>');
  a11ySuite('Disabled state', '<anypoint-radio-button disabled>Label</anypoint-radio-button>');
  a11ySuite('Checked state', '<anypoint-radio-button checked>Label</anypoint-radio-button>');
  a11ySuite('No label', '<anypoint-radio-button></anypoint-radio-button>');
  a11ySuite('Aria label', '<anypoint-radio-button aria-label="Batman">Robin</anypoint-radio-button>');

  it('Sets default role attribute', async () => {
    const element = await basicFixture();
    assert.equal(element.getAttribute('role'), 'radio');
  });

  it('Respects existing role attribute', async () => {
    const element = await roleFixture();
    assert.equal(element.getAttribute('role'), 'checkbox');
  });

  it('Sets default tabindex attribute', async () => {
    const element = await basicFixture();
    assert.equal(element.getAttribute('tabindex'), '0');
  });

  it('Respects existing tabindex attribute', async () => {
    const element = await tabindexFixture();
    assert.equal(element.getAttribute('tabindex'), '-1');
  });
});
```

Because `a11y` is not part of `@open-wc` you need to include AXS testing file manually into the tests.
Add this line into your `karma.conf.js` file:

```javascript
  files: [
    ...
    'node_modules/accessibility-developer-tools/dist/js/axs_testing.js'
  ]
```

## Committing

`@open-wc` generated project provides rules for commit messages using Husky.

See the commit rules in [config-conventional][] package used by `@open-wc`.


## Components versioning

It is very important section and please, don't ignore it.

API Components are build in a way that should not require major releases in the future. If you read and follow rules of [API components architecture overview][] you are almost onboard with this philosophy. So far there were 2 major updates to the components but not because the requirement for the component changed but rather underlying technology (HTML spec).

The general rules is if you need to create a breaking change in existing component then most probably you actually need a new component. The old component probably works well in different application or environment and should not be changed so drastically. If your context has changed then build a new component that might be similar but serves actually different purpose.

This way we won't create a mess with versioning and upgrading and applications consuming the components can use pattern like `^1.0.0` when installing the component as it will never introduce breaking change.

When deprecating an API in your component just leave the old API there but without the actual implementation but with the replacement. We don't want breaking changes as much as we don't like upgrading legacy application for new API. This application should still be working even when upgrading version of a component.

## Further reading

Next: [API components publishing process](apic-publishing-process.md)
Previous: [API components architecture overview](apic-architecture-overview.md)

[event design]: https://w3ctag.github.io/design-principles/#event-design
[karma.sl.config.js]: ../resources/karma.sl.config.js
[.travis.yml]: ../resources/.travis.yml
[api-navigation]: https://github.com/advanced-rest-client/api-navigation/tree/stage/demo
[a11ySuite]: https://www.npmjs.com/package/@advanced-rest-client/a11y-suite
[Accessibility Principles]: https://www.w3.org/WAI/fundamentals/accessibility-principles/
[config-conventional]: https://www.npmjs.com/package/@commitlint/config-conventional
[API components architecture overview]: apic-architecture-overview.md
