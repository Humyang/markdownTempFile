Grunt-项目脚手架


# grunt-init

Grunt-init 是一个脚手架工具用作自动化创建项目。它将根据当前环境设置和你对一些问题的答案构建整个目录结构。

通过模版创建的精确的文件和内容是根据你的回答来进行选择。

# Installation

为了使用 `grunt-init`,你需要以全局模式安装：

```javascript

npm install -g grunt-init

```

这个命令把 `grunt-init` 命令安装在你的系统路径，允许它在任意位置运行。

> **注意：**你可能需要以 sudo 或以管理员运行该命令。

# 使用方式

* 获取帮助和可用的模版列表 `grunt-init --help`
* 基于可用的模版创建项目 `grunt-init TEMPLATE`
* 基于任意位置的模版创建项目 `grunt-init /path/to/TEMPLATE`

注意大部分模版会在当前目录生成文件，如果你不想覆盖当前目录的文件请确认已更改到新目录。

# 安装模版

当模版已安装到你的 `~/.grunt-init/` 目录 (`%USERPROFILE%\.grunt-init\` on Windows) 它们可以在 grunt-init 使用。我们推荐你使用 git clone 模版到目录，例如 `grunt-init-jquery` 模版可以这样安装:

```javascript

git clone https://github.com/gruntjs/grunt-init-jquery.git ~/.grunt-init/jquery

```

注意：如果你想使模版的可用路径改为 “foobarbaz”，你可以在 cloning 时指定 `~/.grunt-init/foobarbaz`。Grunt-init 将会使用实际的目录替代 `~/.grunt-init/`。

这里提供一些官方正式的模版：

* [grunt-init-commonjs](https://github.com/gruntjs/grunt-init-commonjs) - Create a commonjs module, including Nodeunit unit tests. (sample "generated" repo | creation transcript)
* [grunt-init-gruntfile](https://github.com/gruntjs/grunt-init-gruntfile) - Create a basic Gruntfile. (sample "generated" repo | creation transcript)
* [grunt-init-gruntplugin](https://github.com/gruntjs/grunt-init-gruntplugin) - Create a Grunt plugin, including Nodeunit unit tests. (sample "generated" repo | creation transcript)
* [grunt-init-jquery](https://github.com/gruntjs/grunt-init-jquery) - Create a jQuery plugin, including QUnit unit tests. (sample "generated" repo | creation transcript)
* [grunt-init-node](https://github.com/gruntjs/grunt-init-node) - Create a Node.js module, including Nodeunit unit tests. (sample "generated" repo | creation transcript)


# 自定义模版

你可以创建和使用自定义模版。你的模版必须跟随前面模版的结构。

一个名为 `my-template` 的模版,将会生成这些文件结构：

* `my-template/template.js` - 主要的模版文件
* `my-template/rename.json` - 指定重命名规则，处理为模版.
* `my-template/root/` - 复制到目标路径的文件.

假设这些文件在 `/path/to/my-template`，命令 `grunt-init /path/to/my-template` 将会处理成模版。多个唯一命名的模版可以位于同一目录下。

此外，如果你将自定义模版放置在 `~/.grunt-init/` 目录它自动成为模版，可以通过 `grunt-init my-template` 使用。

## 复制文件

在模版主要文件使用 `init-filesToCopy` 和 `init.copyAndProcess` 方法，在运行 `grunt-init template` 时 `root/` 子目录的所在文件都会复制到当前目录，。

注意所有复制的文件都会针对 `props` 数据对象集合处理文件内的 `{%  %}`，除非设置了 `noProcess` 选项。见 [jquery template](https://github.com/gruntjs/grunt-init-jquery) 的做法。

## 重命名或排除模版文件

在 `rename.json` 描述文件从 `sourcepath` 复制到 `destpath` 过程中的重命名规则。`sourcepath` 必须是相对于 `root/` 文件夹的文件复制路径，但 `destpath` 值可以包含 `{% %}`，描述目标路径的位置。

如果对 `destpath` 指定为 `false` 文件将不会被复制。同时，通配符表达式对 `srcpath` 支持。


# 指定默认提示回答

每一个初始提示都可以是硬编码字符或者根据当前环境决定的默认值。如果你想覆盖特定的默认提示值，you can do so in the optional OS X or Linux `~/.grunt-init/defaults.json` or Windows `%USERPROFILE%\.grunt-init\defaults.json` file.

例如，我的 `default.json` 文件是这样的，因为我想使用比默认名称稍微不同的名称，我想排除我的 Email 地址，并且我想自动指定作者 url。

```javascript

{
  "author_name": "\"Cowboy\" Ben Alman",
  "author_email": "none",
  "author_url": "http://benalman.com/"
}

```

说明：在文档中有所有的内置提示，你可以在 [source code](https://github.com/gruntjs/grunt-init/blob/master/tasks/init.js) 中找到它们的名称和默认值。


# 定义一个初始模版


## exports.description

当用户运行 grunt init 或 grunt-init 显示可用的初始模版列表时会在模版列表后见显示对模版简要的描述。

```javascript

exports.description = descriptionString;

```

## exports.notes

如果指定了，在模版选项提示显示之前会显示该拓展描述模版的选项。在这里可以给用户一些命名上约定的帮助。对于提示来说是可选的。

```javascript

exports.notes = notesString;

```

## exports.warnOn

这是可选的选项 (推荐使用)，传入通配符模式或通配符数组进行匹配，如果匹配 Grunt 将中断并产生一个警告，用户可以通过 `--force` 强制执行。这在模版可能覆盖文件时有用。

语法：

```javascript

exports.warnOn = wildcardPattern;

```

最常用的值是 `*`，适配任意文件和目录，[minimatch](https://github.com/isaacs/minimatch) 通配符模式语法使用非常灵活，例如：

```javascript

exports.warnOn = 'Gruntfile.js';    // Warn on a Gruntfile.js file.
exports.warnOn = '*.js';            // Warn on any .js file.
exports.warnOn = '*';               // Warn on any non-dotfile or non-dotdir.
exports.warnOn = '.*';              // Warn on any dotfile or dotdir.
exports.warnOn = '{.*,*}';          // Warn on any file or dir (dot or non-dot).
exports.warnOn = '!*/**';           // Warn on any file (ignoring dirs).
exports.warnOn = '*.{png,gif,jpg}'; // Warn on any image file.

// This is another way of writing the last example.
exports.warnOn = ['*.png', '*.gif', '*.jpg'];

```

## exports.template

`exports` 的所有属性定义在该函数之外，所有实际的初始化代码指定在该函数内。

有三个参数传递到该函数，`grunt` 参数是对 grunt 的引用，包含所有 [grunt methods and libs](http://gruntjs.com/api/grunt)。`init` argument 是一个对象包含指定给该模版的方法和属性。`done` 参数是一个函数当模版初始话化成后必须执行，也可以用来中断初始话，例如传入敏感项目名 jQuery 等。

```javascript

exports.template = function(grunt, init, done) {
  // See the "Inside an init template" section.
};

```

# Inside an init template

## init.addLicenseFiles

添加 license 文件的属性名到 files object。

```javascript

var files = {};
var licenses = ['MIT'];
init.addLicenseFiles(files, licenses);
// files === {'LICENSE-MIT': 'licenses/LICENSE-MIT'}

```

## init.availableLicenses

返回可用的 licenses 数组：

```javascript

var licenses = init.availableLicenses();
// licenses === [ 'Apache-2.0', 'GPL-2.0', 'MIT', 'MPL-2.0' ]

```

## init.copy

给定一个绝对或相对的 source path，一个可选的 相对的 destination path，通过可选的回调方法处理复制文件。

```javascript

init.copy(srcpath[, destpath], options)

```

## init.copyAndProcess

遍历传入对象中所有文件，复制源文件内容到目标，处理这些文件内容。

```javascript

init.copyAndProcess(files, props[, options])

```

## init.defaults

从 defaults.json 指定的用户默认值初始值。｀

```javascript

init.defaults

```

## init.destpath

目标文件的相对路径。

```javascript

init.destpath()

```

## init.expand

与 [grunt.file.expand](https://github.com/gruntjs/grunt/wiki/grunt.file#wiki-grunt-file-expand) 相同。

返回适配通配符表达式的所有文件数组或目录路径。这个方法接受 `,` 分隔的通配符表达式或通配符表达时的数组。适配路径的表达式如果以 `!` 开头将会被排除出数组。表达式按顺序处理，因此包含和排斥的顺序非常重要。

```javascript

init.expand([options, ] patterns)

```

## init.filesToCopy

返回一个对象，该对象包含复制文件的来源路径和相对目标路径，根据 rename.json (如果有) 的规则重命名 (或省略)。

```javascript

var files = init.filesToCopy(props);
/* files === { '.gitignore': 'template/root/.gitignore',
  '.jshintrc': 'template/root/.jshintrc',
  'Gruntfile.js': 'template/root/Gruntfile.js',
  'README.md': 'template/root/README.md',
  'test/test_test.js': 'template/root/test/name_test.js' } */

```

## init.getFile

获取单个任务文件路径

```javascript

init.getFile(filepath[, ...])

```

## init.getTemplates

返回一个包含所有可用 templates 的对象。

```javascript

init.getTemplates()

```

## init.initSearchDirs

为了搜索初始模版而初始化目录。`template` 是模版的位置。还包括 `~/.grunt-init/` 和核心初始任务在 `grunt-init` 内。

```javascript

init.initSearchDirs([filename])

```
 
## init.process

开启处理并提示用户输入

```javascript

init.process(options, prompts, done)

```

```javascript

init.process({}, [
  // Prompt for these values
  init.prompt('name'),
  init.prompt('description'),
  init.prompt('version')
], function(err, props) {
  // All finished, do something with the properties
});

```

## init.prompt

提示用户需要输入的值。

```javascript

init.prompt(name[, default])

```

## init.prompts

获取一个包含所有 prompt 的对象。

```javascript

var prompts = init.prompts;

```

## init.readDefaults

从默认任务文件 (如果有) 读取 JSON，合并它们到一个数据对象。

```javascript

init.readDefaults(filepath[, ...])

```

## init.renames

模版的重命名规则

```javascript

var renames = init.renames;
// renames === { 'test/name_test.js': 'test/{%= name %}_test.js' }

```

## init.searchDirs

搜索模版内的目录结果的数组。

```javascript

var dirs = init.searchDirs;
/* dirs === [ '/Users/shama/.grunt-init',
  '/usr/local/lib/node_modules/grunt-init/templates' ] */

```

## init.srcpath

在初始模版目录搜索文件名并返回绝对路径。

```javascript

init.srcpath(filepath[, ...])

```

## init.userDir

返回用户的模版目录的绝对路径。

```javascript

var dir = init.userDir();
// dir === '/Users/shama/.grunt-init'

```

## init.writePackageJSON

写入 package.json 文件到目标目录。回调方法可以用作后期 add/remove 一些属性。

```javascript

init.writePackageJSON(filename, props[, callback])

```

# Built-in prompts

## author_email

Author's email address to use in the package.json. Will attempt to find a default value from the user's git config.

## author_name

Author's full name to use in the package.json and copyright notices. Will attempt to find a default value from the user's git config.

## author_url

A public URL to the author's website to use in the package.json.

## bin

A relative path from the project root for a cli script.

## bugs

A public URL to the project's issues tracker. Will default to the github issue tracker if the project has a github repository.

## description

A description of the project. Used in the package.json and README files.

## grunt_version

A valid semantic version range descriptor of Grunt the project requires.

## homepage

A public URL to the project's home page. Will default to the github url if a github repository.

## jquery_version

If a jQuery project, the version of jQuery the project requires. Must be a valid semantic version range descriptor.

## licenses

The license(s) for the project. Multiple licenses are separated by spaces. The licenses built-in are: MIT, MPL-2.0, GPL-2.0, and Apache-2.0. Defaults to MIT. Add custom licenses with init.addLicenseFiles.

## main

The primary entry point of the project. Defaults to the project name within the lib folder.

## name

The name of the project. Will be used heavily throughout the project template. Defaults to the current working directory.

## node_version

The version of Node.js the project requires. Must be a valid semantic version range descriptor.

## npm_test

The command to run tests on your project. Defaults to grunt.

## repository

Project's git repository. Defaults to a guess of a github url.

## title

A human readable project name. Defaults to the actual project name altered to be more human readable.

## version

The version of the project. Defaults to the first valid semantic version, 0.1.0.

