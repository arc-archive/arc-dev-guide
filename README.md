# API components developers starting guide

This is a starting point for the API Components developers.

## What is API Components?

API components is an ecosystem of web components for API tooling. It contains base components, the building blocks for REST tooling, and composite components that builds complex UI.
Historically the components were designed for Advanced REST Client and API Console (by MuleSoft). However, because the components are open source we design the components to fit into various scenarios.

### You want contribute in Advanced REST Client?

Great! You are welcome. If you have an idea that the community can find useful we'd be happy to have you on board.

**But before you begin** read following sections and attached articles to understand project philosophy, development process and style guide for codding. As much as we want you to contribute as much we also want to understand your code and intentions.

## Project philosophy

### Free for all

The components and core applications build with it (ARC, APIC) are and will be free to use for everyone. It's open source project and you can involve in its development. We encourage you to build better API tooling with us.

### Easy to use

As a UX designer and developer I promise to develop easiest to use API tools. As a team we put a lot of effort to create beautiful but usable UIs not only for developers but for everyone involved in API life cycle. Every part of the UI and app's functionality is based on real users requirements.

### Quality and stability

We take it seriously if it comes to releasing components and applications build on top of them. We have build a custom release pipeline to ensure the component is well tested and documented before it gets released.
If you however find a bug in the released version, please, [file a bug report][issue_tracker] on project's Github page.

### Minimum breaking changes

The components is design to serve a very specific purpose. If a breaking change has to be introduced and it is a consequence of changing use cases or user requirements, then most probably a new comnponent should be created instead. So far the components were upgraded ony to adjust to web standard after underlaying technology changed.

## Development process

See [development process documentation][] for more details.

## Code style

See [code style documentation][] for more details.

## License

For open source and free software you can use this software on Apache 2.0 license.

For commercial or non open source projects you can use this software on CC-BY 4.0 license.

[issue_tracker]: https://github.com/advanced-rest-client/api-components-issues
[development process documentation]: docs/dev-guide.md
[code style documentation]: docs/code-style.md
