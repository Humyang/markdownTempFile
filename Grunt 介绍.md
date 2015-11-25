# Grunt 介绍

[Grunt](https://github.com/gruntjs/grunt) 是基于 Node.js 的自动化工具。可以帮助你完成项目模版创建，压缩代码，生成测试代码等繁杂的工作。

想象一下，如果经常需要创建一些前端项目，有的是微信页面，有的是 Web App 项目，有的是商城项目。不同的项目的模版文件都不同，通过配置 Grunt 后，只需要一条命令就可以快速搭建好项目环境，直接进行开发。

当项目完成后，需要对 CSS，JavaScript 代码压缩，或者改动代码后的重新压缩，Grunt 同样可以帮助你完成这些工作。

Grunt 还提供了大量开发者贡献的插件，你所需要的任务大部分都可以在这里找到。

## 使用心得

暂时用到的 Grunt 插件有 `grunt-contrib-clean`，用于清除文件夹内容， `grunt-contrib-concat` 用于合并文件内容，`grunt-contrib-jshint` 用于检测 JavaScript 错误。

这些插件都有明确的功能，在 Gruntfile 配置好每个插件需要的参数，并使用 `grunt.loadNpmTasks` 加载插件功能，即可在项目更目录执行 Grunt 或 Grunt Task，使插件自行各自的任务。

有的插件需要指定文件来源 (src)，输出目标是可选的(dest)，如 clean 和 jshint。
有的则需要两者都明确指定，如 concat。

# 使用 Grunt

首先需要在项目目录添加 `package.json` 和 `Gruntfile.js`。

`package.json` 用于存储项目相关的数据和依赖。例如项目名称，版本号等。因为需要将项目放置在 Git 的仓库中，需要将 Grunt 的依赖要保存到该文件中，如 Grunt，Grunt 中的插件等，以便将来直接 `npm install` 使用。

`Gruntfile.js` 是用来保存项目对 Grunt 的配置。配置该文件后，在命令行进入项目目录执行 `grunt` 命令即可自动执行 `Gruntfile.js` 分配的任务。

## package.json

下面有几种创建 `package.json` 文件的方式：

* 使用 `grunt-init`，会自动在项目目录创建 `package.json` 文件。
* 使用 `npm init` 命令，创建一个基本的 `package.json` 文件。
* 复制下面的文本，根据需要进行补充。

```javascript
{
  "name": "my-project-name",
  "version": "0.1.0",
  "devDependencies": {
    "grunt": "~0.4.5",
    "grunt-contrib-jshint": "~0.10.0",
    "grunt-contrib-nodeunit": "~0.4.1",
    "grunt-contrib-uglify": "~0.5.0"
  }
}
```

使用 `npm install <module> --save-dev` 安装 Grunt 和 Grunt 插件是最好的方式，它不仅会在项目目录安装模块，并且会将依赖信息写入到 `package.json` 的 `devDependencies` 字段中。 

例如下面的命令安装 Grunt 的最新版本到项目目录中，并将信息添加到 `devDependencies`。

```javascript
npm install grunt --save-dev
```

同样，安装 Grunt 插件也是这种形式：

```javascript
npm install grunt-contrib-jshint --save-dev
```

[Grunt 插件](http://www.gruntjs.net/plugins) 提供了大量 Grunt 插件，都可以直接安装使用。

安装插件之后，请务必确保将更新之后的 `package.json` 文件提交到项目仓库中。


## Gruntfile

`Gruntfile.js` 或 `Gruntfile.coffee` 文件都是有效的，应当放置在项目根目录，与 `package.json` 同级，并且添加到源码管理器中。

下面是一个 Gruntfile 例子：

```javascript
module.exports = function(grunt) {

  // Project configuration.
  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
    uglify: {
      options: {
        banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
      },
      build: {
        src: 'src/<%= pkg.name %>.js',
        dest: 'build/<%= pkg.name %>.min.js'
      }
    }
  });

  // 加载包含 "uglify" 任务的插件。
  grunt.loadNpmTasks('grunt-contrib-uglify');

  // 默认被执行的任务列表。
  grunt.registerTask('default', ['uglify']);

};
```

Gruntfile 由以下几部分构成：

* "wrapper" 函数
* 项目与任务配置
* 加载grunt插件和任务
* 自定义任务

### "wrapper" 函数

`Gruntfile` 和 Grunt 插件都遵循同样的格式，你的 Grunt 代码必须放在此函数内：

```javascript
module.exports = function(grunt) {
  // Do grunt-related things in here
};
```

### 项目配置和任务配置

大部分 Grunt 都需要进行一些配置，配置信息定义在一个 Object 内，传递给 `grunt.initConfig` 方法。

下面的案例中，`grunt.file.readJSON('package.json')` 将 package.json 文件的配置信息读取到 Grunt ，保存在 pkg 变量中。通过使用 `<% %>` 模版字符串可以引用 Grunt 中任意的配置属性。例如 `pkg.name`,`grunt.template`。通过这种方式可以方便的使用配置数据，减少重复的工作。

你可以在这个配置对象中 (传递给 initConfig 的 Object) 存储任意的数据，只要它不与你任务配置所需的属性冲突。

与大多数任务一样，`grunt-contrib-uglify` 插件中的 `uglify` 任务配置指定在一个同名属性中。在这个例子中，我们使用了一个可选选项 `banner`，用于在文件顶部生成一个注释。接着是一个名为 `build` 的目标，用于配置要压缩的 js 文件和输出文件。

### 加载 Grunt 插件和任务

通过 `npm install grunt-contrib-jshint --save-dev` 安装插件或者直接执行 `npm install` 安装 `package.json` 的依赖文件后，需要在 Gruntfile 中加在这些插件：

```javascript
// 加载能够提供"uglify"任务的插件。
grunt.loadNpmTasks('grunt-contrib-uglify');
```

> 注意：`grunt --help` 命令将列出所有可用的任务。


### 自定义任务

通过定义 `default` 任务，可以让 Grunt 默认执行一个或多个任务。在下面的例子中，在命令行执行 grunt 如果不指定任务，则会执行 `uglify` 任务。这和执行 `grunt uglify` 或 `grunt default` 的效果是一样的。

default 任务列表数组中可以指定任意数量的任务 (可以带参数)。

```css
// Default task(s).
grunt.registerTask('default', ['uglify']);
```

如果 Grunt 提供的插件无法满足你的需求，你还可以在 `Gruntfile` 中自定义任务。

例如，下面的 `Gruntfile` 自定义了一个 default 任务，它甚至不依赖任务配置：

```javascript
module.exports = function(grunt) {

  // A very basic default task.
  grunt.registerTask('default', 'Log some stuff.', function() {
    grunt.log.write('Logging some stuff...').ok();
  });

};
```

针对特定项目的任务不必在 `Gruntfile` 中定义。它们可以定义在外部 `.js` 文件中，通过 `grunt.loadTasks` 方法加载。


# 配置任务

这章说明如何使用 Gruntfile 为你的项目配置任务。在  [Sample Gruntfile](http://gruntjs.com/sample-gruntfile/) 可以了解什么是 `Gruntfile`。

任务配置是通过 `grunt.initConfig` 方法指定给 `Gruntfile`。大部分配置都在任务名属性下，但也可以包含任意的其它数据。只要属性名不与你的任务需要的属性名冲突，否则将会被忽略。

 
```javascript
grunt.initConfig({
  concat: {
    // concat task configuration goes here.
  },
  uglify: {
    // uglify task configuration goes here.
  },
  // Arbitrary non-task-specific properties.
  my_property: 'whatever',
  my_src_files: ['foo/*.js', 'bar/*.js'],
});
```

## 任务配置和 Target

当运行任务时，Grunt 会寻找同名的属性下的配置。多个任务可以有多个配置，使用 Target 定义这些配置。在下面的例子，`concat` 任务有 `foo` 和 `bar` 两个 target，在 `uglify` 任务只有 `bar` 一个 target。

```javascript

grunt.initConfig({
  concat: {
    foo: {
      // concat task "foo" target options and files go here.
    },
    bar: {
      // concat task "bar" target options and files go here.
    },
  },
  uglify: {
    bar: {
      // uglify task "bar" target options and files go here.
    },
  },
});

```

在命令行中使用任务和 target，如 `grunt concat:foo` or `grunt concat:bar` 将会处理指定的 target 的配置。但运行 `grunt concat` 将会处理所有 target，一次处理。注意如果任务已经通过 `grunt.task.renameTask` 重命名，Grunt 将会在配置 Object 的属性中寻找新的任务名。

## Option

在任务的配置内，`option` 属性可以指定覆盖任务默认的配置。此外，每个 target 也有一个 `option` 属性真对 traget 的配置。Target 级别的 `option` 将会覆盖任务级别的 option。

options 对象是可选的，如果不需要可以省略。

```javascript

grunt.initConfig({
  concat: {
    options: {
      // Task-level options may go here, overriding task defaults.
    },
    foo: {
      options: {
        // "foo" target options may go here, overriding task-level options.
      },
    },
    bar: {
      // No options specified; this target will use task-level options.
    },
  },
});

```

## Files

因为大部分任务都需要执行文件操作，Grunt 定义了强大的抽象处理使任务选择需要操作的文件。这里有几种方式定义 `src-dest` (source-destination) 文件映射，提供了不同的详细程度和控制程度。任何多任务都明白下面的格式，因此选择你最需要的。

所有文件格式都支持 `src` 和 `desc` ，但 “Compact” 和 “Files Array” 格式支持少量额外的属性：

* `filter` 传入 [fs.Stats method name ]() 或 function 适配 `src` 的文件路径并返回 `true` 或 `false`。
* `nonull` 如果设置成 `true` 那么该操作将包含非匹配模式。结合 Grunt 的 `--verbose` 标记，这个选项可以帮助调试文件路径问题。
* `dot` 允许表达式适配文件名开始的部分，即使表达式没有明确的指定 `.` 的范围。
* `matchBase` 如果设置，那么路径的 basename 包含斜杠的部分不会被匹配，只会匹配不包含斜杠的部分。例如，a?b 将会适配路径 `/xyz/123/acb`，但不适配 `/xyz/acb/123`。
* `expand` 处理动态的 src-dest 文件映射，见 ["Building the files object dynamically"](http://gruntjs.com/configuring-tasks#building-the-files-object-dynamically) 获取更多信息。


### Compact Format

这个 format 允许在每个 target 中添加单个 **src-dest** 文件映射。在只读任务中它是最通用的，例如 [grunt-contrib-jshint](https://github.com/gruntjs/grunt-contrib-jshint)，只需要单个 `src`，不需要相应的 `dest`。这个格式也支持给每一个 `src-dest` 文件映射添加额外的属性 (filter,nonull 等)。

```javascript

grunt.initConfig({
  jshint: {
    foo: {
      src: ['src/aa.js', 'src/aaa.js']
    },
  },
  concat: {
    bar: {
      src: ['src/bb.js', 'src/bbb.js'],
      dest: 'dest/b.js',
    },
  },
});

```

### Files Object Format

这种格式对每个 target 支持多个 src-dest 映射，每个属性名对应目标文件，值是源文件 (多个)。任意数量的 src-dest 文件映射可以指定为这种方式，但每一个映射不能指定额外的属性 (filter,nonull 等)。

```javascript

grunt.initConfig({
  concat: {
    foo: {
      files: {
        'dest/a.js': ['src/aa.js', 'src/aaa.js'],
        'dest/a1.js': ['src/aa1.js', 'src/aaa1.js'],
      },
    },
    bar: {
      files: {
        'dest/b.js': ['src/bb.js', 'src/bbb.js'],
        'dest/b1.js': ['src/bb1.js', 'src/bbb1.js'],
      },
    },
  },
});

```

### Files Array Format

这种格式支持每个 target 多个 src-dest 文件映射，也允许对每个映射添加额外的属性 (filter,nonull 等)。

```javascript
grunt.initConfig({
  concat: {
    foo: {
      files: [
        {src: ['src/aa.js', 'src/aaa.js'], dest: 'dest/a.js'},
        {src: ['src/aa1.js', 'src/aaa1.js'], dest: 'dest/a1.js'},
      ],
    },
    bar: {
      files: [
        {src: ['src/bb.js', 'src/bbb.js'], dest: 'dest/b/', nonull: true},
        {src: ['src/bb1.js', 'src/bbb1.js'], dest: 'dest/b1/', filter: 'isFile'},
      ],
    },
  },
});
```

### Older Formats

**dest-as-target** 文件格式是遗留的多任务 targets 格式，它把目标目标文件路径作为 target 名。不幸的是，因为 target 名是文件路径，运行 `grunt task:target` 时显得非常笨拙。同时，你不能指定 target 级别的选项或者给每个 src-dest 文件映射添加额外的属性。

因为这种格式已经移除，应当避免下面的方式：

```javascript

grunt.initConfig({
  concat: {
    'dest/a.js': ['src/aa.js', 'src/aaa.js'],
    'dest/b.js': ['src/bb.js', 'src/bbb.js'],
  },
});

```

### Custom Filter Function

`filter` 属性让你精确控制你需要的内容。简单的使用有效的 [fs.Stats method name](http://javascript.org/docs/latest/api/fs.html#fs_class_fs_stats)。下面将适配内容指定为文件：

```javascript

grunt.initConfig({
  clean: {
    foo: {
      src: ['tmp/**/*'],
      filter: 'isFile',
    },
  },
});

```

或者你可以创建属于你的 `filter` 函数，根据文件是否匹配返回 true 或 false 。下面演示选取空文件夹的操作：

```javascript

grunt.initConfig({
  clean: {
    foo: {
      src: ['tmp/**/*'],
      filter: function(filepath) {
        return (grunt.file.isDir(filepath) && require('fs').readdirSync(filepath).length === 0);
      },
    },
  },
});

```

这样将获取了 tmp 所有子目录及子目录的子目录的空文件夹。

### Globbing patterns

使用单个表达式指定所有源文件的路径是不实用的，因此 Grunt 通过内置的 [node-glob](https://github.com/isaacs/node-glob) 和 [minimatch](https://github.com/isaacs/minimatch) 库提供了文件名expansion (也称为通配符)。

下面是不全面的通配符模式指南，用来匹配文件路径：

* `*` 适配任意数量的字符，不包含 `／`
* `?` 匹配单个字符串，不包含 `/`
* `**` 匹配任意数量的字符串，包含 `/`，只要它是唯一的路径
* `{}` 允许使用 `or` 表达式队列，在 `{}` 内部使用 `,` 分隔。
* `!` 出现在表达式开头会适配与该表达时不匹配的内容

大部分人需要知道 `foo/*.js` 将会适配所有以 `.js` 结尾的位于 `foo/` 子目录的所有文件。但 `foo/**/*.js` 将会适配所有 `.js` 结尾的文件位于 `foo/` 子目录和它的所有子目录的文件。

同时，为了简化复杂的通配符表达时，Grunt 允许文件路径或通配符表达式指定为数组。表达式会按顺序处理，前缀是 `!` 的表达式会从结果集排除适配结果。结果集是独特的。
 
例如下面：

```javascript

// You can specify single files:
{src: 'foo/this.js', dest: ...}
// Or arrays of files:
{src: ['foo/this.js', 'foo/that.js', 'foo/the-other.js'], dest: ...}
// Or you can generalize with a glob pattern:
{src: 'foo/th*.js', dest: ...}

// This single node-glob pattern:
{src: 'foo/{a,b}*.js', dest: ...}
// Could also be written like this:
{src: ['foo/a*.js', 'foo/b*.js'], dest: ...}

// All .js files, in foo/, in alpha order:
{src: ['foo/*.js'], dest: ...}
// Here, bar.js is first, followed by the remaining files, in alpha order:
{src: ['foo/bar.js', 'foo/*.js'], dest: ...}

// All files except for bar.js, in alpha order:
{src: ['foo/*.js', '!foo/bar.js'], dest: ...}
// All files in alpha order, but with bar.js at the end.
{src: ['foo/*.js', '!foo/bar.js', 'foo/bar.js'], dest: ...}

// Templates may be used in filepaths or glob patterns:
{src: ['src/<%= basename %>.js'], dest: 'build/<%= basename %>.min.js'}
// But they may also reference file lists defined elsewhere in the config:
{src: ['foo/*.js', '<%= jshint.all.src %>'], dest: ...

```

更多通配符表达时语法，见 [node-glob](https://github.com/isaacs/node-glob) 和 [minimatch](https://github.com/isaacs/minimatch) 文档。

### Building the files object dynamically

当你想处理许多单个文件时，一些属性可以帮助你构建动态的文件列表。这些属性可以指定到 “Compact” 和 “Files Array” 映射格式。

`expand` 设置为 true 启用下面的选项：

* `cwd` 传入一个路径，指定 `src` 的适配相对于 (但不包含) 此路径。
* `src` 适配表达式内容，相对于 `cwd` 路径。
* `dest` 目标路径前缀 (`lib/a.js` 则 /lib/ 是路径前缀)。
* `ext` 传入一个拓展名，将所有生成在 `dest` 路径的文件的拓展名替换为它。
* `extDot` 指定拓展名的位置。可以是 `first` (扩展名位于文件名称的第一个 `.` 之后) 或者 `last` (扩展名位于文件名称最后一个 `.` 之后)，默认值是 `first`。[在 0.43 版本添加] 
* `flatten` 从已生成的 `dest` 路径移除所有路径部分。
* `rename` 这个函数调用每一个适配的 `src` 文件，(拓展名重命名与合并之后)。`dest` 和适配的 `src` 路径会被传递进去，这个函数必须返回新的 `dest` 值。如果同样的 `dest` 返回了多次，每一个 `src` 都会使用它并为它添加到来源数组。 

下面的例子，`uglify` 任务将会在 `static_mappings` target 和 `dynamic_mappings` target 看到同样的 src-dest 文件映射，因为 Grunt 会将 `dynamic_mappings` 文件对象自动解释为四个单独的静态 `src-dest` 文件映射 - 假设在任务运行时找到了这四个文件。

可以指定任意的 静态 `src-dest` 和动态的 `src-dest` 文件映射。

```javascript

grunt.initConfig({
  uglify: {
    static_mappings: {
      // Because these src-dest file mappings are manually specified, every
      // time a new file is added or removed, the Gruntfile has to be updated.
      files: [
        {src: 'lib/a.js', dest: 'build/a.min.js'},
        {src: 'lib/b.js', dest: 'build/b.min.js'},
        {src: 'lib/subdir/c.js', dest: 'build/subdir/c.min.js'},
        {src: 'lib/subdir/d.js', dest: 'build/subdir/d.min.js'},
      ],
    },
    dynamic_mappings: {
      // Grunt will search for "**/*.js" under "lib/" when the "uglify" task
      // runs and build the appropriate src-dest file mappings then, so you
      // don't need to update the Gruntfile when files are added or removed.
      files: [
        {
          expand: true,     // Enable dynamic expansion.
          cwd: 'lib/',      // Src matches are relative to this path.
          src: ['**/*.js'], // Actual pattern(s) to match.
          dest: 'build/',   // Destination path prefix.
          ext: '.min.js',   // Dest filepaths will have this extension.
          extDot: 'first'   // Extensions in filenames begin after the first dot
        },
      ],
    },
  },
});

```

## Templates

Templates 指定使用 `<% %>` 分隔符，当从配置中读取任务时将会自动解析这些内容。Templates 会递归解析，直到没有 `<% %>` 为止。

完整的配置对象是这些属性解析完成后的上下文。此外，`grunt` 和它的方法可以在 templates 中使用，如 `<%= grunt.template.today('yyyy-mm-dd') %>`。

* `<%= prop.subprop %>` 解析为配置中的 `prop.subprop` 的值，不论类型。Templates 不仅可以用来引用字符串类型，也可以是数组或 Object。
* `<% %>` 可以执行任意行的 JavaScript 代码。可以用来执行控制流或循环。

下面给出一个 `concat` 任务的例子，运行 `concat:sample` 将会串联所有匹配 `foo/*.js`，`bar/*.js`，`baz/*.js` 的文件，生成名为 `build/abcde.js` 的文件。

```javascript

grunt.initConfig({
  concat: {
    sample: {
      options: {
        banner: '/* <%= baz %> */\n',   // '/* abcde */\n'
      },
      src: ['<%= qux %>', 'baz/*.js'],  // [['foo/*.js', 'bar/*.js'], 'baz/*.js']
      dest: 'build/<%= baz %>.js',      // 'build/abcde.js'
    },
  },
  // Arbitrary properties used in task configuration templates.
  foo: 'c',
  bar: 'b<%= foo %>d', // 'bcd'
  baz: 'a<%= bar %>e', // 'abcde'
  qux: ['foo/*.js', 'bar/*.js'],
});

```

## Importing External Data 

在下面的 Grunfile，Grunt 配置从 `package.json` 文件导入项目的元数据，`grunt-contrib-uglify plugin` 插件的 uglify 任务被配置为缩小源文件的尺寸并动态使用元数据生成注释 banner。

Grunt 有 `grunt.file.readJSON` 和 `grunt.file.readYAML` 方法分别读取 JSON 和 YAML 数据。

```javascript

grunt.initConfig({
  pkg: grunt.file.readJSON('package.json'),
  uglify: {
    options: {
      banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
    },
    dist: {
      src: 'src/<%= pkg.name %>.js',
      dest: 'dist/<%= pkg.name %>.min.js'
    }
  }
});

```

# Gruntfile 样本分析

下面我们仔细过一遍 `Gruntfile`，这里有一个小例子：

```javascript
module.exports = function(grunt) {

  grunt.initConfig({
    jshint: {
      files: ['Gruntfile.js', 'src/**/*.js', 'test/**/*.js'],
      options: {
        globals: {
          jQuery: true
        }
      }
    },
    watch: {
      files: ['<%= jshint.files %>'],
      tasks: ['jshint']
    }
  });

  grunt.loadNpmTasks('grunt-contrib-jshint');
  grunt.loadNpmTasks('grunt-contrib-watch');

  grunt.registerTask('default', ['jshint']);

};
```









































