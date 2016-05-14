# pull-pull [![stability][0]][1]
[![NPM version][2]][3] [![Downloads][4]][5] [![js-standard-style][6]][7]

> pipe many pull streams into a pipeline

## Background

In pull-streams, you need a complete pipeline before data will flow.

That means: a source, zero or more throughs, and a sink.

But you can still create a _partial_ pipeline, which is a great for tiny pull-stream modules.

## Usage

```js
var pull = require('pull-pull')
```

Create a simple complete pipeline:

```js
pull(source, sink) => undefined
```

Create a source modified by a through:

```js
pull(source, through) => source
```

Create a sink, but modify it's input before it goes.

```js
pull(through, sink) => sink
```

Create a through, by chainging several throughs:

```js
pull(through1, through2) => through
```

These streams combine just like normal streams.

```js
pull(
  pull(source, through),
  pull(through1, through2),
  pull(through, sink)
) => undefined
```

The complete pipeline returns undefined, because it cannot be piped to anything else.

Pipe duplex streams like this:

```js
var a = duplex()
var b = duplex()

pull(a.source, b.sink)
pull(b.source, a.sink)

//which is the same as

b.sink(a.source); a.sink(b.source)

//but the easiest way is to allow pull to handle this

pull(a, b, a)

//"pull from a to b and then back to a"
```

## API

```js
var pull = require('pull-pull')
```

### `pull(...streams)`

`pull` is a function that receives n-arity stream arguments and connects them into a pipeline.

`pull` detects the type of stream by checking function arity, if the function takes only one argument it's either a sink or a through. Otherwise it's a source. A duplex stream is an object with the shape `{ source, sink }`.

If the pipeline is complete (reduces into a source being passed into a sink), then `pull` returns `undefined`, as the data is flowing.

If the pipeline is partial (reduces into either a source, a through, or a sink), then `pull` returns the partial pipeline, as it must be composed with other streams before the data will flow.

## Install

With [npm](https://npmjs.org/) installed, run

```sh
$ npm install pull-pull
```

## See Also

- [`mafintosh/pump`](https://github.com/mafintosh/pump)
- [`mafintosh/pumpify`](https://github.com/mafintosh/pumpify)

## License

[MIT](https://tldrlegal.com/license/mit-license)

[0]: https://img.shields.io/badge/stability-stable-brightgreen.svg?style=flat-square
[1]: https://nodejs.org/api/documentation.html#documentation_stability_index
[2]: https://img.shields.io/npm/v/pull-pull.svg?style=flat-square
[3]: https://npmjs.org/package/pull-pull
[4]: http://img.shields.io/npm/dm/pull-pull.svg?style=flat-square
[5]: https://npmjs.org/package/pull-pull
[6]: https://img.shields.io/badge/code%20style-standard-brightgreen.svg?style=flat-square
[7]: https://github.com/feross/standard
