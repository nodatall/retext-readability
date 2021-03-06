# retext-readability [![Build Status][travis-badge]][travis] [![Coverage Status][codecov-badge]][codecov]

Check readability with [**retext**][retext].  Applies
[Dale—Chall][dale-chall], [Automated Readability][automated-readability],
[Coleman-Liau][coleman-liau], [Flesch][flesch], [Gunning-Fog][gunning-fog],
[SMOG][smog], and [Spache][spache].

## Installation

[npm][]:

```bash
npm install retext-readability
```

## Usage

```js
var retext = require('retext');
var readability = require('retext-readability');
var report = require('vfile-reporter');

var doc = [
  'The cat sat on the mat',
  '',
  'The constellation also contains an isolated neutron ',
  'star—Calvera—and H1504+65, the hottest white dwarf yet ',
  'discovered, with a surface temperature of 200,000 kelvin',
  ''
].join('\n');
```

By default, the target age is 16:

```js
retext()
  .use(readability)
  .process(doc, function (err, file) {
    console.log(report(err || file));
  });
```

Yields:

```txt
   3:1-5:57  warning  Hard to read sentence (confidence: 4/7)  retext-readability

⚠ 1 warning
```

...but ages can be set, for example, to 8:

```js
retext()
  .use(readability, {age: 6})
  .process(doc, function (err, file) {
    console.log(report(err || file));
  });
```

Yields:

```txt
   1:1-1:23  warning  Hard to read sentence (confidence: 4/7)  retext-readability
   3:1-5:57  warning  Hard to read sentence (confidence: 7/7)  retext-readability

⚠ 1 warning
```

## API

### `retext().use(readability[, options])`

Detect possibly hard to read sentences.

###### `options`

*   `age` (`number`, default: `16`)
    — Target age group.  Note that the different algorithms
    provide varying results, so your milage may vary with
    people actually that age.  :wink:
*   `threshold` (`number`, default: `4 / 7`)
    — By default, 4 out of the 7 algorithms need to agree that
    a sentence is higher than the target age and whether it should
    be warned about.  This can be modified by passing in a new
    threshold.
*   `minWords` (`number`, default: `5`)
    — Minimum number of words a sentence should have when warning.
    Most algorithms are designed to take a large sample of
    sentences to detect the body’s reading level.  This plug-in,
    however, works on a per-sentence basis.  This makes the results
    quite skewered when said sentence has, for example, a few long
    words or some unknown ones.

## License

[MIT][license] © [Titus Wormer][author]

<!-- Definitions -->

[travis-badge]: https://img.shields.io/travis/wooorm/retext-readability.svg

[travis]: https://travis-ci.org/wooorm/retext-readability

[codecov-badge]: https://img.shields.io/codecov/c/github/wooorm/retext-readability.svg

[codecov]: https://codecov.io/github/wooorm/retext-readability

[npm]: https://docs.npmjs.com/cli/install

[license]: LICENSE

[author]: http://wooorm.com

[retext]: https://github.com/wooorm/retext

[dale-chall]: https://github.com/wooorm/dale-chall-formula

[automated-readability]: https://github.com/wooorm/automated-readability

[coleman-liau]: https://github.com/wooorm/coleman-liau

[flesch]: https://github.com/wooorm/flesch

[gunning-fog]: https://github.com/wooorm/gunning-fog

[spache]: https://github.com/wooorm/spache-formula

[smog]: https://github.com/wooorm/smog-formula
