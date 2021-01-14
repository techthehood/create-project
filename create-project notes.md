
[How to build a CLI with node.js (tutorial)](https://www.youtube.com/watch?v=s2h28p4s-Xs&feature=youtu.be)   
> awesome tutorial - a small adjustment on **import.meta.url**

> allows us to us es modules without needing to transpile to different node js
versions that may not have that support

[esm on npm](https://www.npmjs.com/package/esm)   

> The brilliantly simple, babel-less, bundle-less ECMAScript module loader.

```
  npm i esm
```
# using comments in create-project script   

**GOTCHA:** this also is running node so it takes regular node comments

```
  npm i arg inquirer
```

```
  // using template: args instead of template: args._[0]
  $ create-project cli --git

  // returns
  {
    skipPrompts: false,
    git: true,
    template: { _: [ 'cli' ], '--git': true },
    runInstall: false
  }

```
> the return value looks like all other arguments that aren't the specifically named ones in
the arg() setup were added to an array object named "_" i.e. _:[...theRest]

[ncp - Asynchronous recursive file & directory copying](https://www.npmjs.com/package/ncp)      
> ncp to copy some template files over

```
  npm i ncp chalk
```

#### [using %s](https://stackoverflow.com/questions/6999572/what-does-s-mean-inside-a-string-literal#:~:text=%25s%20is%20a%20C%20format%20specifier%20for%20a%20string.&text=the%20%25s%20is%20a%20'format,contents%20of%20the%20command%20variable.)      

```
  console.log("%s Project ready", chalk.green.bold('DONE'));
  console.log('%s Project ready', chalk.green.bold('DONE'));
```
> both do the same thing. it somehow puts the txt at the end.

>%s is a C format specifier for a string. ... the %s is a 'format character', indicating "insert a string here". The extra parameters after the string in your two function calls are the values to fill into the format character placeholders: In the first example, %s will be replaced with the contents of the command variable.

#### **GOTCHA**: getting the path right

```
  const currentFileUrl  = import.meta.url;// idk what this is

  console.log('[currentFileUrl]',currentFileUrl);
  console.log('[newPathName]',new URL(currentFileUrl));
  console.log('[dirname]',__dirname);

  const templateDir = path.resolve(
    // new URL(currentFileUrl).pathname,
    __dirname,
    '../templates',
    options.template.toLowerCase()
  );

  const currentFileUrl  = import.meta.url;
  // returns - C:\C:\Users\...\templateName
```
> notice the C:\C:\ at the beginning. i had to use __dirname to fix it. not i need to research why the author used something else in the first place.

#### a working compromise

>this also works an may fix possible limitations with linux on the server where __dirname may not work - i need to test on the server. if __dirname works on the server then nothing else is needed.

```
  import { fileURLToPath } from 'url';

  console.log('[path dirname]',path.dirname(fileURLToPath(currentFileUrl)));
```

Note: if you run it again after the files are created it looks like it does it again but it does nothing.

#### finishing up initializing git and installing the pkgs

```
npm i execa pkg-install listr
```
