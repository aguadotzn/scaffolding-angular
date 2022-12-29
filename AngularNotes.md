# Personal 🅰️ notes 

A bit of everything about angular.

## Commands [Angular CLI](https://angular.io/cli)
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
An Angular application consists primarily of components and their HTML templates. Because the components and templates provided by Angular cannot be understood by the browser directly, Angular applications require a compilation process before they can run in a browser. The Angular ahead-of-time (AOT) compiler converts your Angular HTML and TypeScript code into efficient JavaScript code during the build phase before the browser downloads and executes that code.

### What's Lazy-loading? 
By default Angular applications load all the components that are imported into the main module (`app.module.ts`). This may mean loading modules that the user will not use at any time. Lazy loading is a design pattern to delay the loading of an object until the very moment of its use. That is, if we implement lazy loading in Angular we will load only the modules we need at the beginning of the application and the others on demand as we need them

## Angular coding tools
Collection of extensions/plugins/misc that I use for coding angular
- Browser extension for your favorite browser. More info [here](https://angular.io/guide/devtools)
- If you are using state in Angular you might encounter useful the [Redux dev tools extension](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=es). Again is available in any browser.
- For coding I'm using [visual studio code](https://code.visualstudio.com/). Some extensions:
  - [Angular Language service](https://marketplace.visualstudio.com/items?itemName=Angular.ng-template)
  - [Alpha order](https://marketplace.visualstudio.com/items?itemName=ue.alphabetical-sorter)


