[![oclif](https://img.shields.io/badge/cli-oclif-brightgreen.svg)](https://oclif.io)

# Using OCLIF to create CLI


## Install OCLIF

```
npm i -g oclif
```



## Create a project


```
oclif generate [PROJECT NAME]
```


###  Without Installing OCLIF globally


```
npx oclif generate [PROJECT NAME]
```

## Enter into project directory

```
cd [PROJECT NAME]
```

## Build Project

```
npm run build
```

## Generate Command

 ```
 oclif generate command [Command Name]
 ```
 
 ## Publish Library
 ```
 npm publish
 ```
 
## Command Arguments

[docs](https://oclif.io/docs/args)

## Autoreload when running

* Install [build-if-changed](https://www.npmjs.com/package/build-if-changed) npm library;
```
yarn add build-if-changed -D
```

* Add this to root of `package.json`
```
"bic": ["*.ts"],
```

* Run your command like so
```
yarn bic; ./bin/run [COMMAND NAME]
```
> `yarn bic` will only build the project if `src` folder is changed.  `./bin/run [COMMAND NAME]` will run the command.

## Links

* [Interface components that work well with OCLIF](https://medium.com/adobetech/how-to-easily-create-clis-using-oclif-and-how-we-did-it-for-the-adobe-i-o-cli-e9eb888e9ec6)
* [CLI-UX](https://github.com/oclif/core/blob/main/src/cli-ux/README.md) in latest oclif versions
* [Listr](https://www.npmjs.com/package/listr)
* [Enquirer](https://github.com/enquirer/enquirer#select-prompt) ❤️
### Could not get this working in the project
* [OCLIF Inquirer](https://oclif.io/docs/prompting#inquirer)
* [Inquirer](https://www.npmjs.com/package/inquirer#installation)
