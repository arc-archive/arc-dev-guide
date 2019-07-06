# API Components architecture overview

## Composition

### Web composition model

In API Components ecosystem you can distinguish 2 basic types of components: base components and composite components.

__Base components__ are the most basic parts of the UI logic. They usually don't contain other custom elements. They are used to build composite components.

__Composite components__ are complex components build on top of existing components. You can think of it as parts of a more complex base components or a UI regions of the application.

For example `api-body-editor` component is a composition of various body editors (code, multipart, x-www-urlencode) that adds an integration layer on top of other components. All of the components can be used separately.

What is important is to not assume that your component has to work with only other component. In web environment is rarely a true. Take a dropdown menu as an example.

```html
<my-dropdown>
  <my-listbox>
    <my-list-item></my-list-item>
  </my-listbox>
</my-dropdown>
```

The `dropdown` element assumes it will have a child that exposes a `selected` property that is used to determine current selection and therefore it can obtain corresponding label to render when the `dropdown` is closed. The `listbox` element just renders whatever was added to it's [light DOM][] and manages selection state.
It doesn't really matter for the components what is the name of the component passed as a child element. The `listbox` renders `my-list-item` but it will render `img` or `p` elements as well.
The same is for `dropdown` element. As long as child element has `selected` property it will work properly. However if the element won't expose this property the component should still render without any error message. This is consistent with the web platform. When a `p` (paragraph) is passed to the `ul` (unordered list), the paragraph will still be rendered even though the composition is incorrect. The component should not throw error when incorrectly initialized.

### Composite API components

API Components comes with a rich library of web components that allows developers to build complex application for interacting with APIs. Each component reuse existing components and minimize coder repetition. Basic rule is to create separate component or a JavaScript mixin that can be reused later in other components.

The component never serves more than one purpose. In the example above the `my-dropdown` only renders a label for the dropdown form control and changes the label when selection in the `my-listbox` element changes. Selection management is a sole responsibility of `my-listbox` element. It has `selected` and `selectedItem` properties and few helper functions like `selectNext()` or `clearSelction()` functions. Finally `my-list-item` just renders any content passed to the [light DOM][] in a single line. Having this composition you can:
-   reuse `my-listbox` in various situations when a selection of one of the option is required, like application menus, drop downs, radio buttons and so on.
-   reuse `my-list-item` whenever you need to render a list if items in an unified way like menus, drop downs, selection options.

This kind of composition is highly scalable when creating new components. Instead of repeating code that serves the same purpose but in different context it use another component.

#### Things to avoid

__Don't build monoliths__. The `my-dropdown` components could be build as a single component that accepts a configurable list of children to render. This however is not scalable as the state selection and rendering is enclosed in this component and cannot be shared. It would also allow you to render very specific view because it would internally render list items only. No way to render in a dropdown a list of images without breaking changes to the component. Finally it is not web friendly as this is not how native HTML elements works.

__Think about the web__. When creating new component think how similar patterns were used in the web platform. When using similar and therefore familiar patterns the developer that uses the component in the future has less work to do when learning how to use the component.


## Web component vs JavaScript library

Some of the components in the API Components library do not offer an UI. We call them logic components. They only performs some kind of logic but do has no visual layer.
You may think a JavaScript library should be used instead and in many use cases you would be right. However, sometimes the component may need to use DOM APIs and it would be inconvenient to initialize the functionality via imperative API. API Components prefer declarative use of APIs.

`arc-models` elements are an example of this. The `arc-models` elements is a set of components that provide an access to the datastore in Advanced REST Client. The `arc-request-model` provides access to the request store and allows to query for requests for given criteria. It could be a JavaScript library but the model listens to DOM events so other component requesting access to the datastore do not have to have direct access to the model element. When `arc-request-model` is inserted to the DOM it attaches a listener to a node and waits for events or direct call of a function in exposed API. It also dispatches events when a model changes so other elements can react on the change. It could be don with a library but with each component that uses the model you would have to initialize the library and remember to clean up when the component is detached from the DOM. Finally, as mentioned, in API Components ecosystem we prefer declarative programming. Therefore the model should be added to the shadow DOM of the component instead of calling a function in JavaScript library.

This doesn't apply to 3rd party libraries which has to be used imperatively unless a custom element wrapper is created for the library.

