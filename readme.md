<h1 align="center">
	<br>
	<br>
	<img width="320" src="media/logo.svg" alt="Chalk">
	<br>
	<br>
	<br>
</h1>


Donation : Paypal (https://www.paypal.com/donate?business=RBHMVN4AQGQE2&item_name=Donation&currency_code=BRL)
> Terminal string styling done right

[![Coverage Status](https://coveralls.io/repos/github/chalk/chalk/badge.svg?branch=main)](https://coveralls.io/github/chalk/chalk?branch=main)
[![npm dependents](https://badgen.net/npm/dependents/chalk)](https://www.npmjs.com/package/chalk?activeTab=dependents) [![Downloads](https://badgen.net/npm/dt/chalk)](https://www.npmjs.com/package/chalk)
[![run on repl.it](https://repl.it/badge/github/chalk/chalk)](https://repl.it/github/chalk/chalk)
[![Support Chalk on DEV](https://badge.devprotocol.xyz/0x44d871aebF0126Bf646753E2C976Aa7e68A66c15/descriptive)](https://stakes.social/0x44d871aebF0126Bf646753E2C976Aa7e68A66c15)

<img src="https://cdn.jsdelivr.net/gh/chalk/ansi-styles@8261697c95bf34b6c7767e2cbe9941a851d59385/screenshot.svg" width="900">

<br>

---

<div align="center">
	<p>
		<p>
			<sup>
				Sindre Sorhus' open source work is supported by the community on <a href="https://github.com/sponsors/sindresorhus">GitHub Sponsors</a> and <a href="https://stakes.social/0x44d871aebF0126Bf646753E2C976Aa7e68A66c15">Dev</a>
			</sup>
		</p>
		<sup>Special thanks to:</sup>
		<br>
		<br>
		<a href="https://standardresume.co/tech">
			<img src="https://sindresorhus.com/assets/thanks/standard-resume-logo.svg" width="160"/>
		</a>
		<br>
		<br>
		<a href="https://retool.com/?utm_campaign=sindresorhus">
			<img src="https://sindresorhus.com/assets/thanks/retool-logo.svg" width="210"/>
		</a>
		<br>
		<br>
		<a href="https://doppler.com/?utm_campaign=github_repo&utm_medium=referral&utm_content=chalk&utm_source=github">
			<div>
				<img src="https://dashboard.doppler.com/imgs/logo-long.svg" width="240" alt="Doppler">
			</div>
			<b>All your environment variables, in one place</b>
			<div>
				<span>Stop struggling with scattered API keys, hacking together home-brewed tools,</span>
				<br>
				<span>and avoiding access controls. Keep your team and servers in sync with Doppler.</span>
			</div>
		</a>
	</p>
</div>

---

<br>

## Highlights

- Expressive API
- Highly performant
- Ability to nest styles
- [256/Truecolor color support](#256-and-truecolor-color-support)
- Auto-detects color support
- Doesn't extend `String.prototype`
- Clean and focused
- Actively maintained
- [Used by ~50,000 packages](https://www.npmjs.com/browse/depended/chalk) as of January 1, 2020

## Install

```console
$ npm install chalk
```

## Usage

```js
import chalk from 'chalk';

console.log(chalk.blue('Hello world!'));
```

Chalk comes with an easy to use composable API where you just chain and nest the styles you want.

```js
import chalk from 'chalk';

const log = console.log;

// Combine styled and normal strings
log(chalk.blue('Hello') + ' World' + chalk.red('!'));

// Compose multiple styles using the chainable API
log(chalk.blue.bgRed.bold('Hello world!'));

// Pass in multiple arguments
log(chalk.blue('Hello', 'World!', 'Foo', 'bar', 'biz', 'baz'));

// Nest styles
log(chalk.red('Hello', chalk.underline.bgBlue('world') + '!'));

// Nest styles of the same type even (color, underline, background)
log(chalk.green(
	'I am a green line ' +
	chalk.blue.underline.bold('with a blue substring') +
	' that becomes green again!'
));

// ES2015 template literal
log(`
CPU: ${chalk.red('90%')}
RAM: ${chalk.green('40%')}
DISK: ${chalk.yellow('70%')}
`);

// ES2015 tagged template literal
log(chalk`
CPU: {red ${cpu.totalPercent}%}
RAM: {green ${ram.used / ram.total * 100}%}
DISK: {rgb(255,131,0) ${disk.used / disk.total * 100}%}
`);

// Use RGB colors in terminal emulators that support it.
log(chalk.rgb(123, 45, 67).underline('Underlined reddish color'));
log(chalk.hex('#DEADED').bold('Bold gray!'));
```

Easily define your own themes:

```js
import chalk from 'chalk';

const error = chalk.bold.red;
const warning = chalk.hex('#FFA500'); // Orange color

console.log(error('Error!'));
console.log(warning('Warning!'));
```

Take advantage of console.log [string substitution](https://nodejs.org/docs/latest/api/console.html#console_console_log_data_args):

```js
import chalk from 'chalk';

const name = 'Sindre';
console.log(chalk.green('Hello %s'), name);
//=> 'Hello Sindre'
```

## API

### chalk.`<style>[.<style>...](string, [string...])`

Example: `chalk.red.bold.underline('Hello', 'world');`

Chain [styles](#styles) and call the last one as a method with a string argument. Order doesn't matter, and later styles take precedent in case of a conflict. This simply means that `chalk.red.yellow.green` is equivalent to `chalk.green`.

Multiple arguments will be separated by space.

### chalk.level

Specifies the level of color support.

Color support is automatically detected, but you can override it by setting the `level` property. You should however only do this in your own code as it applies globally to all Chalk consumers.

If you need to change this in a reusable module, create a new instance:

```js
import {Chalk} from 'chalk';

const customChalk = new Chalk({level: 0});
```

| Level | Description |
| :---: | :--- |
| `0` | All colors disabled |
| `1` | Basic color support (16 colors) |
| `2` | 256 color support |
| `3` | Truecolor support (16 million colors) |

### supportsColor

Detect whether the terminal [supports color](https://github.com/chalk/supports-color). Used internally and handled for you, but exposed for convenience.

Can be overridden by the user with the flags `--color` and `--no-color`. For situations where using `--color` is not possible, use the environment variable `FORCE_COLOR=1` (level 1), `FORCE_COLOR=2` (level 2), or `FORCE_COLOR=3` (level 3) to forcefully enable color, or `FORCE_COLOR=0` to forcefully disable. The use of `FORCE_COLOR` overrides all other color support checks.

Explicit 256/Truecolor mode can be enabled using the `--color=256` and `--color=16m` flags, respectively.

### chalkStderr and supportsColorStderr

`chalkStderr` contains a separate instance configured with color support detected for `stderr` stream instead of `stdout`. Override rules from `supportsColor` apply to this too. `supportsColorStderr` is exposed for convenience.

## Styles

### Modifiers

- `reset` - Resets the current color chain.
- `bold` - Make text bold.
- `dim` - Emitting only a small amount of light.
- `italic` - Make text italic. *(Not widely supported)*
- `underline` - Make text underline. *(Not widely supported)*
- `inverse`- Inverse background and foreground colors.
- `hidden` - Prints the text, but makes it invisible.
- `strikethrough` - Puts a horizontal line through the center of the text. *(Not widely supported)*
- `visible`- Prints the text only when Chalk has a color level > 0. Can be useful for things that are purely cosmetic.

### Colors

- `black`
- `red`
- `green`
- `yellow`
- `blue`
- `magenta`
- `cyan`
- `white`
- `blackBright` (alias: `gray`, `grey`)
- `redBright`
- `greenBright`
- `yellowBright`
- `blueBright`
- `magentaBright`
- `cyanBright`
- `whiteBright`

### Background colors

- `bgBlack`
- `bgRed`
- `bgGreen`
- `bgYellow`
- `bgBlue`
- `bgMagenta`
- `bgCyan`
- `bgWhite`
- `bgBlackBright` (alias: `bgGray`, `bgGrey`)
- `bgRedBright`
- `bgGreenBright`
- `bgYellowBright`
- `bgBlueBright`
- `bgMagentaBright`
- `bgCyanBright`
- `bgWhiteBright`

## Tagged template literal

Chalk can be used as a [tagged template literal](https://exploringjs.com/es6/ch_template-literals.html#_tagged-template-literals).

```js
import chalk from 'chalk';

const miles = 18;
const calculateFeet = miles => miles * 5280;

console.log(chalk`
	There are {bold 5280 feet} in a mile.
	In {bold ${miles} miles}, there are {green.bold ${calculateFeet(miles)} feet}.
`);
```

Blocks are delimited by an opening curly brace (`{`), a style, some content, and a closing curly brace (`}`).

Template styles are chained exactly like normal Chalk styles. The following three statements are equivalent:

```js
import chalk from 'chalk';

console.log(chalk.bold.rgb(10, 100, 200)('Hello!'));
console.log(chalk.bold.rgb(10, 100, 200)`Hello!`);
console.log(chalk`{bold.rgb(10,100,200) Hello!}`);
```

Note that function styles (`rgb()`, `hex()`, etc.) may not contain spaces between parameters.

All interpolated values (`` chalk`${foo}` ``) are converted to strings via the `.toString()` method. All curly braces (`{` and `}`) in interpolated value strings are escaped.

## 256 and Truecolor color support

Chalk supports 256 colors and [Truecolor](https://gist.github.com/XVilka/8346728) (16 million colors) on supported terminal apps.

Colors are downsampled from 16 million RGB values to an ANSI color format that is supported by the terminal emulator (or by specifying `{level: n}` as a Chalk option). For example, Chalk configured to run at level 1 (basic color support) will downsample an RGB value of #FF0000 (red) to 31 (ANSI escape for red).

Examples:

- `chalk.hex('#DEADED').underline('Hello, world!')`
- `chalk.rgb(15, 100, 204).inverse('Hello!')`

Background versions of these models are prefixed with `bg` and the first level of the module capitalized (e.g. `hex` for foreground colors and `bgHex` for background colors).

- `chalk.bgHex('#DEADED').underline('Hello, world!')`
- `chalk.bgRgb(15, 100, 204).inverse('Hello!')`

The following color models can be used:

- [`rgb`](https://en.wikipedia.org/wiki/RGB_color_model) - Example: `chalk.rgb(255, 136, 0).bold('Orange!')`
- [`hex`](https://en.wikipedia.org/wiki/Web_colors#Hex_triplet) - Example: `chalk.hex('#FF8800').bold('Orange!')`
- [`ansi256`](https://en.wikipedia.org/wiki/ANSI_escape_code#8-bit) - Example: `chalk.bgAnsi256(194)('Honeydew, more or less')`

## Browser support

Since Chrome 69, ANSI escape codes are natively supported in the developer console.

## Windows

If you're on Windows, do yourself a favor and use [Windows Terminal](https://github.com/microsoft/terminal) instead of `cmd.exe`.

## Origin story

[colors.js](https://github.com/Marak/colors.js) used to be the most popular string styling module, but it has serious deficiencies like extending `String.prototype` which causes all kinds of [problems](https://github.com/yeoman/yo/issues/68) and the package is unmaintained. Although there are other packages, they either do too much or not enough. Chalk is a clean and focused alternative.

## chalk for enterprise

Available as part of the Tidelift Subscription.

The maintainers of chalk and thousands of other packages are working with Tidelift to deliver commercial support and maintenance for the open source dependencies you use to build your applications. Save time, reduce risk, and improve code health, while paying the maintainers of the exact dependencies you use. [Learn more.](https://tidelift.com/subscription/pkg/npm-chalk?utm_source=npm-chalk&utm_medium=referral&utm_campaign=enterprise&utm_term=repo)

## Related

- [chalk-cli](https://github.com/chalk/chalk-cli) - CLI for this module
- [ansi-styles](https://github.com/chalk/ansi-styles) - ANSI escape codes for styling strings in the terminal
- [supports-color](https://github.com/chalk/supports-color) - Detect whether a terminal supports color
- [strip-ansi](https://github.com/chalk/strip-ansi) - Strip ANSI escape codes
- [strip-ansi-stream](https://github.com/chalk/strip-ansi-stream) - Strip ANSI escape codes from a stream
- [has-ansi](https://github.com/chalk/has-ansi) - Check if a string has ANSI escape codes
- [ansi-regex](https://github.com/chalk/ansi-regex) - Regular expression for matching ANSI escape codes
- [wrap-ansi](https://github.com/chalk/wrap-ansi) - Wordwrap a string with ANSI escape codes
- [slice-ansi](https://github.com/chalk/slice-ansi) - Slice a string with ANSI escape codes
- [color-convert](https://github.com/qix-/color-convert) - Converts colors between different models
- [chalk-animation](https://github.com/bokub/chalk-animation) - Animate strings in the terminal
- [gradient-string](https://github.com/bokub/gradient-string) - Apply color gradients to strings
- [chalk-pipe](https://github.com/LitoMore/chalk-pipe) - Create chalk style schemes with simpler style strings
- [terminal-link](https://github.com/sindresorhus/terminal-link) - Create clickable links in the terminal

## Maintainers

- [Sindre Sorhus](https://github.com/sindresorhus)
- [Josh Junon](https://github.com/qix-)
