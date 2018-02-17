# gameloop-compatible [![npm version](https://badge.fury.io/js/gameloop-compatible.svg)](https://badge.fury.io/js/gameloop-compatible)

A game loop FORK

* designed by [timetocode](https://github.com/timetocode) for NodeJS applications. Uses a combination of `setTimeout` and `setImmediate` to achieve accurate update ticks with minimal CPU usage.

* Uses Editions for non es6 code importers.

* This repo adds `npm` module support and an API that allows it to be called from client code.

```sh
npm install --save gameloop-compatible
```

## Example

`gameloop-compatible` uses an API very similar to `setTimeout`/`setInterval`, returning an ID that can be used to clear the game loop later.

```js
const gameloop = require('gameloop-compatible');

// start the loop at 30 fps (1000/30ms per frame) and grab its id
let frameCount = 0;
const id = gameloop.setGameLoop(function(delta) {
	// `delta` is the delta time from the last frame
	console.log('Hi there! (frame=%s, delta=%s)', frameCount++, delta);
}, 1000 / 30);

// stop the loop 2 seconds later
setTimeout(function() {
	console.log('2000ms passed, stopping the game loop');
	gameloop.clearGameLoop(id);
}, 2000);
```

## API

```js
var gameloop = require('gameloop-compatible');
```

Return | Function | Params | Description
--- | --- | --- | ---
number `id` | `setGameLoop` | (function `update(delta)`, <br>[float `targetDeltaMs`]) | Sets and runs a game loop at a target delta (in milliseconds) [defaults to 30fps]. Runs function `update` with a parameter delta (time in seconds from last update). Returns the game loop ID used in `clearGameLoop`
void | `clearGameLoop` | (number `id`) | Clears and stops a given game loop. Will cancel the loop immediately and will not wait for current frame to finish.
