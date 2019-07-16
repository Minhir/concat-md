---
id: module
title: Module
sidebar_label: Module
---

# Class: Module

Easy file operations in node.js modules and auto logging to help building zero-config boilerplates, postinstall and other scripts.

## Hierarchy

* **Module**

## Properties

###  config

• **config**: *`Config`*

*Defined in [module.ts:70](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L70)*

Config of the module. Configuration system uses [cosmiconfig](https://github.com/davidtheclark/cosmiconfig).
Config name is determined using `configName` constructor parameter. For target module this is the name of source module.

For example your module (source module) is named `my-boilerplate` and `my-project` uses by installing `my-boilerplate`,
then `my-project/.my-boilerplate.rc.json` (or any cosmiconfig supported file name) configuration file located in root of `my-project` is used.

___

###  package

• **package**: *`JSONObject`*

*Defined in [module.ts:56](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L56)*

JavaScript object created from the module's `package.json`.

___

###  root

• **root**: *string*

*Defined in [module.ts:51](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L51)*

Absolute path of the module's root directory, where `package.json` is located.

___

### `Optional` tsConfig

• **tsConfig**? : *`JSONObject`*

*Defined in [module.ts:61](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L61)*

JavaScript object created from the module's `tsconfig.json` if exists.

## Accessors

###  isCompiled

• **get isCompiled**(): *boolean*

*Defined in [module.ts:207](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L207)*

Whether project is a compiled project via TypeScript or Babel.
**Note that, currently this method simply checks whether module is a TypeScript project or `babel-cli`, `babel-preset-env` is a dependency in `package.json`.**

**Returns:** *boolean*

___

###  isTypeScript

• **get isTypeScript**(): *boolean*

*Defined in [module.ts:214](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L214)*

Whether module is a TypeScript project.

**Returns:** *boolean*

___

###  name

• **get name**(): *string*

*Defined in [module.ts:176](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L176)*

Name of the module as defined in `package.json`.

**Returns:** *string*

___

###  nameWithoutUser

• **get nameWithoutUser**(): *string*

*Defined in [module.ts:187](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L187)*

Name of the module without user name.

#### Example
```typescript
const name = module.name(); // @microsoft/typescript
const safeName = module.nameWithoutUser(); // typescript
```

**Returns:** *string*

___

###  safeName

• **get safeName**(): *string*

*Defined in [module.ts:199](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L199)*

Safe project name, generated by deleting "@" signs from module name and replacing "/" characters with `-`.
Useful for npm packages whose names contain user name such as `@microsoft/typescript`.

#### Example
```typescript
const name = module.name(); // @microsoft/typescript
const safeName = module.safeName(); // microsoft-typescript
```

**Returns:** *string*

## Methods

###  bin

▸ **bin**(`bin`: string): *string*

*Defined in [module.ts:481](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L481)*

Searches path of the `executable` located in `node_modules/.bin` relative to current working directory (`cwd`).

#### Example
```typescript
module.bin("my/command"); // ./from/CWD//to/module/node_modules/.bin/my/command
```

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`bin` | string | is the name or path of the bin relative to `node_modules/.bin`; |

**Returns:** *string*

path relative to cwd().

___

###  executeAllSync

▸ **executeAllSync**(...`commands`: [SerialCommands](../README.md#serialcommands)): *[CommandResults](commandresults.md)*

*Defined in [module.ts:600](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L600)*

Executes multiple commands serially using [Execa](https://www.npmjs.com/package/execa) or parallel using [concurrently](https://www.npmjs.com/package/concurrently).
By default commands are executed serially. If commands are provided in an object they are executed concurrently. (keys are names, values are commands).
To provide execa options to concurrently commands use [executeAllWithOptionsSync](module.md#executeallwithoptionssync). If command is `undefined` or `null`, it will be skipped. This is useful for
conditional commands.

#### Example
```typescript
module.executeAllSync("ls", ["ls", ["-al"]]); // Run: `ls` then `ls -al`.
module.executeAllSync({ "ls1": "ls", "ls2": ["ls", ["-al"]] }); // Run `ls` and `ls -al` concurrently.
module.executeAllSync("ls", { "ls2": ["ls", ["-a"]], "ls3": ["ls", ["-al"]] }); // Run `ls` then `ls -a` and `ls -al` concurrently.
const result = module.executeAllSync(
  [
    "serialCommand1", ["arg"]],
    "serialCommand2",
    someCondition ? "serialCommand3" : null,
    {
      myParallelJob1: ["someCommant4", ["arg"],
      myParallelJob2": "someCommand4"
      builder: ["tsc", ["arg"]],
    },
    [
      "other-serial-command", ["arg"]
    ],
  ]
);
```

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`...commands` | [SerialCommands](../README.md#serialcommands) | are list of commands to execute. |

**Returns:** *[CommandResults](commandresults.md)*

[[CommandResults]] instance, which contains results of the executed commands.

___

###  executeAllWithOptionsSync

▸ **executeAllWithOptionsSync**(`options`: [ExecuteAllSyncOptions](../interfaces/executeallsyncoptions.md), ...`commands`: [SerialCommands](../README.md#serialcommands)): *[CommandResults](commandresults.md)*

*Defined in [module.ts:611](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L611)*

Executes multiple commands with given options. See [executeAllSync](module.md#executeallsync) fro details.

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`options` | [ExecuteAllSyncOptions](../interfaces/executeallsyncoptions.md) | are options to pass to [Execa](https://www.npmjs.com/package/execa) executing [concurrently](https://www.npmjs.com/package/concurrently). |
`...commands` | [SerialCommands](../README.md#serialcommands) | are list of commands to execute. |

**Returns:** *[CommandResults](commandresults.md)*

[[CommandResults]] instance, which contains results of the executed commands.

___

###  executeSync

▸ **executeSync**(`bin`: string, `args?`: string[], `options?`: `SyncOptions`): *`ExecaSyncReturnValue`*

*Defined in [module.ts:559](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L559)*

Executes given command using `spawn.sync` with given arguments and options.

#### Example
```typescript
module.executeSync("ls"); // Run `ls`.
module.executeSync("ls", ["-al"]); // Run `ls -al`.
```

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`bin` | string | is binary file to execute. |
`args?` | string[] | are arguments to pass to executable. |
`options?` | `SyncOptions` | are passed to [Execa](https://www.npmjs.com/package/execa). |

**Returns:** *`ExecaSyncReturnValue`*

[[ExecaSyncReturnValue]] instance.

▸ **executeSync**(`bin`: string, `args?`: string[], `options?`: `SyncOptions<null>`): *`ExecaSyncReturnValue<Buffer>`*

*Defined in [module.ts:560](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L560)*

**Parameters:**

Name | Type |
------ | ------ |
`bin` | string |
`args?` | string[] |
`options?` | `SyncOptions<null>` |

**Returns:** *`ExecaSyncReturnValue<Buffer>`*

▸ **executeSync**(`bin`: string, `options?`: `SyncOptions`): *`ExecaSyncReturnValue`*

*Defined in [module.ts:561](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L561)*

**Parameters:**

Name | Type |
------ | ------ |
`bin` | string |
`options?` | `SyncOptions` |

**Returns:** *`ExecaSyncReturnValue`*

▸ **executeSync**(`bin`: string, `options?`: `SyncOptions<null>`): *`ExecaSyncReturnValue<Buffer>`*

*Defined in [module.ts:562](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L562)*

**Parameters:**

Name | Type |
------ | ------ |
`bin` | string |
`options?` | `SyncOptions<null>` |

**Returns:** *`ExecaSyncReturnValue<Buffer>`*

___

###  existsSync

▸ **existsSync**(`pathInModule`: string): *boolean*

*Defined in [module.ts:404](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L404)*

Checks whether given path exists.

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`pathInModule` | string | is file/directory path relative to module root. |

**Returns:** *boolean*

whether given path exists.

___

###  getDataFileSync

▸ **getDataFileSync**(`pathInModule`: string, `__namedParameters`: object): *[DataFile](datafile.md)*

*Defined in [module.ts:432](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L432)*

Gets [DataFile](datafile.md) for `pathInModule` file. [DataFile](datafile.md) provide useful utilities to work with data files.

**Parameters:**

▪ **pathInModule**: *string*

is file path relative to module root.

▪`Default value`  **__namedParameters**: *object*=  {} as any

Name | Type | Default | Description |
------ | ------ | ------ | ------ |
`defaultFormat` | "json" \| "yaml" | "json" | is default file format to be used during file creation if file is not found and format cannot be inferred from file extension. |

**Returns:** *[DataFile](datafile.md)*

[[DataFile]] instance for `pathInModule`.

___

###  getDependencyVersion

▸ **getDependencyVersion**(`moduleName`: string, `dependencyTypes`: [DependencyType](../enums/dependencytype.md)[]): *string | undefined*

*Defined in [module.ts:232](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L232)*

Fetches a dependent module's version.

**Parameters:**

Name | Type | Default | Description |
------ | ------ | ------ | ------ |
`moduleName` | string | - | is the name of the module to get version of. |
`dependencyTypes` | [DependencyType](../enums/dependencytype.md)[] |  [
      DependencyType.Dependencies,
      DependencyType.DevDependencies,
      DependencyType.PeerDependencies,
      DependencyType.OptionalDependencies,
    ] | are array of dependency types to search module in. |

**Returns:** *string | undefined*

version of the `moduleName`.

___

###  getPrettierConfigSync

▸ **getPrettierConfigSync**(`pathInModule`: string): *`Options` | null*

*Defined in [module.ts:464](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L464)*

Fetches prettier configuration for `pathInModule` file. Prettier uses [cosmiconfig](https://github.com/davidtheclark/cosmiconfig)
which may be cascaded. If no file path is given returns configuration located in module root.

Also adds necessary parser to configuration if not exists.

**Parameters:**

Name | Type | Default | Description |
------ | ------ | ------ | ------ |
`pathInModule` | string |  this.root | is file path relative to module root. |

**Returns:** *`Options` | null*

prettier configuration for given file.

___

###  hasAnyDependency

▸ **hasAnyDependency**(`moduleNames`: string | string[], `dependencyTypes`: [DependencyType](../enums/dependencytype.md)[]): *boolean*

*Defined in [module.ts:261](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L261)*

Checks whether `moduleName` module exists in given [dependency types](../enums/dependencytype.md) of `package.json`.

**Parameters:**

Name | Type | Default | Description |
------ | ------ | ------ | ------ |
`moduleNames` | string \| string[] | - | - |
`dependencyTypes` | [DependencyType](../enums/dependencytype.md)[] |  [
      DependencyType.Dependencies,
      DependencyType.DevDependencies,
      DependencyType.PeerDependencies,
      DependencyType.OptionalDependencies,
    ] | are array of dependency types to search module in. |

**Returns:** *boolean*

whether `moduleName` exists in one of the dependency types.

___

###  ifAnyDependency

▸ **ifAnyDependency**<**T**, **F**>(`moduleNames`: string | string[]): *boolean*

*Defined in [module.ts:281](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L281)*

Checks single or multiple module's existence in any of the `package.json` dependencies.

**Type parameters:**

▪ **T**

▪ **F**

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`moduleNames` | string \| string[] | are Module or modules to check whether this module has any dependency. |

**Returns:** *boolean*

`trueValue` if module depends on any of the `moduleNames`. Otherwise returns `falseValue`.

▸ **ifAnyDependency**<**T**, **F**>(`moduleNames`: string | string[], `t`: `T`): *`T` | false*

*Defined in [module.ts:282](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L282)*

**Type parameters:**

▪ **T**

▪ **F**

**Parameters:**

Name | Type |
------ | ------ |
`moduleNames` | string \| string[] |
`t` | `T` |

**Returns:** *`T` | false*

▸ **ifAnyDependency**<**T**, **F**>(`moduleNames`: string | string[], `t`: `T`, `f`: `F`): *`T` | `F`*

*Defined in [module.ts:283](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L283)*

**Type parameters:**

▪ **T**

▪ **F**

**Parameters:**

Name | Type |
------ | ------ |
`moduleNames` | string \| string[] |
`t` | `T` |
`f` | `F` |

**Returns:** *`T` | `F`*

___

###  install

▸ **install**(`packageName`: string, `__namedParameters`: object): *void*

*Defined in [module.ts:644](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L644)*

Installs `packageName` node module using specified package manager.

**Parameters:**

▪ **packageName**: *string*

is the name of the package to install.

▪`Default value`  **__namedParameters**: *object*=  {} as any

Name | Type | Default | Description |
------ | ------ | ------ | ------ |
`type` | [DependencyType](../enums/dependencytype.md) |  DependencyType.Dependencies | is the dependency type of the package. `dev`, `peer`, `optional`.  |

**Returns:** *void*

___

###  isEqual

▸ **isEqual**(`pathInModule`: string, `data`: string | `Record<string, any>`): *boolean*

*Defined in [module.ts:451](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L451)*

Checks whether content of `pathInModule` file is equal to `data` by making string comparison (for strings)
or deep comparison (for objects).

#### Example
```typescript
const isConfigEqual = module.isFileEqual("config.json", { size: 4 });
const textEqual = module.isFileEqual("some.txt", "content");
```

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`pathInModule` | string | is file path relative to module root. |
`data` | string \| `Record<string, any>` | is string or JavaScript object to compare to file's content. |

**Returns:** *boolean*

whether `pathInModule` file content is equal to `data`.

___

###  parseSync

▸ **parseSync**(`pathInModule`: string): *`Record<string, any>`*

*Defined in [module.ts:328](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L328)*

Reads, parses and returns content of file from `pathInTargetModule` path relative to module's root.

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`pathInModule` | string | is file path relative to module root. |

**Returns:** *`Record<string, any>`*

parsed file content as a JavaScript object.

___

###  parseWithFormatSync

▸ **parseWithFormatSync**(`pathInModule`: string): *object*

*Defined in [module.ts:339](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L339)*

Reads and parses content of file from `pathInTargetModule` path relative to module's root
and retunrs format and parsed object.

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`pathInModule` | string | is file path relative to module root. |

**Returns:** *object*

file format and data.

* **data**: *`Record<string, any>`*

* **format**: *[FileFormat](../README.md#fileformat)*

___

###  pathOf

▸ **pathOf**(...`parts`: string[]): *string*

*Defined in [module.ts:308](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L308)*

Returns absolute path of given path parts relative to module root.

#### Example
```typescript
// In /path/to/project/node_modules/module-a/src/index.js
module.pathOf("images", "photo.jpg"); // /path/to/project/node_modules/module-a/images/photo.jpg
```

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`...parts` | string[] | are path or array of path parts relative to module root. |

**Returns:** *string*

absolute path to given destination.

___

###  readSync

▸ **readSync**(`pathInModule`: string): *string*

*Defined in [module.ts:318](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L318)*

Reads content of file from `pathInModule` path relative to module's root.

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`pathInModule` | string | is file path relative to module root. |

**Returns:** *string*

file contents.

___

###  reloadConfig

▸ **reloadConfig**(): *void*

*Defined in [module.ts:221](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L221)*

Reloads configuration. This may be useful if you create or update your configuration file.

**Returns:** *void*

___

###  removeSync

▸ **removeSync**(`pathInModule`: string, `__namedParameters`: object): *void*

*Defined in [module.ts:387](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L387)*

Removes file `pathInTargetModule` path relative to module's root.

**Parameters:**

▪ **pathInModule**: *string*

is file path relative to module root.

▪`Default value`  **__namedParameters**: *object*=  {}

Name | Type | Description |
------ | ------ | ------ |
`ifEqual` | undefined \| string \| object | allows modification if only value stored at `path` equals/deeply equals to it's value. |
`ifNotEqual` | undefined \| string \| object | allows modification if only value stored at `path` equals/deeply equals to it's value.  |

**Returns:** *void*

___

###  renameSync

▸ **renameSync**(`oldPathInModule`: string, `newPathInModule`: string, `__namedParameters`: object): *void*

*Defined in [module.ts:415](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L415)*

Renames given path.

**Parameters:**

▪ **oldPathInModule**: *string*

is old path to rename relative to module root.

▪ **newPathInModule**: *string*

is new path to rename relative to module root.

▪`Default value`  **__namedParameters**: *object*=  {}

Name | Type | Default | Description |
------ | ------ | ------ | ------ |
`overwrite` | boolean |  this._overwrite | is whether to allow rename operation if target path already exists. Silently ignores operation if overwrite is not allowed and target path exists.  |

**Returns:** *void*

___

###  resolveBin

▸ **resolveBin**(`modName`: string, `__namedParameters`: object): *string*

*Defined in [module.ts:507](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L507)*

Finds and returns path of given command, trying to following steps:
1. If it is a command defined in env.PATH, returns it's path. (PATH includes `node_modules/.bin`)
2. Searches if given node module is installed.
2.a. If executable is given, looks `bin[executable]` in module's package.json.
2.b. If no executable is given, looks `bin[module name]` in module's package.json.
2.c. If found returns it's path.
3. Throws error.
module name is used by default.

#### Example
```typescript
project.resolveBin("typescript", { executable: "tsc" }); // Searches typescript module, looks `bin.tsc` in package.json and returns it's path.
project.resolveBin("tsc"); // If `tsc` is defined in PATH, returns `tsc`s path, otherwise searches "tsc" module, which returns no result and throws error.
project.resolveBin("some-cmd"); // Searches "some-cmd" module and
```

**`throws`** Error if no binary cannot be found.

**Parameters:**

▪ **modName**: *string*

is module name to find executable from.

▪`Default value`  **__namedParameters**: *object*=  {} as any

Name | Type | Default | Description |
------ | ------ | ------ | ------ |
`cwd` | string |  process.cwd() | is current working directory. |
`executable` | string | "" | is xecutable name. (Defaults to module name) |

**Returns:** *string*

path to binary.

___

###  uninstall

▸ **uninstall**(`packageName`: string): *void*

*Defined in [module.ts:670](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L670)*

Uninstalls `packageName` node module using specified package manager.

**Parameters:**

Name | Type | Description |
------ | ------ | ------ |
`packageName` | string | is the name of the package to uninstall. |

**Returns:** *void*

___

###  writeSync

▸ **writeSync**(`pathInModule`: string, `data`: string | `Record<string, any>`, `__namedParameters`: object): *void*

*Defined in [module.ts:354](https://github.com/ozum/intermodular/blob/4dd044c/src/module.ts#L354)*

Serializes, formats and writes `data` to file `pathInTargetModule` path relative to module's root.

**Parameters:**

▪ **pathInModule**: *string*

is file path relative to module root.

▪ **data**: *string | `Record<string, any>`*

is content to write to file.

▪`Default value`  **__namedParameters**: *object*=  {} as any

Name | Type | Default | Description |
------ | ------ | ------ | ------ |
`format` | "json" \| "yaml" | "json" | is file format to be used for serializing. |
`ifEqual` | undefined \| string \| object | - | allows modification if only value stored at `path` equals/deeply equals to it's value. |
`ifNotEqual` | undefined \| string \| object | - | allows modification if only value stored at `path` not equals/deeply equals to it's value. |
`overwrite` | boolean |  this._overwrite | allows overwrite. |
`usePrettier` | boolean | true | - |

**Returns:** *void*