# Google Summer of Code 2012 - Ideas Page

1. **Idea:** Community repository and package manager
   * **Summary:** We envision the Enyo project growing to include a thriving community of developers providing and sharing components that address common development problems (such as the web services library described below).  To support this vision, we need to provide the community with a central repository for distributing, discovering, and managing dependencies between community-developed libraries and add-ons.  This project will aim to tackle that problem by developing a solution (think npm, RubyGems, Maven, etc.), which may involve borrowing concepts from existing solutions or using them outright.
   * **Expected Results:** Recommendation on a solution justified by relevant research to be presented to the team, followed by implementation of your solution and if appropriate, design and deployment of your solution with support of the Enyo core team and the community.
   * **Prerequisites:** Familiarity with other package distribution and management solutions such as npm, RubyGems and/or Maven.  SCM and dependency management experience or interest.
   * **Mentor:** Adam Crabtree
1. **Idea:** Enyo add-on for Facebook integration
   * **Summary:** This project focuses on developing an add-on Enyo library that provides components for quick and easy integration of Facebook with Enyo apps.  You should start with components that handle login and authentication, and then propose and design Enyo controls that expose common developer use cases as easy-to-use Enyo objects, such as displaying a Friends list or posting a status, link, or photo.
   * **Expected Results:** An add-on Enyo libraries that provide turnkey Enyo components and API's for integrating Facebook services.  Developer-focused supporting documentation and samples.
   * **Prerequisites:** HTML/JS/CSS skills.  Working knowledge of OAuth.  Familiarity with Facebook Graph API and JavaScript SDK.
   * **Mentor:** Scott Miles
1. **Idea:** Enyo add-ons for OAuth and other popular web services
   * **Summary:** Enyo provides basic support for integrating with web services through the Ajax component.  Similar to the Facebook project above, this project focuses on developing one or more higher-level libraries that provide Enyo app developers with turnkey Enyo components to handle login/authentication and common operations of other popular web services, such Twitter, Box.net, Dropbox, etc.  You could attack these individually, or propose something like an uber social integration widget that allows posting to or aggregating info from multiple services. 
   * **Expected Results:** One or more add-on Enyo libraries that provide turnkey Enyo components and API's for popular web services.  Developer-focused supporting documentation and samples.
   * **Prerequisites:** HTML/JS/CSS skills.  Working knowledge of OAuth.  Familiarity with popular web services and their JavaScript SDK's.
   * **Mentor:** Scott Miles
1. **Idea:** Android scrolling performance
   * **Summary:** The market success of Android Gingerbread, coupled with the incredibly poor web scrolling performance of Android across the board, serves as one of the largest barriers to making web-based cross-platform app development a native killing solution.  We'd love your help tackling this problem.  This could range from a deep dissection of every possible solution to moving HTML content around on the Android browser, to porting your own custom version of Webkit to Android that solves the scrolling problem.
   * **Expected Results:** This is a broadly-defined problem, and so the expected results will be defined with your mentor based on a discussion of which angle of the problem you would like to research and attack.
   * **Prerequisites:**  HTML/JS/CSS skills.  Webkit development experience or interest.  CSS animation and transform experience.  
   * **Mentor:** Steve Orvell
1. **Idea:** UI widget themes based on other platforms
   * **Summary:** Enyo currently provides the Onyx UI component set, which is styled based on the original webOS/Touchpad design.  This project involves adapting/creating new sets of widgets with styles that resemble the user interface styling of other major mobile platforms, for those developers who would like a design more consistent with native apps on those platforms.  The project would likely involve exposure to the webOS professional user experience designers to review the designs you create.
   * **Expected Results:** Functional, beautiful, and high-performing widget sets based on styling of a number of major mobile platforms.
   * **Prerequisites:** Strong HTML and CSS skills, and an eye for design polish
   * **Mentor:** Frankie Fu
1. **Idea:** PhoneGap API wrappers
   * **Summary:** Many of PhoneGap's cross-platform JavaScript API's may be easier to use in an Enyo app if wrapped within an Enyo component.  This project aims at creating a sample Enyo app that exercises all of the PhoneGap API's, and in the process creating an Enyo PhoneGap integration library that wraps PhoneGap APIs when it provides better ease-of-use.
   * **Expected Results:** Sample Enyo PhoneGap kitchen sink app that uses your functional, tested PhoneGap API integration library.
   * **Prerequisites:** HTML/JS/CSS skills.  Familiarity with PhoneGap and one or more mobile SDK's (iOS, Android, webOS, etc.).
   * **Mentor:** Markus Leutwyler
1. **Idea:** Cross-platform Enyo components for Email, In-app Purchase, and Ad Network integration
   * **Summary:** PhoneGap provides a cross-platform JavaScript API that brings many device capabilities to web applications, however it tackles mostly low-level areas such as sensors, storage, and media.  To create robust web-based applications that can compete with the features of their native peers, apps often need access to the email, in-app purchase, ad networks, and other higher-level features of the platform.  This project centers around developing cross-platform Enyo components that tackle these areas, so developers have a richer toolkit of cross-platform, web-based components on which to build their apps.
   * **Expected Results:** One or more add-on Enyo libraries that provide cross-platform solutions to the use cases described above, as well as any others you propose.  Coordination with and contribution of parts of your solution to the PhoneGap project may also be appropriate.
   * **Prerequisites:** HTML/JS/CSS skills.  Proficiency with the iOS, Android, webOS, and WP7 SDK's and their respective languages. Familiarity with 3rd-party mobile ad networks.
   * **Mentor:** Markus Leutwyler
