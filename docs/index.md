## En este documento se adjunta el informe perteneciente a esta practica
Dado que codigo como tal no se ha desarrollado, adjuntare, la parte referente al index.js donde se implementa la funcionalidad. El codigo referente al action.yml del repo original. Y finalmente el action del repo para comprobar el funcionamiento de la action

### Codigo helloWorld.js

```
    name: 'Hello World'
        description: 'Creacion de actions usando codigo de Hello World'
    inputs:
        who-to-greet:  # id of input
            description: 'Who to greet'
            required: true
            default: 'World'
    outputs:
        time: # id of output
            description: 'The time we greeted you'
    runs:
        using: 'node12'
        main: 'index.js'

```

### Action.yml de repo base

```
    'use strict'

    const core = require('@actions/core');
    const github = require('@actions/github');

    try {
        const nombreUsuario = core.getInput('who-to-greet');
        const time = (new Date()).toTimeString();
        const payload = JSON.stringify(github.context.payload, undefined, 2)

        console.log(`Hello ${nombreUsuario}!`);
        core.setOutput("time", time);
        console.log(`The event payload: ${payload}`);
    } catch (error) {
        core.setFailed(error.message);
    }

```


### Action.yml de repo de uso de la action

```
    name: Using hello world
        on: [push]

    jobs:
        hello_world_job:
            runs-on: ubuntu-latest
            name: A job to say hello
            steps:
            # To use this repository's private action, you must check out the repository
            - name: Checkout
                uses: actions/checkout@v2
            - name: Hello world action step
                uses: ULL-ESIT-PL-2021/hello-js-action-ChristianTorresGonzalez@v6
                id: hello
                with:
                who-to-greet: 'Christian Torres Gonzalez'
            # Use the output from the `hello` step
            - name: Get the output time
                run: echo "The time was ${{ steps.hello.outputs.time }}"

```
