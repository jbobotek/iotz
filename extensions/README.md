### iotz extension development

iotz extensions define the behavior for predefined commands based on predefined
functions.

```
exports.detectProject = function(project_path) {
  // return true or false based on whether the project on `project_path` is a
  // match to extension. If multiple extensions return true. User has to set the
  // correct extension manually
}

exports.directCall = function(config, runCmd, command, compile_path) {
  // define the behavior for a named call
  // i.e. your extension name is `abc`
  // user might call `iotz abc`
  // what bash command you want to execute on the container?
  // check what we do with arduino, mbed, and raspberry
};

exports.createExtensions = function() {
  // what bash command you want to run on the container to prepare the environment
  // for your extension?
};

exports.build = function build(config, runCmd, command, compile_path) {
  var callback = null;
  var runString = "";

    // define things to do for `init`, `container_init`, `clean`, `compile`, and `export` commands
    // set bash stuff into `runString`
    // if you want to run any additional post init logic, set the callback = function(config)
    // `config` corresponds to `iotz.json` contents

  if (command == 'init') {
    // set init stuff here
  } else if (command == 'container_init') {
    // set container_init things here.
    // difference between `container_init` and `init` is.. `init` is a user command
    // `container_init` will be called no matter what and will be called pre init.
  } else if (command == 'clean') {
    // set what to do for `clean`
  } else if (command == 'compile') { // build
    // things for `compile`
  } else if (command == 'export') {
    // things for `export`
  } else {
    console.error(" -", console.red("error :"),
              "Unknown command", command);
    process.exit(1);
  }

  return {
    run: runString,
    callback: callback
  };
}
```

By default, iotz searches an extension under the official extensions folder and then
tries to `require` blindly. So, if you have published your `iotz` extension to npm,
user should install that extension globally to make it available to `iotz`