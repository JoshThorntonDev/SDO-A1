# Simple Todo App with MongoDB, Express.js and Node.js
The ToDo app uses the following technologies and javascript libraries:
* MongoDB
* Express.js
* Node.js
* express-handlebars
* method-override
* connect-flash
* express-session
* mongoose
* bcryptjs
* passport
* docker & docker-compose

## What are the features?
You can register with your email address, and you can create ToDo items. You can list ToDos, edit and delete them. 

# How to use
First install the depdencies by running the following from the root directory:

```
npm install --prefix src/
```

To run this application locally you need to have an insatnce of MongoDB running. A docker-compose file has been provided in the root director that will run an insatnce of MongoDB in docker. TO start the MongoDB from the root direction run the following command:

```
docker-compose up -d
```

Then to start the application issue the following command from the root directory:
```
npm run start --prefix src/
```

The application can then be accessed through the browser of your choise on the following:

```
localhost:5000
```

## Testing

Basic testing has been included as part of this application. This includes unit testing (Models Only), Integration Testing & E2E Testing.

### Linting:
Basic Linting is performed across the code base. To run linting, execute the following commands from the root directory:

```
npm run test-lint --prefix src/
```



The code coverage report can be found within the artifacts tab of the ci-build job. It can be viewed by clicking the artefact "src/coverage/lcov-report/index.html". It will then present you with a table showing code coverage.

### Static Code Analysis
This feature automatically detects potential security issues and causes ci builds to fail when it does.
This helps to prevent accidental deployment of potentially serious security flaws into the production branch.

This is accomplished through the 'sast' job defined in .circleci/config.yml
When the build-and-package workflow is triggered, and after ci-build has finished, sast installs python3-pip and uses that to install nodejsscan, the tool being used to generate a security report.

Nodejsscan creates a sast-output.json file, which contains information on things such as the files nodejsscan has scanned and most importantly, the number of security issues it found.

The sast job then uses jquery to read sast-output-json and return an exit code equal to the number of security issues. An exit code of 0 (no issues), is good. A code other than 0 will cancel the workflow.


### Unit Testing
Unit testing is important, as it allows us to identify bugs in the code we have written. However, if they have to be run
manually, it is possible that someone may forget to run them, thus letting a bug into production code. If they are run automatically, there is no chance that they will be forgotten.

Automated unit testing is accomplished within the ci-build job defined in .circleci/config.yml
ci-build is triggered by the build-and-package workflow, which runs whenever a change is made to master or a pull request is made.
ci-build first does some preliminary tasks; installing dependencies and running the 'checkout' command. Then, it automatically runs the following command:
```
npm run test-unit --prefix src/ -- --coverage
```
This generates a junit.xml file containing the results of the tests. This file is stored within the folder specified with JEST_JUNIT_OUTPUT_DIR, and uploaded to circleci using store_test_results

### Code Coverage
Code Coverage allows us to know how much of our code is being tested by our tests.
It lets us know if we are neglecting our test writing as we develop new features.

It works through the '-- --coverage' found in 'Run unit tests' in ci-build. This argument causes jest to save a code coverage report as it conducts its tests. The report is saved in the ARTIFACTS tab through store_artifacts in ci-build.

### Artefact Generation
Generating a deployable version of the application takes time. If it needs to be done manually, when a change is made, someone may forget to build a new version with the updated code, wasting even more time.

Artefact generation is automated with the 'pack' job defined in .circleci/config.yml, it executes after ci-build has finished, using 'requires'. It also only activates on the master branch because of the filter applied. This is to prevent needlessly building the application on test branches.

### Integration Testing
Integration testing is included to ensure the applicaiton can talk to the MongoDB Backend and create a user, redirect to the correct page, login as a user and register a new task. 

Note: MongoDB needs to be running locally for testing to work (This can be done by spinning up the mongodb docker container).

To perform integration testing execute the following commands from the root directory:

```
npm run test-integration --prefix src/
```

### E2E Tests
E2E Tests are included to ensure that the website operates as it should from the users perspective. E2E Tests are executed in docker containers. To run E2E Tests execute the following commands:

```
chmod +x scripts/e2e-ci.sh
./scripts/e2e-ci.sh
```


###### This project is licensed under the MIT Open Source License
