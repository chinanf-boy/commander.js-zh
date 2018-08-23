# commander.js [![explain]][source] [![translate-svg]][translate-list]

[explain]: http://llever.com/explain.svg
[source]: https://github.com/chinanf-boy/Source-Explain
[translate-svg]: http://llever.com/translate.svg
[translate-list]: https://github.com/chinanf-boy/chinese-translate-list
    
「 node 命令行接口 简单操作 」

[中文](./readme.md) | [english](./en.md)


---

## 校对 🀄️

<!-- doc-templite START generated -->
<!-- repo = 'tj/commander.js' -->
<!-- commit = 'e5b27cc553c0c55eb2f8890dc83034d3a3eee531' -->
<!-- time = '2018 8.7' -->
翻译的原文 | 与日期 | 最新更新 | 更多
---|---|---|---
[commit] | ⏰ 2018 8.17 | ![last] | [中文翻译][translate-list]

[last]: https://img.shields.io/github/last-commit/chinanf-boy/debug.svg
[commit]: https://github.com/chinanf-boy/debug/tree/1.1.1

<!-- doc-templite END generated -->

### 贡献

欢迎 👏 勘误/校对/更新贡献 😊 [具体贡献请看](https://github.com/chinanf-boy/chinese-translate-list#贡献)

## 生活

[help me live , live need money 💰](https://github.com/chinanf-boy/live-need-money)

---

### 目录

<!-- START doctoc -->
<!-- END doctoc -->


# 指挥官JS

[![Build Status](https://api.travis-ci.org/tj/commander.js.svg?branch=master)](http://travis-ci.org/tj/commander.js)
[![NPM Version](http://img.shields.io/npm/v/commander.svg?style=flat)](https://www.npmjs.org/package/commander)
[![NPM Downloads](https://img.shields.io/npm/dm/commander.svg?style=flat)](https://npmcharts.com/compare/commander?minimal=true)
[![Install Size](https://packagephobia.now.sh/badge?p=commander)](https://packagephobia.now.sh/result?p=commander)
[![Join the chat at https://gitter.im/tj/commander.js](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/tj/commander.js?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

完整的解决方案[J.N.](http://nodejs.org)露比的启发下的命令行接口[指挥官](https://github.com/commander-rb/commander).\
  [API文档](http://tj.github.com/commander.js/)

## 安装

    $ npm install commander --save

## 选项解析

带有命令的选项被定义为`.option()`方法,也用作选项的文档. 下面的例子解析ARGS和选项`process.argv`留下剩余的ARG作为`program.args`未被选项消耗的数组. 

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

例如,可以将短标志作为单个ARG传递. `-abc`相当于`-a -b -c`. 多字选项,如"模板引擎"是骆驼装箱,成为`program.templateEngine`等. 

注意,多词选项从`--no`前缀否定下列单词的布尔值. 例如,`--no-sauce`设置值`program.sauce`错了. 

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

调用`version`隐式添加`-V`和`--version`命令的选项. 当存在这些选项中的任何一个时,命令打印版本号并退出. 

    $ ./examples/pizza -V
    0.0.1

如果您希望程序响应`-v`选项而不是`-V`选项,只需将自定义标志传递给`version`方法使用与`option`方法. 

```js
program
  .version('0.0.1', '-v, --version')
```

版本标志可以命名为任何东西,但需要长选项. 

## 命令特定选项

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

使用命令时验证命令的选项. 任何未知选项将被报告为一个错误. 但是,如果基于动作的命令不定义动作,那么选项不被验证. 

## 强制

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

## 变元论证

命令的最后一个参数可以是可变的,只有最后一个参数. 为了使论点多样化,你必须追加. `...`参数的名称. 下面是一个例子: 

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

安`Array`用于变量变量的值. 这适用于`program.args`以及上面提到的传递给您的动作的参数. 

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

斜角括号 (例如) `<cmd>`指示所需的输入. 方括号 (例如) `[env]`指示可选输入. 

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

什么时候?`.command()`用描述参数调用,不`.action(callback)`应该调用子命令来处理,否则会出错. 这告诉指挥官,你将使用单独的可执行文件来执行子命令,非常类似. `git(1)`以及其他受欢迎的工具. \
指挥官将尝试搜索入口脚本目录中的可执行文件 (如`./examples/pm`) `program-command`,像`pm-install`,`pm-search`.

选项可以通过调用`.command()`. 指定`true`对于`opts.noHelp`将从生成的帮助输出中移除子命令. 指定`true`对于`opts.isDefault`如果没有指定其他子命令,则将运行子命令. 

如果程序被设计为全局安装,请确保可执行文件具有适当的模式,例如`755`.

### `--harmony`

您可以启用`--harmony`期权有两种方式: 

-   使用`#! /usr/bin/env node --harmony`在子命令脚本中. 注意有些OS版本不支持这种模式. 
-   使用`--harmony`调用命令时的选项`node --harmony examples/pm publish`. 这个`--harmony`当生成子命令过程时,将保留选项. 

## 自动化ℴℴ帮助

帮助信息是基于信息命令器已经了解您的程序而自动生成的,因此如下`--help`信息是免费的: 

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

可以任意显示`-h, --help`通过听"帮助"信息. 一旦你完成了,指挥官会自动退出,这样你的程序的其余部分就不会执行而导致不期望的行为,例如,在下面的可执行程序中,当`--help`使用. 

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

何时产生以下帮助输出`node script-name.js -h`或`node script-name.js --help`运行: 

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

## OutoPelp (CB) 

输出帮助信息而不退出. 可选回调CB允许在显示帮助文本之前进行后处理. 

如果默认情况下希望显示帮助 (例如,如果没有提供命令) ,则可以使用如下内容: 

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

## 帮助 (CB) 

输出帮助信息并立即退出. 可选回调CB允许在显示帮助文本之前进行后处理. 

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

更多的演示可以在[实例](https://github.com/tj/commander.js/tree/master/examples)目录. 

## 许可证

MIT
