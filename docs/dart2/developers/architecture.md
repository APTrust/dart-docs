# Architecture

DART was designed around a few core principles.

1. It should use commonly available and well understood open source technologies.

1. It should run on Windows, Mac, and Linux.

1. It should include a plugin architecture so developers can add new features and functions without having to learn the system's internals.

1. It should separate core functional code from UI code, so the application's core features can be run from the UI or from the command line.

1. Components should be as loosely coupled as possible so that:

    1. They can be replaced at will, and so
    1. They don't cause bugs in other components.

1. All components should be implemented in the simplest way possible. This means:

    1. Don't add extraneous JavaScript libraries just to pick up one or two handy functions.
    1. Don't do in JavaScript what the browser already does for you (such as navigation and making decisions about rendering).
    1. Don't use libraries like React, Angular, Vue, etc. (because DART's HTML is so simple, these libraries would only add complexity without adding value).

1. DART should be maintainable for the long term. In practice, this goes beyond architectural principles to include two ongoing practices to accompany all new and updated code:

    1. Thorough testing.
    1. Complete and useful documentation.

## Tech Stack

DART is built on the following tech stack:

* [Electron](https://electronjs.org/), which provides a cross-platform UI as well as build and distribution capabilities.

* [Node.js](https://nodejs.org/), which provides a portable JavaScript runtime and a set of core libraries.

* [HTML5](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5), which provides simple interactive displays.

* [Bootstrap 4](https://getbootstrap.com/docs/4.0/getting-started/introduction/), which provides a rich and flexible CSS framework.

* [Handlebars](https://handlebarsjs.com/), a brain-dead templating language that forces you to separate logic from presentation.

## MVC Pattern

DART uses an MVC design pattern similar to Ruby on Rails. This choice was guided by the following considerations:

* DART can route requests using standard URLs. There's no need for a routing engine, or for JavaScript to keep track of navigation history, the state of HTML components on the page, data mappings, a virtual DOM, etc.

* Each MVC request routes to one endpoint of one controller and the mapping is always obvious from the URL. For example, AppSetting/list maps to the list() method of the AppSetting controller.

* All request parameters are passed through the URL, so there's no need for special JavaScript classes, structures, or mappings.

* Each request results in a full re-rendering of the DOM. This is seen as an evil to be avoided in web applications because a page load requires one or more network calls and causes the user to lose state information and the dynamically loaded content that comes from infinite scrolling. DART does not need to make network calls to load pages, and because it keeps no state, there's nothing to lose. Because a typical DART page loads only 10-20 kilobytes of HTML, it renders quickly, taking advantage of all the browser's rendering optimizations.
