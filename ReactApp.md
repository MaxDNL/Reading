# React Application 
This project is an opinionated template for building Front End Single Page Web Applications. It includes transpilation, hot dev, asset packing, unit testing, functional testing, Selenium end-to-end web testing, and source code that defines a modified MVC pattern to enforce unidirectional data flow and centralized state management. Developers familar with Flux and Redux patterns will recognize some familiar constructs.

This document and its linked documents describe the various components of this project in detail. For those who are already familiar with the patterns and processes this project defines, there is a [Quick Start](#user-content-quick-start).

## Table Of Contents
* [Quick Start](#user-content-quick-start)
* [Prerequisites](#user-content-prerequisites)
* [Start A New Project](#user-content-start-a-new-project)
	* [Fork Project](#user-content-fork-project)
	* [Clone Project](#user-content-clone-project)
	* [Install Project Dependencies](#user-content-install-project-dependencies)
* Overviewgit 
	* [Classes](#user-content-classes)
		* [Model](#user-content-model)
			* [Dispatcher](#user-content-dispatcher)
			* [Service](#user-content-service)
			* [Store](#user-content-store)
		* [View](#user-content-view)
		   * [AbstractView And React Component Lifecycle](#user-content-abstractView-and-react-component-lifecycle) 
		* [Controller](#user-content-controller)
		* [Application State](#user-content-application-state)
	* [Dependency Diagram](#user-content-dependency-diagram)
	* [Data Flow](#user-content-data-flow)
	* [Routing](#user-content-routing)
* Conventions And Patterns
	* [Conventions](#user-content-conventions)
		* [Access Modifiers](#user-content-access-modifiers)
	* [Patterns](#user-content-patterns)
	* [Idioms](#user-content-idioms)
* [Development](#user-content-development)
	* [Test](#user-content-test)
		* [Unit Tests](#user-content-unit-test)
			* [Unit Test Development](#user-content-unit-test-development)
				* [AbstractNotificationStub](#user-content-abstractnotificationstub)
				* [DispatcherStub](#user-content-dispatcherstub)
				* [ServiceTest](#user-content-servicetest)
				* [ViewTest](#user-content-viewtest)
			* [Running Unit Tests](#user-content-running-unit-tests)
				* [Run All Unit Tests](#user-content-run-all-unit-tests)
				* [Run One Unit Test File](#user-content-run-one-unit-test-file)
				* [Run A Single Unit Test](#user-content-run-a-single-unit-test)
      * [Test Coverage](#user-content-test-coverage)
		* [End-To-End Tests](#user-content-end-to-end-tests)
			* [Run All End-To-End Tests](#user-content-run-all-end-to-end-tests)
			* Troubleshooting
				* [Cannot Find My Element](#user-content-cannot-find-my-element)
	* [Source Code](#user-content-application-code)
		*[Directories](#user-content-directories) 
* [Deployment](#user-content-deployment)
* [Browser Support](#user-content-browser-support)
* [Resources](#user-content-resources)


## Quick Start
This Quick Start assumes that you already have all of the [Prerequisites](#user-content-prerequisites)

1. [Fork](#user-content-fork-project) this repo.
2. [Clone](#user-content-clone-project) to your local
3. [Install](#user-content-install-project-dependencies) dependencies with `npm install`
4. [Start](#user-content-running-ocally) developing with `npm start`
5. [Test](#user-content-run-all-unit-tests), _test_, _**test!** with `npm test`

## Prerequisites
This project assumes a general knowledge of OO and Functional programming concepts and constructs. It also assumes you are running a *NIX dev box or have [set up your Windows machine](https://github.com/taylor-digital/react-application/blob/windows-setup-docs/_docs/WindowsMachineSetup.md), and that the developer machine is set up per the recommendations that follow:

* Installed NVM
	* **[Mac (nvm)](http://dev.topheman.com/install-nvm-with-homebrew-to-use-multiple-versions-of-node-and-iojs-easily/)**
	* **[Windows (nvm-windows)](https://github.com/coreybutler/nvm-windows)** 


* Node v6+ and NPM v3+ via NVM: 
	* **Mac** `nvm install v6`
	* **Windows** `nvm install 6.10.2`



## Start A New Project
To start a new project, you will need to fork the latest codebase from this repository, clone it to your box, and then install all of the necessary dependencies via npm.

### Fork Project
Navigate to this project root here on GitHub. In the top right corner of the page, click "Fork."

### Clone Project
Navigate to the deisired location on your box where you would like your local version of the newly [Forked](#user-content-fork-project) project to live. Then, run the following in your terminal.

```sh
git clone <URI>

```
_*Note: the above script may require that you have proper credentials depending upon how you choose to connect to GitHub. [SSH credential creation and management](https://help.github.com/articles/connecting-to-github-with-ssh/) are outside of the scope of this document._

### Install Project Dependencies
This project and all forked projects require dependencies. After you have [cloned](#user-content-clone-project) the project to your local. You can install all of these dependencies with NPM by running the following in your terminal.

```sh
npm install

```
_*Note: this project is configured to assume failure of 3rd party modules to adhere strictly to Semver. Therefore, npm has been configured to save project dependencies by exact version. This can be turned off by setting `save-exact=false` in `./.npmrc`, or by removing this file completely._


## Overview

### Classes
There are seven main constructs that comprise this project pattern:

* Model
	* [Dispatcher](#user-content-dispatcher)
	* [Service](#user-content-service)
	* [Store](#user-content-store)
* [View](#user-content-view)
* [Controller](#user-content-controller)
* [ApplicationState](#user-content-applicationstate)
* [Routing](#user-content-routing)

#### Model
Though it is expected that the model will contain all of any specific project's domain models, business logic, decision support, services, operations, and capabilities, this project ships with the very basics of a project model.

##### Dispatcher
There should always be one and only one Dispatcher in any system built from this project. The dispatcher can be found at `./src/model/ApplicationDispatcher.js`. This class is merely a wrapper on a Flux Dispatcher class intended to protect Taylor systems from risk exposure to necessary future system updates due to Flux Dispatcher API changes out of our control. The current API is 1:1 with the Flux Dispatcher. Therefore, it can be replaced with a Flux Dispatcher itself. This is discouraged due to the increased potential and unnecessary risk profile already discussed.

The Dispatcher is merely the Subject of an Observer pattern that exposes a `register` method to enable interested Observers to register for notifications. Any class that implements the register/unregister API will work in place of this class. The Flux Dispatcher is currently chosen to leverage any optimization internals provided.

##### Service
Services are responsible for exposing model internals to external dependents. Service responsibilities will include: server communications, cross-component communications, and system decisioning. Depending upon the system, they may also be responsible for overseeing and supporting the collaboration of component sub-systems and domain defining capabilities and operations. Note that the single responsibility of the Service is managing the collaboration of these components. Any domain operations exceeding the scope of collaboration should be delegated to other well defined system constructs.

All system Services should inherit from `./src/model/services/AbstractService.js`. Child classes will inherit the asynchronous server communication defined. Both the `get` and `post` methods return [Bluebird](http://bluebirdjs.com/docs/getting-started.html) Promises to their callers.
All methods outside of the root level server communications should be defined in system defined subclasses.

##### Store
Stores are responsible for storing and managing system state. They will be notified of system activity via the [Dispatcher](#user-content-dispatcher). Stores will register for notifications from the [Dispatcher](#user-content-dispatcher). Stores extend EventEmitter and can be registered to by interested classes, namely, [Views](#user-content-View). Stores will have an opportunity to handle incoming data from [Dispatcher](#user-content-dispatcher) notifications and update it's internal state accordingly before finally calling it's protected `$notifyStateChanged()` method to notify all of it's subscribing [Views](#user-content-View).

Using this project you can follow a multi-store or single-store style pattern. For those wishing to implement a multi-store solution i.e Flux, you will implement as many stores as you desire inheriting from `./src/model/store/AbstractStore` for each store implemented. For those wishing to implement a single-store solution, i.e. Redux, you can simply leverage the `./src/model/store/Store` class which merely subscribes to the [Dispatcher](#user-content-dispatcher).

#### View
Views are responsible for managing and displaying human readable screens that act as the Users Interface. They contain the markup (JSX) necessary to coordinate the structure, symantics, styling, and behavior of the User Interface. They should delegate all server communications and state management to [Services](#user-content-services) and [Stores](#user-content-stores) respectively. Though, they could be responsible for necessary User input data collection. It is recommended that this responsibility be delegated to [Controllers](#user-content-controller).

##### AbstractView And React Component Lifecycle
There are a number of methods that define the "React Component Lifecycle". These methods are abstracted away in the `AbstractView` of a react-application in effort to reveal the intentions of these methods in the "lifecycle" of a react-application. Any view that extends `AbstractView` will have access to the protected methods: `$mount`, `$unmount`, and `$prerender`. These methods should be used as follows:

* `$mount`: Implement this method in a child class to register to any dispatchers or other object that extend EventEmitter. These registrations should unregistered in `$unmount` to prevent any memory leaks.

* `$unmount`: Implement this method in a child class to unregister from any dispatchers or other object that extend EventEmitter. These unregistrations should unregister any registrations that occur in `$mount` to prevent any memory leaks.

* `$prerender`: This method will be called before any `render`. This will allow you to update the Component state via `setState()` with any new prop data received from a parent class or dependent class before this component is rendered.

#### Controller
Controllers are not a mandatory system construct, and for small systems it may be advised that all of a Controllers responsibilities be relegated to a [View](#user-content-view). For large or enterprise scale systems it is advised that Controllers be leveraged to separate the responsibilities of User data collection to be owned by Controllers from User data display to be owned by [Views](#user-content-View).

Controllers collect User input data, modify it as necessary, and send it to a [Service](#user-content-service). A common example may be a [View](#user-content-view) displaying a button on a form: when the button is invoked, the corresponding Controller will be called, the appropriate form data read, formatted, and then sent to the appropriate [Service](#user-content-service) for processing. 

#### Application State
The Application State is the root level application dependency injection mechanism and application cache found at `./ApplicationState`. The system [Dispatcher](#user-content-dispatcher), [Stores](#user-content-store), and [Controllers](#user-content-controller) should all be instantiated here. The [Dispatcher](#user-content-dispatcher) will be injected into the dependent [Services](#user-content-service) and [Stores](#user-content-store), and the [Services](#user-content-service) will be injected into the dependent [Controllers](#user-content-controller) in the Application State. The Application State exposes the [Stores](#user-content-store) and [Controllers](#user-content-controller) to the [View](#user-content-view) classes via extending `./src/views/AbstractView`

### Dependencies
This project defines an opinionated class dependency hierarchy.

```txt
Dispatcher  ←  Store
    ↑
 Service         ↑
    ↑             
Controller  ←  View

```

### Data Flow
This project enforces an opinionated unidirectional dataflow whereby a User is presented with data via a [View](#user-content-view), the user's actions and inputs are handled by a [Controller](#user-content-controller). Controllers send data to [Services](#user-content-service) to communicate with the server, operate on the domain model, and update state. The Services leverage a [Dispatcher](#user-content-dispatcher) to notify [Stores](#user-content-store) of communication and model results. [Stores](#user-content-store) update their state per the data that they receive, and notify registered [Views](#user-content-view) of their updated state.

```txt
 View   →  Controller  →   Service  ⇄  [Server]
   ⇡                          ⇣
<event>  ⇠   Store   ⇠  <Dispatcher>

```

### Routing
Due to the nature of Single Page Applications, it is the responsibility of the FE application to manage routing. In this project routing is managed in the main entry point `./src/main.jsx`. This project leverages [React Router](https://www.npmjs.com/package/react-router4) for routing.


## Conventions and Patterns

### Conventions
A few conventions to be aware of and adhere to, to aid in reasoning about systems that are modeled in an OO methodology where the language (JavaScript) does not natively support expected OO constructs.

#### Access Modifiers 
Because JavaScript exposes all methods and properties implicitly as "Public," we leverage some conventions to express our member accessibility intentions:

* **$**: Prepending member names with a "$" indicate a "Protected" member. Protected members should be accessed only by the defining class and inheriting classes. 

* **\_**: Prepending member names with a "\_" indicate a "Private" member. Private members should be accessed only by the defining class.

### Patterns
Some common patterns are implied in this project. Links for more information on these patterns:

* [Design Patterns](https://books.google.com/books/about/Design_Patterns.html?id=6oHuKQe3TjQC&printsec=frontcover&source=kp_read_button&hl=en#v=onepage&q&f=false)
* [Domain Driven Design](https://books.google.com/books?id=xColAAPGubgC&printsec=frontcover&dq=Domain+Driven+Design&hl=en&sa=X&ved=0ahUKEwjElo7_4urSAhVs0YMKHftRBwMQ6AEIHDAA#v=onepage&q=Domain%20Driven%20Design&f=false)
* [React](https://facebook.github.io/react/)
* [Flux](https://facebook.github.io/flux/)
* [Redux](http://redux.js.org/)

### Idioms
This project leverages ES6 idioms transpiled by Babel:

* [ES6](http://es6-features.org/#Constants)


## Development
This project promotes modular and test driven development workflow. Although this project does support hot load development by default, it is encouraged that developers leverage the built-in test driven workflow to maximize code quality and stability, simplify feature extention and maintenance, and ensure that features implemented correspond directly with feature requirements driven by product owners.

Before beginning development of a feature(s) it is important for a developer to fully understand the requirements and how those requirements can be defined by a set of tests. To ensure maximal code stability and quality it is suggested to _**not**_ begin development on a feature(s) whose requirements cannot be so defined.

This section will assume a given arbitrary feature or set of features that can be defined by a cohesive test plan and are, therefore, ready for development.

### Tests
All tests for a project are found in the `./test` directory. This directory splits tests into [Unit Tests](#user-content-unit-tests) for coverage of all system units, and [End-To-End Tests](#user-content-end-to-end-tests) to cover the application as a whole and the defined workflow scenarios as they run in a given browser.

#### Unit Tests
Unit tests are intended to cover individual units of functionality at an arbitrarily minimal aggregate. A common and natural arbitrary aggregate is a class. It is likely that a given project will have one test file per class file. For example, a class `./src/model/operation/ExampleClass.js` will have a corresponding Unit Test `./test/ut/model/operation/ExampleClass.test.js`. It is the intention of the unit test to confirm expected outcomes for all expected scenarios.

##### Unit Test Development
This project leverages [Mocha](https://mochajs.org/) for Unit Testing, and expands on it by providing helper modules, where possible, to contain common testing constructs. These helper constructs assume adherence to the APIs and patterns defined in this project.

* [AbstractNotificationStub](#user-content-abstractnotificationstub)
* [DispatcherStub](#user-content-dispatcherstub)
* [ServiceTest](#user-content-servicetest)
* [ViewTest](#user-content-viewtest)

###### AbstractNotificationStub
The `./test/stubs/AbstractNotificationStub` exposes a common Observer Pattern API found in this project. It encapsulates the shared API between [DispatcherStub](#user-content-dispatcherstub) and [ServiceTest](#user-content-servicestub)

###### DispatcherStub
The `/test/stubs/DispatcherStub` enables developers to hook into the dispatching mechanism and collect data from internal and "inaccessible" class calls to dispatch. This allows the test to assert data through those constructs that depend upon a [Dispatcher](#user-content-dispatcher). Example: `./test/ut/model/services/ExampleService.test`.

###### ServiceTest
The `./test/ut/model/services/ServiceTest` encapsulates an asynchronous server call mock that can be leveraged to make assertions about expected data returned to the target service from a server. All service tests that extend ServiceTest receive this functionality. Example: `./test/ut/model/service/ExampleService.test`.

###### ViewTest
The `./test/ut/views/ViewTest` encapsulates behaviors necessary for testing a React Component and exposes a common JQuery-style means of querying the test DOM to simplify test assertions. The common test flow for a view test will be to first test the rendering of the component, then to test all expected state changes under all relevant scenarios. Example: `./test/ut/views/ViewTest`.

##### Running Unit Tests

###### Run All Unit Tests

```sh
npm test

```

###### Run One Unit Test File

```sh
npm run test-file -- <PATH>

```

###### Run A Single Test
1. Navigate to the test that you wish to isolate, and append it with .only: `it.only('TEST NAME', () => { /*assertions*/ }`
2. [Run All Unit Tests](#user-content-run-all-unit-tests) or [Run One Unit Test File](#user-content-run-one-unit-test-file).

_**WARNING: Do not forget to remove .only from your isolated test once you are finished isolating said test, or your test coverage will fail!**_

###### Code Coverage
There are two commands to investigate code coverage:

####### Test Coverage Overview

```sh
npm run test-coverage

```

####### Test Coverage Details

```sh
npm run test-coverage-details

```
_*Note that currently test-coverage-details is set up to run in a Chrome browser on a Mac, and assumes default Chrome installation._

#### End-To-End Tests
End-to-end tests are driven by Selenium. All scenarios for the target application should be defined in `./test/e2e/scenarios/example/`. Due to known race conditions there are no helper modules for end-to-end testing. Instead, there is a template at `./test/e2e/scenarios/Scenario.temp.js`. This template should be used for all scenario testing. It will set up the necessary drivers. Example: `./test/e2e/scenarios/example/ExampleScenario`.

_*Note: that the Selenium Web Driver pattern is a modified Promise pattern. There are some deviations from the Promises/A+ definition discovered during development. [This](https://marmelab.com/blog/2016/04/19/e2e-testing-with-node-and-es6.html) is a nice post that discusses some of these quirks._

##### Run All End-To-End Tests
You can run all of the end-to-end tests in a project by runnning the following npm command followed by that target browser.

```sh
npm run test-e2e -- [chrome|firefox|safari]

```

##### Troubleshooting

###### Cannot Find My Element

Selenium can fail such that the target browser actually opens but cannot connect to your server. This can cause valid markup to be displayed in the target browser, but that markup is error content. It is very difficult to distinguish that this is the case. You can run the following to see this markup to discover if this is your issue.

```javascript
driver.navigate().to('<URL>')
  .then(() => { return driver.findElement(By.tagName('div')) })
  .then((element) => { return element.getAttribute('innerHTML') })
  .then((html) => { console.log(html) });

```

### Source Code
The project source code is found in `./src`. As the name would suggest, all project source code should be located in this directory and its subdirectories. It is the code in this directory that will be compiled into a deployable release unit artifact.

#### Directories
The source code should be organized into a logical directory hierarchy. A common and reasonable hierarchy follows. This project assumes this organization:

```txt
src
+-- controllers
+-- model
|   +-- capabilities
|   +-- decisionSupport
|   +-- operation
|   +-- services
|   +-- stores
|
+-- styles
|   +-- base
|   +-- views
|
+-- views

```

### Running Locally
The project can be run locally for development by running:

```sh
npm start

```

This will start a local server running at `http://localhost:3001`. Note that this server is set up for hot-reloads. This means that you can leave this server running and as you make changes to your project and save them, the browser will automatically reload to display those changes.

## Deployment
A deployment artifact can be generated by compiling the application source using the following command. The deployment artifact is built and saved to `./dist`

```sh
npm run compile

```

## Browser Support
This project is intended to build FE applications that support the following browsers:

| Browser | Version |
| --- | :---: |
| Chrome | Latest |
| Firefox | Latest |
| Safari | Latest |
| Edge | Latest |
| IE | 9+ |

## Resources
* https://github.com/ReactTraining/react-router/blob/master/packages/react-router/docs/guides/blocked-updates.md