1. **Idea:** Enyo componetization of other JS framework UI components
   * **Summary:** Enyo kinds, the class-like encapsulation model that gives Enyo its object-oriented elegance, plays very nicely with UI elements from other JavaScript frameworks.  Essentially, any HTML content, rendered statically or dynamically by another library, can be wrapped in an Enyo control, allowing it to gain Enyo's object-based encapsulation.  For example, a jQueryUI date picker, Bootstrap navbar, or Flot graph component could be wrapped as an Enyo kind and easily integrated along other Enyo widgets in an Enyo app.  In addition, once promoted to an Enyo kind, these components can then be manipulated by Ares 2, our upcoming cloud-based application and GUI editor.  This project focuses on identifying and "Enyo-izing" the most useful components from other open-source Javascript/CSS libraries for use by Enyo developers.
   * **Expected Results:** One or more add-on Enyo libraries that wrap another framework UI toolkit such as jQuery Mobile, jQuery UI, etc.
   * **Prerequisites:** HTML/JS/CSS skills.  Familiarity with both Enyo and other JavaScript UI frameworks.
   * **Mentor:** Daniel Freedman
1. **Idea:** Localization solution for Enyo
   * **Summary:** Enyo needs a localization package to ease development of multi-language/locale apps.  This project would involve porting the original webOS localization library for use in Enyo apps, resulting in an add-on library that organizes and provides lang/locale-specific user strings, date formats, number formats, etc. to Enyo applications. 
   * **Expected Results:** A functional, reviewed, and tested Enyo localization add-on library.  Developer-focused supporting documentation and samples.  Bonus points for porting an existing single-language Enyo app to use your library to add localization support for one or more other languages/locale.
   * **Prerequisites:** HTML/JS/CSS skills.  i18n and L10n experience or interest.  Multi-lingual a plus.
   * **Mentor:** Daniel Freedman
1. **Idea:** Solution and best practices for MVC Enyo development
   * **Summary:** Enyo is non-dogmatic as to how application data, control code, and UI design are organized in your app code.  Enyo provides an object model that includes renderable and non-renderable components, and the flexibility to situate application logic and data in your renderable components or separated using a full MVC approach.  Since we recognize the MVC model is deeply embraced by some developers, this project aims to develop best practices, and where applicable, supporting components or integration of 3rd-party MVC libraries to both enable and provide best practices to MVC-oriented developers on how to develop Enyo apps using the model-view-controller approach.
   * **Expected Results:** Recommendation on best practices justified by relevant research to be presented to the team, and be formalized in developer-focused documentation.  If this solution involves development or use of a 3rd-party MVC library, proof-of-concept or sample app to be completed and included on developer site.
   * **Prerequisites:** HTML/JS/CSS skills.  MVC development experience or relevant study/research. 
   * **Mentor:** Ben Combee
1. **Idea:** Analytics integration library
   * **Summary:** Collecting user usage analytics provides valuable insight for any web or mobile app developer.  This project will focus on developing an Enyo add-on that leverages 3rd-party web analytics providers such as Google Analytics to provide insights to developers on application usage.  A simple solution might hit GA when an API is called, but a more robust solution might allow the developer to register and log custom events from certain user interface components or actions.  
   * **Expected Results:** An functional, tested add-on Enyo library that provides turnkey analytics integration with a major free analytics provider.  Developer-focused supporting documentation and samples.  Bonus points to integrating the library with an existing Enyo sample app.
   * **Prerequisites:** HTML/JS/CSS skills.  Familiarity with Google Analytics, Quantcast, SiteCatalyst, etc. a plus.  
   * **Mentor:** Markus Leutwyler
1. **Idea:** webGL Library
   * **Summary:** Analogous to the Enyo existing [canvas library](https://github.com/enyojs/canvas), this project would focus on developing an Enyo add-on library for webGL development.  This would allow developers to code self-contained webGL components as building blocks for a full-featured app.
   * **Expected Results:** A functional, reviewed, and tested webGL add-on Library for Enyo.  Developer-focused supporting documentation and samples.  Bonus points for using your library to develop a cool showcase game or app that highlights how Enyo's encapsulation model helps manage a complex graphics-based application.
   * **Prerequisites:**  OpenGL/ES or webGL proficiency. HTML/JS/CSS skills. Experience with game design a plus.
   * **Mentor:** Frankie Fu
1. **Idea:** Enyo Game development
   * **Summary:** This is a broader, open-ended project focused on coming up with best practices for developing games with Enyo.  The scope could range from developing a showcase game using Enyo that demonstrates integration with one of the many [HTML5 game engines](https://github.com/bebraw/jswiki/wiki/Game-Engines) that provide physics, collision detection, etc., to developing your own Enyo Game Kit that becomes a recommended add-on library for Enyo game developers.
   * **Expected Results:** This is a broadly-defined project, and so the expected results will be defined with your mentor based on a discussion of how you propose approaching the problem.
   * **Prerequisites:**  Game development experience.  HTML/JS/CSS skills.
   * **Mentor:** Frankie Fu
1. **Idea:** Cross-platform deployment for Ares
   * **Summary:** Our upcoming release of the Ares 2 will provide a turnkey, cloud-based IDE for developing cross-platform applications using Enyo.  This project focuses on getting the application pushed from Ares to the target device for testing, a key part of the cross-platform app development lifecycle.  The project will involve close coordination with the server-side Ares developer.
   * **Expected Results:** A working, deployable solution for downloading the packaged apps produced by Ares to one or more target device/environments such as iOS or Android devices.
   * **Prerequisites:** Mobile SDK experience (iOS, Android).  Strong JavaScript skills and node.js development experience or interest.
   * **Mentor:** Mark Bessey