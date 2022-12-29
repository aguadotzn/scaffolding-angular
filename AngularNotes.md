# Personal 🅰️ notes 

A bit of everything about angular.


## Table of Contents
1. [Useful commands](#commands)
2. [Architecture](#architecture)
3. [Best practices](#best-practices)
4. [Q&A](#q&a)
5. [Coding tools](#coding-tools)

## Commands ([Angular CLI](https://angular.io/cli))
Use this commands inside an angular project.

| Command                               | Description                             | Notes                                                                                                                                                                                                                               |
| ------------------------------------- | --------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ng new {appName} --{flagName}`       | Create a new angular project            | Useful flags:<br/> Create project without tests`--skip-tests`<br/> Specify a prefix for your entire app `--prefix {prefixName}`<br/>Create the app with routing `--routing`                                                         |
| `ng serve `                           | Run your angular project                | Useful flags: <br/>Open the browser: `-o` <br/>Specify a port `-p 666`                                                                                                                                                              |
| `ng generate {type} {name} {options}` | Generate new angular code automatically | Types:    <ul>   <li>component</li>   <li>service</li>   <li>module</li>   <li>pipe</li>   <li>class</li>   <li>interface</li>   <li>enum</li>   <li>directive</li>   <li>guard</li>   <li>service-worker</li>   <li>...</li> </ul> |
| `ng build`                            | Build your angular project              | Useful flags: <br/>Build for production `--prod`                                                                                                                                                                                    |
| `ng test`                             | Test your angular project               |                                                                                                                                                                                                                                     |
| `ng update`                           | Update your angular project             |                                                                                                                                                                                                                                     |
| `ng version`                          | Outputs Angular CLI version             |                                                                                                                                                                                                                                     |

## Architecture
The architecture is one of the most important aspects of any application. There are many ways one can structure an Angular app. Every design decision has its own set of benefits and draw-backs.  Let's take a look at the following diagram of the final architecture we want to achieve. 
![Angular architecture](https://miro.medium.com/max/1400/1*25fSJMgktZZjUmVdJNI30Q.webp)

Let's explore the different parts 
   - **CORE**: The things that are absolutely essential for the app to start. Core directory is the place where we should put singleton services, injection tokens, constants, app configurations, pipes, interceptors, guards, auth service, utils, etc. that will be used across the suite.
> Services that are specific to a feature can go in the feature folder.
   - **FEATURES**: are all organized into their own folder, they’re all self-contained and everything it’s pretty easy to find for that given feature. Business features live in this `features` directory. The idea is to make a module per feature. That module can contain components, directives, pipes, services, interfaces, enums, utils, and so on. The concept is to keep things close.
> `Feature` modules shouldn't be dependant on other modules other than the services provided by `CoreModule` and features exported by `SharedModule`
   - **SHARED**: Consider `shared` directory module as a mini-library for the UI components or for third-party components. They are not specific to a single business feature. They should be super dumb that you can take all the components, drop in another angular project, and expect to work (given the dependencies are met). This module can be then imported to each feature module.
> Do not make a giant `SharedModule`, rather granularize each atomic feature into its own module.
 
 If you have any doubt of where something goes:
 ![Angular architecture tree decision](https://miro.medium.com/max/1124/1*22skrCDLM6C3oRTuIecIng.webp)

### Personal architecture tips
Personally in my latests projects on top of previous characteristics I add a couple of layyers. This totally depends on the requirements of the specific application that you wanna build.
- **API LAYER**: contains all the _API-related_ files in a folder called `API` inside `core` folder. The idea is that only the files related to the back-end will live inside that folder
  - These services will have no business logic, their responsibility will only be to call the backend, do data transformations and return the data. 
  - There should be one service per controller under a folder with the name of the module
  - The name of the service should be `ControllerName+ ApiService` and the `fileName` `controller-name.api.service.ts`
  - In the folder `core/api/api-routes` we are going to create one file per module with all the **routes** of that module. The name of that file would be `featureX-api.routes.ts`

- **LAYOUT**:  If you have an app where you repeat the layout I strongly recommend you to create an extra feature, called `layout`, that contains the `main-layout` (shared a across al the app) and the `empty-layout` (parts of the app the doesn't requiere a layout such as the login) 
> The idea with the `main-layout` is to load the content of each page inside that layout that contains, for example, a common menu and a footer that are repeated accross the entire app.

- Sometimes I also differenciate between **components** and **pages**. For angular are exactly the same but I create a folder inside each lazy feature, one for _pages_ and one per _components_. “Page” is just terminology to identify a component that is being used as a route. Each route of the feature module has a page under `/page` folder.



#### 🔗 Key links
* [Build an epic architecture in Angular](https://tomastrajan.medium.com/how-to-build-epic-angular-app-with-clean-architecture-91640ed1656)
* [Planning the architecture of your angular app](https://itnext.io/planning-the-architecture-of-your-angular-app-a4840bfec13b)
* [Angular guide architecture](https://angular.io/guide/architecture)

## Best practices
Of course these are my **own best practices** that I'm learning on the fly with experience. I'll try to have all in mind once I'm starting a new project. Some of them are not specifically from Angular projects but could apply to any software project.
   
### LIFT principle
This principle helps you to find code quickly. That's mean: 
- _**L**ocate the easily_ 
- _**I**dentify code at a glance_ 
- _**F**lat structure as long as we can_
- _**T**ry to stay DRY([Don't repeat yourself](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself))_

### SPR: Single Principle Responsibility
A single class or module should only have a single responsibility.

### Meaningful names
It’s very important that you give good names to methods, variables, and parameters. Besides, respect the architecture of your application and name things accordingly.

- Properties and methods must be [camel case](https://en.wikipedia.org/wiki/Camel_case) (e.g. `currentUser`)
- Classes (components, services, directives...) must be upper camel case, called Pascal case (e.g. `UserComponent`)
- Components and services should have the respective suffix on the name.

### File structure conventions 🅰️
Use the shortcut `example-name.component.ts|html|css|spec` to represent those various files.

### Organization for readability (files) 🅰️
- The most important stuff goes first.
- Properties followed by methods
- Grouped and sorted (_alpha order_ as preference) 
- Consistent naming and spelling matter
- Small functions are better to read and faster to understand the purpose. If your function has more than 10 lines you need to ask yourself if it would be better to break it into smaller functions.


### Components 🅰️
- Prefixing your components is a good practice. selector: ‘app-component-name’
- Separating the HTML, CSS and TypeScript files is also a good practice. If you don't need a file (i.e. the styles), you can simply delete it.
- Inputs: declare it inside of the class and at the beginning.
- Outputs: declare it inside of the class and at the beginning. After the inputs.
- Delegate complex logic to services: If we are talking about one or two lines of logic, maybe it’s ok to leave it in the component. If not, move the logic into a service.
- Component member sequence: public methods must be declared before private ones.

### Services 🅰️
- Make services as injectable: this is necessary only when a service injects another service, but is recommended to use every time, because
you never know when the service will need to inject another one (dependency injection) and it will be hard to remember that we decided not to use the injectable decorator. The recommendation is to use always `@Injectable`

- Using services for data retrieval: invocating again the , our component must call a service to get some data. If we need to update the call [SPR](https://en.wikipedia.org/wiki/Single-responsibility_principle) in our service, the component remains the same. The component shouldn’t have to think how to get the data and it shouldn’t have to know if the data comes from an API or localStorage. The responsibility of the component is only to know that he had the necessity to call the service. The service has the duty to know where and how to get the data. So, please, avoid the temptation to call the API directly on your component.

### Extra 🅰️
- Use the power of Angular CLI
- Take advantage of Angular life cycle hooks 

  
## Q&A
General questions (with answers!)

### What's AOT ([Ahead-of-time](https://angular.io/guide/aot-compiler))? 
<details>
  <summary></summary>
An Angular application consists primarily of components and their HTML templates. Because the components and templates provided by Angular cannot be understood by the browser directly, Angular applications require a compilation process before they can run in a browser. The Angular ahead-of-time (AOT) compiler converts your Angular HTML and TypeScript code into efficient JavaScript code during the build phase before the browser downloads and executes that code.
</details>

### What's Lazy-loading? 
<details>
  <summary></summary>
  By default Angular applications load all the components that are imported into the main module (`app.module.ts`). This may mean loading modules that the user will not use at any time. Lazy loading is a design pattern to delay the loading of an object until the very moment of its use. That is, if we implement lazy loading in Angular we will load only the modules we need at the beginning of the application and the others on demand as we need them
</details>

## Coding tools
Collection of extensions/plugins/misc that I use for coding angular
- Browser extension for your favorite browser. More info [here](https://angular.io/guide/devtools)
- If you are using any state management library in Angular you might encounter useful the [Redux dev tools extension](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=es). Again is available in any browser.
- For coding I'm using [visual studio code](https://code.visualstudio.com/). Some extensions:
  - [Angular Language service](https://marketplace.visualstudio.com/items?itemName=Angular.ng-template)
  - [Alpha order](https://marketplace.visualstudio.com/items?itemName=ue.alphabetical-sorter)


