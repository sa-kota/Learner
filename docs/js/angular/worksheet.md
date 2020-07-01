## Project arch
* package.json
  - A global config file with dependent assemblies, version, command scripts, etc are stored
  - Holds the references of dependent assemblies that are required, uses semantic versioning (semver).
    - ex: "protractor": "^5.4.0"  includes everything greater than this version in the same Major range (5)
    - ex: "protractor": "~5.4.0"  includes everything greater than this version in the same Minor range (5.4)
    - similarly there  >, <, =, >=, <=, - (range 1.0.0 - 1.2.0), || (or) can be used
* pacakge-lock.json
  - A file which stores the actual assemblies that are installed using package.json.
  - As the package.json uses semver, we will not know which version is actually installed. The actual installed versions are and their internal dependent assemblies and their versions are stored in package-lock.json
* tsconfig.json
  - Contains the configurations related to typescript. Ex: ECMA version, sourcemap, baseurl, etc..
  - sourcemap => true, creates a mapping between the actual js file and the compiled (bundled and minified) so that in case of debugging we can use the map file
* tslint.json (lint, or a linter, is a tool that analyzes source code to flag programming errors, bugs, stylistic errors, and suspicious constructs)
  - Contains the rules that are used to check the typescript code quality
* angular.json
  - Contains configuration that are related to the project (including the test projects)

## Testing
- Karma tests: unit tests
- Protractor tests: e2e tests. Testing end to end business functionality
- Jasmine: testing framework


## Links
- https://medium.com/@areai51/angular-2-without-node-and-typescript-f80cfc853904/[Angular 2 without Typescript and Node]. https://github.com/ILearny/angular2-tour-of-heroes-es5/[Code]
- https://github.com/sudheerj/angular-interview-questions#what-is-angular-framework/[Questions]
