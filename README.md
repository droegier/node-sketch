# 💎 node-sketch
Javascript library to manipulate sketch files

## Install

```sh
npm install node-sketch
```

## Example:

```js
const ns = require('node-sketch');

ns.read('design.sketch').then(sketch => {

    //Search the symbol named 'button'
    const buttonSymbol = sketch.symbolsPage.get('symbolMaster', 'button');

    //Search all instances of a symbol named 'old-button' and replace it with 'button'
    sketch
        .getAll('symbolInstance', instance => instance.symbolMaster.name === 'old-button')
        .forEach(instance => instance.symbolMaster = buttonSymbol);

    //Save the result
    sketch.save('modified-design.sketch');
});
```

## API

Two classes are used to manage sketch files:

### `Sketch`

Represents the sketch file and contains all data (pages, symbols, styles, shapes, etc). Contains the method `.save()` to create a sketch file with the result.

```js
const ns = require('node-sketch');

ns.read('input.sketch').then(sketch => {
    console.log(sketch.pages) //return all pages
    console.log(sketch.sharedStyles) //return the shared styles
    console.log(sketch.textStyles) //return the text styles
    //etc...

    sketch.save('output.sketch');
});

```

### `Node`

It's the base class used by all other elements. Any page, symbol, color, etc is an instance of this class containing all its properties.

```js
const symbolsPage = sketch.symbolsPage;

console.log(symbolsPage instanceof Node); //true 

//It include useful methods to search an inner node by class:
const button = symbolsPage.get('symbolMaster');

//by class and name
const button = symbolsPage.get('symbolMaster', 'button');

//by class and callback
const button = symbolsPage.get('symbolMaster', (symbol) => symbol.name === 'button');

//Just a callback
const button = symbolsPage.get((node) => node._class === 'symbolMaster' && node.name === 'button');

//or all inner nodes:
const allSymbols = symbolsPage.getAll('symbolMaster');
```

There are other classes extending `Node` to provide special funcionalities in some nodes:

* `Page`
* `SharedStyle`
* `Style`
* `SymbolInstance`
