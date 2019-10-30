# About

The project consist of 3 components ecosystems:

-   ARC Components
-   API Components
-   Anypoint Web Components

The **ARC Components** ecosystem are general use web components that focuses on
API tooling. Provides UI and logic for applications that performs API calls.

Main recipient: Advanced REST Client.

The **API Components** ecosystem are the components that uses AMF model to generate
the view auto-magically. They are used to generate documentation from API spec file
and to populate request panel with values.

Main recipient: API Console, Advanced REST Client

The **Anypoint Web Components** is the basic building block for the other components ecosystems.
It provides low level UI components like inputs, selection control, chips, and so on.
The component were build to replace `paper-*` elements and to provide compatibility layer
with Anypoint Platform. Most components have `compatibility` attribute that is
passed to this components which turns the components to render Anypoint theme.

Main recipient: any application that is OSS and is to be used in Anypoint Platform.
