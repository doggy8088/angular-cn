# Workspace and project file structure

# 工作空间与项目文件的结构

You develop apps in the context of an Angular [workspace](guide/glossary#workspace). A workspace contains the files for one or more [projects](guide/glossary#project). A project is the set of files that comprise a standalone app, a library, or a set of end-to-end (e2e) tests. 

你要在 Angular [工作空间](guide/glossary#workspace)的上下文环境中开发应用。每个工作空间中包括一个或多个[项目](guide/glossary#project)的文件。每个项目是一组文件，由标准应用、库或一组端到端（e2e）测试组成。

The Angular CLI command `ng new <project_name>` gets you started. 
When you run this command, the CLI installs the necessary Angular npm packages and other dependencies in a new workspace, with a root folder named *project_name*. 
It also creates the following workspace and starter project files:

* An initial skeleton app project, also called *project_name* (in the `src/` subfolder).
* An end-to-end test project (in the `e2e/` subfolder).
* Related configuration files.

The initial app project contains a simple Welcome app, ready to run. 

## Workspace files

The top level of the workspace contains a number of workspace-wide configuration files.

| WORKSPACE CONFIG FILES    | PURPOSE |
| :--------------------- | :------------------------------------------|
| `.editorconfig`        | Configuration for code editors. See [EditorConfig](https://editorconfig.org/). |
| `.gitignore`           | Specifies intentionally untracked files that [Git](https://git-scm.com/) should ignore. |
| `angular.json`        | CLI configuration for all projects in the workspace, including configuration options for build, serve, and test tools that the CLI uses, such as [Karma](https://karma-runner.github.io/) and [Protractor](http://www.protractortest.org/).  |
| `node_modules`         | Provides [npm packages](guide/npm-packages) to the entire workspace. |
| `package.json`        | Lists package dependencies. See [npm documentation](https://docs.npmjs.com/files/package.json) for the specific format and contents of this file.|
| `tsconfig.app.json`   | Default [TypeScript](https://www.typescriptlang.org/) configuration for apps in the workspace. |
| `tsconfig.spec.json`  | Default TypeScript configuration for e2e test apps in the workspace. |
| `tslint.json`         | Default [TSLint](https://palantir.github.io/tslint/) configuration for apps in the workspace. |
| `README.md`            | Introductory documentation. |
| `package-lock.json`    | Provides version information for all packages installed into `node_modules` by the npm client. See [npm documentation](https://docs.npmjs.com/files/package-lock.json) for details. If you use the yarn client, this file will be [yarn.lock](https://yarnpkg.com/lang/en/docs/yarn-lock/) instead. |

All projects within a workspace share this configuration context. 
Project-specific [TypeScript](https://www.typescriptlang.org/) configuration files inherit from the workspace-wide `tsconfig.*.json`, and app-specific [TSLint](https://palantir.github.io/tslint/) configuration files inherit from the workspace-wide `tslint.json`.

### Default app project files

The CLI command `ng new my-app` creates a workspace folder named "my-app" and generates a new app skeleton. 
This initial app is the *default app* for CLI commands (unless you change the default after creating additional apps). 

A newly generated app contains the source files for a root module, with a root component and template. 
When the workspace file structure is in place, you can use the `ng generate` command on the command line to add functionality and data to the initial app.

<div class="alert is-helpful>

 Besides using the CLI on the command line, You can also use an interactive development environment like [Angular Console](https://angular.console.com), or manipulate files directly in the app's source folder and configuration files.

</div>

The `src/` subfolder contains the source files (app logic, data, and assets), along with configuration files for the initial app.
Workspace-wide `node_modules` dependencies are visible to this project.

| APP SOURCE & CONFIG FILES    | PURPOSE |
| :--------------------- | :------------------------------------------|
| `app/`                 | Contains the component files in which your app logic and data are defined. See details in [App source folder](#app-src) below. |
| `assets/`              | Contains image files and other asset files to be copied as-is when you build your application. | 
| `environments/`        | Contains build configuration options for particular target environments. By default there is an unnamed standard development environment and a production ("prod") environment. You can define additional target environment configurations. |
| `browserlist`          | Configures sharing of target browsers and Node.js versions among various front-end tools. See [Browserlist on GitHub](https://github.com/browserslist/browserslist) for more information.  |
| `favicon.ico`          | An icon to use for this app in the bookmark bar. |
| `index.html`           | The main HTML page that is served when someone visits your site. The CLI automatically adds all JavaScript and CSS files when building your app, so you typically don't need to add any `<script>` or` <link>` tags here manually. |
| `main.ts`              | The main entry point for your app. Compiles the application with the [JIT compiler](https://angular.io/guide/glossary#jit) and bootstraps the application's root module (AppModule) to run in the browser. You can also use the [AOT compiler](https://angular.io/guide/aot-compiler) without changing any code by appending the `--aot` flag to the CLI `build` and `serve` commands. |
| `polyfills.ts`         | Provides polyfill scripts for browser support. |
| `styles.sass`          | Lists CSS files that supply styles for a project. The extension reflects the style preprocessor you have configured for the project. |
| `test.ts`              | The main entry point for your unit tests, with some Angular-specific configuration. You don't typically need to edit this file. |
| `tsconfig.app.json`   | Inherits from the workspace-wide `tsconfig.json` file. |
| `tsconfig.spec.json`  | Inherits from the workspace-wide `tsconfig.json` file. |
| `tslint.json`         | Inherits from the workspace-wide `tslint.json` file. |

### Default app project e2e files

An `e2e/` subfolder contains configuration and source files for a set of end-to-end tests that correspond to the initial app.
Workspace-wide `node_modules` dependencies are visible to this project.

<code-example language="none" linenums="false">
my-app/
  e2e/                  (end-to-end test app for my-app)
    src/                (app source files)
    protractor.conf.js  (test-tool config)
    tsconfig.e2e.json   (TypeScript config inherits from workspace tsconfig.json)
</code-example>

### Project folders for additional apps and libraries

When you generate new projects in a workspace, 
the CLI creates a new *workspace*`/projects` folder, and adds the generated files there.

When you generate an app (`ng generate application my-other-app`), the CLI adds folders under `projects/` for both the app and its corresponding end-to-end tests. Newly generated libraries are also added under `projects/`.

<code-example language="none" linenums="false">
my-app/
  ...
  projects/           (additional apps and libs)
    my-other-app/     (a second app)
      src/
      (config files)
    my-other-app-e2e/  (corresponding test app) 
      src/
      (config files)
    my-lib/            (a generated library)
      (config files)
</code-example>

{@a app-src}
## App source folder

Inside the `src/` folder, the `app/` folder contains your app's logic and data. Angular components, templates, and styles go here. An `assets/` subfolder contains images and anything else your app needs. Files at the top level of `src/` support testing and running your app.

<code-example language="none" linenums="false">
   src/
    app/
        app.component.css
        app.component.html
        app.component.spec.ts
        app.component.ts
        app.module.ts
        assets/...
    ...
</code-example>

| APP SOURCE FILES | PURPOSE |
| :-------------------------- | :------------------------------------------|
| `app/app.component.ts`      | Defines the logic for the app's root component, named `AppComponent`. The view associated with this root component becomes the root of the [view hierarchy](guide/glossary#view-hierarchy) as you add components and services to your app. |
| `app/app.component.html`    | Defines the HTML template associated with the root `AppComponent`. |
| `app/app.component.css`     | Defines the base CSS stylesheet for the root `AppComponent`. |
| `app/app.component.spec.ts` | Defines a unit test for the root `AppComponent`. |
| `app/app.module.ts`         | Defines the root module, named `AppModule`, that tells Angular how to assemble the application. Initially declares only the `AppComponent`. As you add more components to the app, they must be declared here. |
| `assets/*`                  | Contains image files and other asset files to be copied as-is when you build your application. |