mongoose-tsgen
==============

A Typescript interface generator for Mongoose that works out of the box.

[![oclif](https://img.shields.io/badge/cli-oclif-brightgreen.svg)](https://oclif.io)
[![Version](https://img.shields.io/npm/v/mongoose-tsgen.svg)](https://npmjs.org/package/mongoose-tsgen)
[![Downloads/week](https://img.shields.io/npm/dw/mongoose-tsgen.svg)](https://npmjs.org/package/mongoose-tsgen)
[![License](https://img.shields.io/npm/l/mongoose-tsgen.svg)](https://github.com/francescov1/mongoose-tsgen/blob/master/package.json)

No, you don't need to rewrite your entire schemas, nor do you need to limit which Mongoose features you use (really!). This CLI works by importing your schema definitions, parsing the structure and generating an index.d.ts file.

Features:
- Automatically creates two interfaces for each exported Schema, `I{model_name}` and `I{model_name}Model` (ie IUser and IUserModel). The first can be imported across your project and used as a document type and will contain your schema properties, virtual properties and methods. The second will be used to initialize your Mongoose model and will contain any static functions.
- Any subdocuments will also be given their own interface which will be referenced in its parent interface. These can also be referenced in your project the same way as the parent interfaces. Interface names will be camel-cased with respect to the flattened property name (ie say we have an subdocument array of `friends` on `IUser`, where each subdoc contains various properties related to the friend. An interface named `IUserFriend` will be created with the document properties, and `IUser` will reference it like so: `friends: mongoose.Types.DocumentArray<IUserFriend>`).
- A section for custom interfaces and types is provided at the bottom of `index.d.ts`. This will remain untouched when re-generating the interfaces unless the `--fresh` flag is provided.

Coming Soon (most of the following features are already supported but use looser typing than likely desired):
- Enum types. Currently Mongoose enum properties are treated as strings.
- Multi-nested subdocuments. Currently subdoc interfaces are generated one level down; further nesting is compatible but is achieved using `any` for the time being.
- Methods and statics parameter types. Currently these are typed as `Function`.
- Support for `Model.Create`. Currently `new Model` must be used.
- Support for setting subdocument properties without casting to any. When setting a subdocument array, Typescript will yell at you if you try and set them directly (ie `user.friends = [{ uid, name }]`) as it expects the array to contain additional subdocument properties. For now, this can be achieved by writing `user.friends = [{ uid, name }] as any`.

Would love any help with the listed features above.

Once you've generated your index.d.ts file, all you need to do is make the following changes to your schema definitions:

TODO: model file changes

<!-- toc -->
* [Usage](#usage)
* [Commands](#commands)
<!-- tocstop -->
# Usage
<!-- usage -->
```sh-session
$ npm install -g mongoose-tsgen
$ mtgen COMMAND
running command...
$ mtgen (-v|--version|version)
mongoose-tsgen/0.0.0 darwin-x64 node-v14.3.0
$ mtgen --help [COMMAND]
USAGE
  $ mtgen COMMAND
...
```
<!-- usagestop -->
# Commands
<!-- commands -->
* [`mtgen gen [PATH]`](#mtgen-gen-path)
* [`mtgen help [COMMAND]`](#mtgen-help-command)

## `mtgen gen [PATH]`

generate mongoose type definitions

```
USAGE
  $ mtgen gen [PATH]

OPTIONS
  -f, --fresh
  -h, --help           show CLI help
  -o, --output=output  [default: ./index.d.ts]
  --dry-run
```

_See code: [src/commands/gen.ts](https://github.com/Bounced-Inc/mongoose-tsgen/blob/v0.0.0/src/commands/gen.ts)_

## `mtgen help [COMMAND]`

display help for mtgen

```
USAGE
  $ mtgen help [COMMAND]

ARGUMENTS
  COMMAND  command to show help for

OPTIONS
  --all  see all commands in CLI
```

_See code: [@oclif/plugin-help](https://github.com/oclif/plugin-help/blob/v3.2.0/src/commands/help.ts)_
<!-- commandsstop -->