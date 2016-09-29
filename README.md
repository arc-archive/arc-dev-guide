# Advanced REST Client developers starting guide

This is a starting point for the ARC developers.

### You want contribute in Advanced REST CLient?
Geat! You are welcome. If you have an idea that the community can find useful we'd be happy to have you on board.

**But before you begin** read following sections and attached articles to understand project philosophy, development process and style guide for codding. As much as we want you to contribute as much we also want to understand your code and intentions.

## Project philosophy

### Free for all
The app is and will be free to use for everyone. It's open source project and you can involve in its development.

### Easy to use
As a UX designer and developer I promise to develop easiest to use tool from them all. Every part of the UI and app's functionality is based on real users requirements. All you can find there is what you really need to work with REST services.

### Quality and stability
The app should work as desired. I take it serious if it comes to release and I've prepared complex testing and releasing process so the final release should be free of bugs.
If you however find a bug in the released version, please, [file a bug report][issue_tracker] on project's Github page.

## The Polymer framework and web components
Web components it's quite new thing and you may not be familiar with it. Polymer framework basically registers a custom element that it's only task is to register other custome elements according to Polymer's framerowk development book.
It also add additional functions accessible inside the components like data binding and DOM manipulation.

Read more about Polymer framework on their website [www.polymer-project.org](https://www.polymer-project.org/)

## Development process
ARC's development process is tailored for it's structure and fully automated. The project uses it's own CLI tools like [polymd] or [arc-tools] to speed up development process. It also have custom made CI (alonside Travis-CI) so every merge in the main branch builds and publish other related tools.

First thing to remember that in ARC ecosystem the main branch is stage, not main. Main branch is just a reflection of current release and is supporting CI workflow. **Every pull request has to be made to the stage branch.**

More about development process in [development process wiki] page.

## Code style
Please, follow rules under [code style wiki] page. Every moderm editor can be configured for specific code style for given project.

##Contact
You can contact me using my [Google+ account][gp_profile].
Also, follow ARC's [profile on Google Plus][gp_appprofile] or read application [blog][app_blog].

##License
This project is licensed under Apache License 2.0.
See the LICENSE file for more details.

[cws_url]: https://chrome.google.com/webstore/detail/advanced-rest-client/hgmloofddffdnphfgcellkdfbfbjeloo?utm_source=gitgub&utp_campaign=app&utm_medium=installation
[cws_logo]: https://developer.chrome.com/webstore/images/ChromeWebStore_BadgeWBorder_v2_340x96.png "Get from Chrome Web Store"
[issue_tracker]: https://github.com/jarrodek/ChromeRestClient/issues
[gp_profile]: https://plus.google.com/+PawelPsztyc
[gp_appprofile]: https://plus.google.com/b/117577071661965941720/117577071661965941720
[app_blog]: restforchrome.blogspot.com
[polymd]: https://github.com/advanced-rest-client/polymd
[arc-tools]: https://github.com/advanced-rest-client/arc-tools
[development process wiki]: https://github.com/advanced-rest-client/arc-tools
[code style wiki]: http://github.com
