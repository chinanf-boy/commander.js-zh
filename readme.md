# commander.js [![explain]][source] [![translate-svg]][translate-list]

[explain]: http://llever.com/explain.svg
[source]: https://github.com/chinanf-boy/Source-Explain
[translate-svg]: http://llever.com/translate.svg
[translate-list]: https://github.com/chinanf-boy/chinese-translate-list
    
「 node 命令行接口 简单操作 」

[中文](./readme.md) | [english](./en.md)


---

## 校对 ✅

<!-- doc-templite START generated -->
<!-- repo = 'tj/commander.js' -->
<!-- commit = 'e5b27cc553c0c55eb2f8890dc83034d3a3eee531' -->
<!-- time = '2018 8.7' -->
翻译的原文 | 与日期 | 最新更新 | 更多
---|---|---|---
[commit] | ⏰ 2018 8.7 | ![last] | [中文翻译][translate-list]

[last]: https://img.shields.io/github/last-commit/tj/commander.js.svg
[commit]: https://github.com/tj/commander.js/tree/e5b27cc553c0c55eb2f8890dc83034d3a3eee531

<!-- doc-templite END generated -->

### 贡献

欢迎 👏 勘误/校对/更新贡献 😊 [具体贡献请看](https://github.com/chinanf-boy/chinese-translate-list#贡献)

## 生活

[If help, **buy** me coffee —— 营养跟不上了，给我来瓶营养快线吧! 💰](https://github.com/chinanf-boy/live-need-money)

---

### 目录

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Commander.jS](#commanderjs)
  - [安装](#%E5%AE%89%E8%A3%85)
  - [参数解析](#%E5%8F%82%E6%95%B0%E8%A7%A3%E6%9E%90)
  - [版本选项](#%E7%89%88%E6%9C%AC%E9%80%89%E9%A1%B9)
  - [特定-命令选项](#%E7%89%B9%E5%AE%9A-%E5%91%BD%E4%BB%A4%E9%80%89%E9%A1%B9)
  - [强制多态](#%E5%BC%BA%E5%88%B6%E5%A4%9A%E6%80%81)
  - [正则表达式](#%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F)
  - [可变参数](#%E5%8F%AF%E5%8F%98%E5%8F%82%E6%95%B0)
  - [指定参数语法](#%E6%8C%87%E5%AE%9A%E5%8F%82%E6%95%B0%E8%AF%AD%E6%B3%95)
  - [Git风格的子命令](#git%E9%A3%8E%E6%A0%BC%E7%9A%84%E5%AD%90%E5%91%BD%E4%BB%A4)
    - [`--harmony`](#--harmony)
  - [自动化帮助信息 --help](#%E8%87%AA%E5%8A%A8%E5%8C%96%E5%B8%AE%E5%8A%A9%E4%BF%A1%E6%81%AF---help)
  - [自定义帮助](#%E8%87%AA%E5%AE%9A%E4%B9%89%E5%B8%AE%E5%8A%A9)
  - [.outputHelp(cb)](#outputhelpcb)
  - [.help(cb)](#helpcb)
  - [自定义事件侦听器](#%E8%87%AA%E5%AE%9A%E4%B9%89%E4%BA%8B%E4%BB%B6%E4%BE%A6%E5%90%AC%E5%99%A8)
  - [实例](#%E5%AE%9E%E4%BE%8B)
  - [许可证](#%E8%AE%B8%E5%8F%AF%E8%AF%81)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


# Commander.jS

[![Build Status](https://api.travis-ci.org/tj/commander.js.svg?branch=master)](http://travis-ci.org/tj/commander.js)
[![NPM Version](http://img.shields.io/npm/v/commander.svg?style=flat)](https://www.npmjs.org/package/commander)
[![NPM Downloads](https://img.shields.io/npm/dm/commander.svg?style=flat)](https://npmcharts.com/compare/commander?minimal=true)
[![Install Size](https://packagephobia.now.sh/badge?p=commander)](https://packagephobia.now.sh/result?p=commander)
[![Join the chat at https://gitter.im/tj/commander.js](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/tj/commander.js?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

  [node.js](http://nodejs.org) 命令行接口的完整解决方案，灵感来自 Ruby 的 [commander](https://github.com/commander-rb/commander)。  
  [API 文档](http://tj.github.com/commander.js/)



## 安装

    $ npm install commander --save

## 参数解析

commander 的 `.option()` 方法定义选项，同时也作为 这些选项的说明。下面的例子会解析来自 `process.argv` 的参数和选项，没有匹配任何选项的参数将会放到 `program.args` 数组中. 

```js
#!/usr/bin/env node

/**
 * Module dependencies.
 */

var program = require('commander');

program
  .version('0.1.0')
  .option('-p, --peppers', 'Add peppers')
  .option('-P, --pineapple', 'Add pineapple')
  .option('-b, --bbq-sauce', 'Add bbq sauce')
  .option('-c, --cheese [type]', 'Add the specified type of cheese [marble]', 'marble')
  .parse(process.argv);

console.log('you ordered a pizza with:');
if (program.peppers) console.log('  - peppers');
if (program.pineapple) console.log('  - pineapple');
if (program.bbqSauce) console.log('  - bbq');
console.log('  - %s cheese', program.cheese);
```

例如, 短标志可以作为单独的参数传递。像 `-abc` 等于 `-a -b -c`。多词组成的选项，像“--template-engine”会变成 `program.templateEngine` 等。

注意,多词选项以`--no`为前缀会 否定 接下单词的布尔值. 例如,`--no-sauce`设置`program.sauce`是`false`的. 

```js
#!/usr/bin/env node

/**
 * Module dependencies.
 */

var program = require('commander');

program
  .option('--no-sauce', 'Remove sauce')
  .parse(process.argv);

console.log('you ordered a pizza');
if (program.sauce) console.log('  with sauce');
else console.log(' without sauce');
```

## 版本选项

隐式添加`-V`和`--version`命令的选项来调用`version`. 当存在这些选项中的任何一个时,打印版本号并退出. 

    $ ./examples/pizza -V
    0.0.1

如果您希望程序响应`-v`选项而不是`-V`选项,只需将自定义`flag`传递给`version`方法.

```js
program
  .version('0.0.1', '-v, --version')
```

版本`flag`可以是其他命名,但长选项是必须的. 

## 特定-命令选项

可以将选项附加到命令中. 

```js
#!/usr/bin/env node

var program = require('commander');

program
  .command('rm <dir>')
  .option('-r, --recursive', 'Remove recursively')
  .action(function (dir, cmd) {
    console.log('remove ' + dir + (cmd.recursive ? ' recursively' : ''))
  })

program.parse(process.argv)
```

使用对应命令时,验证其命令的选项. 任何未知选项将被报告为一个错误. 但是,如果基于`action`的命令不定义`action`方法,那么选项将不被验证. 

## 强制多态

```js
function range(val) {
  return val.split('..').map(Number);
}

function list(val) {
  return val.split(',');
}

function collect(val, memo) {
  memo.push(val);
  return memo;
}

function increaseVerbosity(v, total) {
  return total + 1;
}

program
  .version('0.1.0')
  .usage('[options] <file ...>')
  .option('-i, --integer <n>', 'An integer argument', parseInt)
  .option('-f, --float <n>', 'A float argument', parseFloat)
  .option('-r, --range <a>..<b>', 'A range', range)
  .option('-l, --list <items>', 'A list', list)
  .option('-o, --optional [value]', 'An optional value')
  .option('-c, --collect [value]', 'A repeatable value', collect, [])
  .option('-v, --verbose', 'A value that can be increased', increaseVerbosity, 0)
  .parse(process.argv);

console.log(' int: %j', program.integer);
console.log(' float: %j', program.float);
console.log(' optional: %j', program.optional);
program.range = program.range || [];
console.log(' range: %j..%j', program.range[0], program.range[1]);
console.log(' list: %j', program.list);
console.log(' collect: %j', program.collect);
console.log(' verbosity: %j', program.verbose);
console.log(' args: %j', program.args);
```

## 正则表达式

```js
program
  .version('0.1.0')
  .option('-s --size <size>', 'Pizza size', /^(large|medium|small)$/i, 'medium')
  .option('-d --drink [drink]', 'Drink', /^(coke|pepsi|izze)$/i)
  .parse(process.argv);
  
console.log(' size: %j', program.size);
console.log(' drink: %j', program.drink);
```

## 可变参数

 一个命令的最后一个参数可以是可变参数, 并且只有最后一个参数可变。为了使参数可变，你需要在参数名后面追加 `...`。 下面是个示例：

```js
#!/usr/bin/env node

/**
 * Module dependencies.
 */

var program = require('commander');

program
  .version('0.1.0')
  .command('rmdir <dir> [otherDirs...]')
  .action(function (dir, otherDirs) {
    console.log('rmdir %s', dir);
    if (otherDirs) {
      otherDirs.forEach(function (oDir) {
        console.log('rmdir %s', oDir);
      });
    }
  });

program.parse(process.argv);
```

可变参数的值以 `数组` 的形式保存。如上所示，在传递给你的 action 的参数和 `program.args` 中的值都是如此。

## 指定参数语法

```js
#!/usr/bin/env node

var program = require('commander');

program
  .version('0.1.0')
  .arguments('<cmd> [env]')
  .action(function (cmd, env) {
     cmdValue = cmd;
     envValue = env;
  });

program.parse(process.argv);

if (typeof cmdValue === 'undefined') {
   console.error('no command given!');
   process.exit(1);
}
console.log('command:', cmdValue);
console.log('environment:', envValue || "no environment given");
```

尖括号（例如 `<cmd>`）代表必填输入，方括号（例如 `[env]`）代表可选输入。

## Git风格的子命令

```js
// file: ./examples/pm
var program = require('commander');

program
  .version('0.1.0')
  .command('install [name]', 'install one or more packages')
  .command('search [query]', 'search with optional query')
  .command('list', 'list packages installed', {isDefault: true})
  .parse(process.argv);
```

当 `.command()` 带有描述参数时，不能采用 `.action(callback)` 来处理子命令，否则会出错。这告诉 commander，你将采用单独的可执行文件作为子命令，就像 `git(1)` 和其他流行的工具一样。
Commander 将会尝试在入口脚本（例如 `./examples/pm`）的目录中搜索 `program-command` 形式的可执行文件，例如 `pm-install`, `pm-search`。

你可以在调用 `.command()` 时传递选项。指定 `opts.noHelp` 为 `true` 将从生成的帮助输出中剔除该选项。指定 `opts.isDefault` 为 `true` 将会在没有其它子命令指定的情况下，执行该子命令。

如果你打算全局安装该命令，请确保可执行文件有对应的权限，例如 `755`。

### `--harmony`

您可以采用两种方式启用 `--harmony`：

* 在子命令脚本中加上 `#!/usr/bin/env node --harmony`。注意一些系统版本不支持此模式。
* 在指令调用时加上 `--harmony` 参数，例如 `node --harmony examples/pm publish`。`--harmony` 选项在开启子进程时会被保留。

## 自动化帮助信息 --help

 帮助信息是 commander 基于你的程序自动生成的，下面是 `--help` 生成的帮助信息：

     $ ./examples/pizza --help

       Usage: pizza [options]

       An application for pizzas ordering

       Options:

         -h, --help           output usage information
         -V, --version        output the version number
         -p, --peppers        Add peppers
         -P, --pineapple      Add pineapple
         -b, --bbq            Add bbq sauce
         -c, --cheese <type>  Add the specified type of cheese [marble]
         -C, --no-cheese      You do not want any cheese

## 自定义帮助

 你可以通过监听 `--help` 来控制 `-h, --help` 显示任何信息。一旦调用完成， Commander 将自动退出，你的程序的其余部分不会展示。例如在下面的 “stuff” 将不会在执行 `--help` 时输出。

```js
#!/usr/bin/env node

/**
 * Module dependencies.
 */

var program = require('commander');

program
  .version('0.1.0')
  .option('-f, --foo', 'enable some foo')
  .option('-b, --bar', 'enable some bar')
  .option('-B, --baz', 'enable some baz');

// must be before .parse() since
// node's emit() is immediate

program.on('--help', function(){
  console.log('  Examples:');
  console.log('');
  console.log('    $ custom-help --help');
  console.log('    $ custom-help -h');
  console.log('');
});

program.parse(process.argv);

console.log('stuff');
```

下列帮助信息是运行 `node script-name.js -h` or `node script-name.js --help` 时输出的:

    Usage: custom-help [options]

    Options:

      -h, --help     output usage information
      -V, --version  output the version number
      -f, --foo      enable some foo
      -b, --bar      enable some bar
      -B, --baz      enable some baz

    Examples:

      $ custom-help --help
      $ custom-help -h

## .outputHelp(cb)

输出帮助信息而不退出. 
可选的回调可在显示帮助文本后处理。
如果你想显示默认的帮助（例如，如果没有提供命令），你可以使用类似的东西：

```js
var program = require('commander');
var colors = require('colors');

program
  .version('0.1.0')
  .command('getstream [url]', 'get stream URL')
  .parse(process.argv);

if (!process.argv.slice(2).length) {
  program.outputHelp(make_red);
}

function make_red(txt) {
  return colors.red(txt); //display the help text in red on the console
}
```

## .help(cb)

输出帮助信息并立即退出. 
可选的回调可在显示帮助文本后处理. 

## 自定义事件侦听器

可以通过监听命令和选项事件来执行自定义操作. 

```js
program.on('option:verbose', function () {
  process.env.VERBOSE = this.verbose;
});

// error on unknown commands
program.on('command:*', function () {
  console.error('Invalid command: %s\nSee --help for a list of available commands.', program.args.join(' '));
  process.exit(1);
});
```

## 实例

```js
var program = require('commander');

program
  .version('0.1.0')
  .option('-C, --chdir <path>', 'change the working directory')
  .option('-c, --config <path>', 'set config path. defaults to ./deploy.conf')
  .option('-T, --no-tests', 'ignore test hook');

program
  .command('setup [env]')
  .description('run setup commands for all envs')
  .option("-s, --setup_mode [mode]", "Which setup mode to use")
  .action(function(env, options){
    var mode = options.setup_mode || "normal";
    env = env || 'all';
    console.log('setup for %s env(s) with %s mode', env, mode);
  });

program
  .command('exec <cmd>')
  .alias('ex')
  .description('execute the given remote cmd')
  .option("-e, --exec_mode <mode>", "Which exec mode to use")
  .action(function(cmd, options){
    console.log('exec "%s" using %s mode', cmd, options.exec_mode);
  }).on('--help', function() {
    console.log('  Examples:');
    console.log();
    console.log('    $ deploy exec sequential');
    console.log('    $ deploy exec async');
    console.log();
  });

program
  .command('*')
  .action(function(env){
    console.log('deploying "%s"', env);
  });

program.parse(process.argv);
```

更多的演示,可以看看[实例](https://github.com/tj/commander.js/tree/master/examples)目录. 

## 许可证

MIT