## Passing data to and from a component

__Use attributes to receive non-complex data__. Elements are configured declaratively as much as possible, which allows them to be used declaratively, as well as work great with template bindings.
For complex data structures use JavaScript properties instead. Properties for complex data structures do not have to be reflected to an attribute. This is how the native elements works. You can control their behaviour using both JavaScript properties and attributes.

__Use events to pass data to the outside world__. All custom elements inherit from EventTarget, and thus events can be used to pass data along, but please follow the guidelines in the [DOM Living Standard §2.11][]. See also the TAG’s guidance on [event design][].

## Events API

See guidance on [event design][] as it helps to understand how web events are designed.

API components has event's API that allows the components to communicate with each other without being directly "wired" together by a binding system.
At the first iteration of the components library elements were bind together via (Polymer's) binding system. This caused a lot of problems when it comes to re-usability. Consider content type selector and headers editor. Both of them are operating on the headers string that is used to construct HTTP request. However they are not used in the same part of the application. The headers editor is included into request editor and the content type selector is included in body editor (code editor exactly). Naturally in this scenario data binding would work as expected. However you would have to do the same thing every time you want to use this two elements in other application in different configuration. It is inconvenient for an author and has to be somehow documented in both components.

Instead API components has a common communication system so this two components can communicate state change using events system. When one dispatches an event about `content-type` header change then the other updates it's state without being connected to each other using data binding.
The same model is used in `arc-models`. When a model has changed and the changes has been committed to the data store the component dispatches corresponding event. Each component that relays on the model state listens for this event and updates it's state if necessary. Consider `arc-history-menu` element. When new entry is added to the history store the menu should also add new item to the list. This is done by listening for change event.

Currently there is no central documentation for events API. Each event is documented in the component documentation. It may be available in the future.

## Shell application concept

Both API Console and Advanced REST Client follows the same pattern when building an application. It is shell application.
The application itself is a host that contains all components needed for the functionality of the application and provide platform specific interfaces (usually via event API). However the application do not provide application specific logic.

Take API Console as an example. The application consist of three main components:
-   `api-navigation`
-   `api-documentation`
-   `api-request-panel`

This three components are responsible for rendering navigation and documentation, and also performing a test request to currently selected endpoint and method.

It also has optional component `api-simple-xhr-request` to perform HTTP request from the browser. However in some situations it is not required as this can be handled by other application logic (like running request via proxy).

The whole logic of the application is enclosed in those components and the `api-console` application only passes navigation selection state to the other components. It also manages state for media queries and allows to load AMF model from an external file. However it has nothing to do with rendering navigation, documentation, or making a request to the endpoint.

With this pattern it is relatively easy to introduce new application on different platform. It would be easy to create API Console as an Electron or Chrome application just by providing different shells. The application logic is still the same - 3 components working together.

One note here. Sometimes your code may require platform specific API. For example, `localStorage` is not available in Chrome Apps. You have to use either `IndexedDB` or Chrome specific API. In this situation it is better to use events to store / read data from `localStorage` and introduce a component that handles the events. In chrome platform you would have to only replace the storage component instead of the underlying component which would cause significant code refactoring and reduce time to market with new application.

## Accessibility

Accessibility is very important for custom elements. It is crucial to add accessibility attributes and states to your component. There are multiple contexts the user can use your component. It can be visually impaired person that uses spoken feedback, it can be a kiosk application that has only keyboard available, or high contrast environment.

The component has to use proper `role` and `aria-*` attributes to ensure the component can be used in all this situations. Components without accessibility setup won't be accepted to the API Components ecosystem.
Also, each component is required to contain accessibility tests. Even though automatic tests can only discover handful number of problems it is still better to use them than not at all.


Further reading:
-   [Web Components design guidelines](https://www.w3.org/2001/tag/doc/webcomponents-design-guidelines/)
-   [Web Components Gold Standard](https://github.com/webcomponents/gold-standard/wiki)

Next: [API components development process](apic-development-process.md)
Previous: [Prerequisites](dev-prerequisites.md)

[light DOM]: https://developers.google.com/web/fundamentals/web-components/shadowdom#lightdom
[DOM Living Standard §2.11]: https://dom.spec.whatwg.org/#action-versus-occurance
[event design]: https://w3ctag.github.io/design-principles/#event-design
