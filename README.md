# VSCode LCOV

Renders Line and Branch coverage from `lcov.info` files interactively.

## Live coverage info

![Live info](https://cloud.githubusercontent.com/assets/5047891/18036349/9101c648-6d68-11e6-9502-4ed14e03f51e.gif)

## Features

* loosely coupled, just point it to a `lcov.info` file and you're in business!
* can read multiple `lcov.info` files (e.g. one for the entire project, one for the last single test run)
* generates a Coverage Report
* supports watching certain files and executing a command (e.g. run test on file change)
* supports JavaScript source maps

## Coverage Report
 * `Ctrl+T` or `Cmd+T` then choose `Show Coverage Report`
![Coverage Report](https://cloud.githubusercontent.com/assets/5047891/18036350/94acd634-6d68-11e6-908b-a18ef6b80c0f.gif)

## Setting up

* There is a complete simple example at https://github.com/alexandrudima/vscode-lcov/tree/master/examples/fizzbuzz
* There are many tools that can generate `lcov.info` files
* I have tested `lcov.info` files generated by `istanbul` e.g.`istanbul cover --report lcovonly ./node_modules/mocha/bin/_mocha -- -R spec test.js`
* **Configure the paths** to your `lcov.info` files via the setting `lcov.path`. e.g:
```json
"lcov.path": [
    "./.build/coverage/lcov.info",
    "./.build/coverage-single/lcov.info"
]
```
* **Live coverage** via the setting `lcov.watch`. e.g. to execute a certain command any time a `.js` file is changed:
```json
"lcov.watch": [{
    "pattern": "**/*.js",
    "command": "npm run test-coverage"
}]
```
* Live coverage can be switched on via `Ctrl+T` or `Cmd+T` then choose `Enable watchers`
* **JavaScript sourcemaps** via the setting `lcov.sourceMaps`.
```json
"lcov.sourceMaps": true
```

## Troubleshooting
 * `View` > `Output` and choose `lcov`.

---

## Branch coverage: Configuration

```json
"lcov.branchCoverage": "off" or "simple" (default) or "full"
```

## Branch coverage: A single boolean expression

```js
exports.example = function (a) {
	if (a) {
		console.log('1');
	}
}
```

The above source code contains a single branch block consisting of two branches.

|  Test Code               | if taken | else taken |  "simple" |  "full" |
|--------------------------|:--------:|:----------:|---------|---------|
| `//no calls`             |   **∅**  |    **∅**   | ![image](https://cloud.githubusercontent.com/assets/5047891/18351583/115a7374-75db-11e6-9ffd-06214ae78f2d.png) | ![image](https://cloud.githubusercontent.com/assets/5047891/18345523/308ccea0-75bc-11e6-8d2c-f15cd8c8796c.png) |
| `example(1)`             |   **✓**  |    **∅**   | ![image](https://cloud.githubusercontent.com/assets/5047891/18346801/942d4d94-75c2-11e6-9925-1349ccffc2bb.png) | ![image](https://cloud.githubusercontent.com/assets/5047891/18346801/942d4d94-75c2-11e6-9925-1349ccffc2bb.png) |
| `example(0)`             |   **∅**  |    **✓**   | ![image](https://cloud.githubusercontent.com/assets/5047891/18346828/b20a862e-75c2-11e6-9404-b16cc955b150.png) | ![image](https://cloud.githubusercontent.com/assets/5047891/18346828/b20a862e-75c2-11e6-9404-b16cc955b150.png) |
| `example(0), example(1)` |   **✓**  |    **✓**   | ![image](https://cloud.githubusercontent.com/assets/5047891/18351595/2267bac8-75db-11e6-95a0-94686bc32b74.png) | ![image](https://cloud.githubusercontent.com/assets/5047891/18346867/d86d2394-75c2-11e6-8c76-ea6de4c57644.png) |

## Branch coverage: A binary boolean expression

```js
exports.example = function (a, b) {
	if (a && b) {
		console.log('1');
	}
}
```

|  Test Code                                     | if taken | else taken | a evaluated | b evaluated |  "simple" | "full" |
|------------------------------------------------|:--------:|:----------:|:-----------:|:-----------:|-----------|--------|
| `//no calls`                                   |   **∅**  |    **∅**   |    **∅**    |    **∅**    | ![image](https://cloud.githubusercontent.com/assets/5047891/18351364/29261450-75da-11e6-8483-11c34b4d0212.png) | ![image](https://cloud.githubusercontent.com/assets/5047891/18349139/f1d6c5d0-75cf-11e6-879b-f561bdd3d44a.png) |
| `example(0,0)`                                 |   **∅**  |    **✓**   |    **✓**    |    **∅**    | ![image](https://cloud.githubusercontent.com/assets/5047891/18351448/80d88048-75da-11e6-90c0-b4d96a029f0e.png) | ![image](https://cloud.githubusercontent.com/assets/5047891/18349161/162373e8-75d0-11e6-868a-bc2c3a9f0bf4.png) |
| `example(0,1)`                                 |   **∅**  |    **✓**   |    **✓**    |    **∅**    | ![image](https://cloud.githubusercontent.com/assets/5047891/18351448/80d88048-75da-11e6-90c0-b4d96a029f0e.png) | ![image](https://cloud.githubusercontent.com/assets/5047891/18349181/33c268b4-75d0-11e6-93e7-9e9249609511.png) |
| `example(1,0)`                                 |   **∅**  |    **✓**   |    **✓**    |    **✓**    | ![image](https://cloud.githubusercontent.com/assets/5047891/18351448/80d88048-75da-11e6-90c0-b4d96a029f0e.png) | ![image](https://cloud.githubusercontent.com/assets/5047891/18349199/4fa9beba-75d0-11e6-83b7-61c54407ad6d.png) |
| `example(1,1)`                                 |   **✓**  |    **∅**   |    **✓**    |    **✓**    | ![image](https://cloud.githubusercontent.com/assets/5047891/18351510/be93476a-75da-11e6-8be9-d62a097781a2.png) | ![image](https://cloud.githubusercontent.com/assets/5047891/18349218/62efca8c-75d0-11e6-8c6c-038e2ecd9ed7.png) |
| `example(0,0)`, `example(0,1)`                 |   **∅**  |    **✓**   |    **✓**    |    **∅**    | ![image](https://cloud.githubusercontent.com/assets/5047891/18351448/80d88048-75da-11e6-90c0-b4d96a029f0e.png) | ![image](https://cloud.githubusercontent.com/assets/5047891/18349295/f89324b2-75d0-11e6-88d9-08034771a6d3.png) |
| `example(0,0)`, `example(1,0)`                 |   **∅**  |    **✓**   |    **✓**    |    **✓**    | ![image](https://cloud.githubusercontent.com/assets/5047891/18351448/80d88048-75da-11e6-90c0-b4d96a029f0e.png) | ![image](https://cloud.githubusercontent.com/assets/5047891/18349328/2014641a-75d1-11e6-8e69-ac28a627abc0.png) |
| `example(0,0)`, `example(1,1)`                 |   **✓**  |    **✓**   |    **✓**    |    **✓**    | ![image](https://cloud.githubusercontent.com/assets/5047891/18351549/eb45eec0-75da-11e6-9f47-4587ca8af982.png) | ![image](https://cloud.githubusercontent.com/assets/5047891/18349355/481f52e4-75d1-11e6-8c18-f083095fb764.png) |
| `example(0,1)`, `example(1,0)`                 |   **∅**  |    **✓**   |    **✓**    |    **✓**    | ![image](https://cloud.githubusercontent.com/assets/5047891/18351448/80d88048-75da-11e6-90c0-b4d96a029f0e.png) | ![image](https://cloud.githubusercontent.com/assets/5047891/18349373/6ffbefe8-75d1-11e6-9762-388882e1f657.png) |
| `example(0,1)`, `example(1,1)`                 |   **✓**  |    **✓**   |    **✓**    |    **✓**    | ![image](https://cloud.githubusercontent.com/assets/5047891/18351549/eb45eec0-75da-11e6-9f47-4587ca8af982.png) | ![image](https://cloud.githubusercontent.com/assets/5047891/18349396/972f8066-75d1-11e6-8ce7-7880854705d2.png) |
| `example(1,0)`, `example(1,1)`                 |   **✓**  |    **✓**   |    **✓**    |    **✓**    | ![image](https://cloud.githubusercontent.com/assets/5047891/18351549/eb45eec0-75da-11e6-9f47-4587ca8af982.png) | ![image](https://cloud.githubusercontent.com/assets/5047891/18349414/ad7cb5be-75d1-11e6-8bed-f07b93c732e5.png) |
| `example(0,0)`, `example(0,1)`, `example(1,0)` |   **∅**  |    **✓**   |    **✓**    |    **✓**    | ![image](https://cloud.githubusercontent.com/assets/5047891/18351448/80d88048-75da-11e6-90c0-b4d96a029f0e.png) | ![image](https://cloud.githubusercontent.com/assets/5047891/18349517/251ff284-75d2-11e6-8945-d910b3b19284.png) |
| `example(0,0)`, `example(0,1)`, `example(1,1)` |   **✓**  |    **✓**   |    **✓**    |    **✓**    | ![image](https://cloud.githubusercontent.com/assets/5047891/18351549/eb45eec0-75da-11e6-9f47-4587ca8af982.png) | ![image](https://cloud.githubusercontent.com/assets/5047891/18349605/7747a390-75d2-11e6-87c5-8f3f887547bf.png) |
| `example(0,0)`, `example(1,0)`, `example(1,1)` |   **✓**  |    **✓**   |    **✓**    |    **✓**    | ![image](https://cloud.githubusercontent.com/assets/5047891/18351549/eb45eec0-75da-11e6-9f47-4587ca8af982.png) | ![image](https://cloud.githubusercontent.com/assets/5047891/18349636/96680152-75d2-11e6-95da-38f11be1ad15.png) |
| `example(0,1)`, `example(1,0)`, `example(1,1)` |   **✓**  |    **✓**   |    **✓**    |    **✓**    | ![image](https://cloud.githubusercontent.com/assets/5047891/18351549/eb45eec0-75da-11e6-9f47-4587ca8af982.png) | ![image](https://cloud.githubusercontent.com/assets/5047891/18349673/b2d450fc-75d2-11e6-8652-81a3f3ce5e26.png) |

## Branch coverage: A ternary boolean expression

```js
exports.example = function (a, b, c) {
	if (a && b && c) {
		console.log('1');
	}
}
```

|  Test Code                                     | if taken | else taken | a evaluated | b evaluated | c evaluated |  Output |
|------------------------------------------------|:--------:|:----------:|:-----------:|:-----------:|:-----------:|---------|
| `//no calls`                                   |   **∅**  |    **∅**   |    **∅**    |    **∅**    |    **∅**    | ![image](https://cloud.githubusercontent.com/assets/5047891/18350180/0794aae0-75d5-11e6-8d70-f06642253e73.png) |
| `example(0,0,0)`                               |   **∅**  |    **✓**   |    **✓**    |    **∅**    |    **∅**    | ![image](https://cloud.githubusercontent.com/assets/5047891/18350238/4c59e532-75d5-11e6-9611-cdc10037c1e6.png) |
| `example(1,0,0)`                               |   **∅**  |    **✓**   |    **✓**    |    **✓**    |    **∅**    | ![image](https://cloud.githubusercontent.com/assets/5047891/18350272/6bb6eb0a-75d5-11e6-8be3-3ece76abd900.png) |
| `example(1,1,0)`                               |   **∅**  |    **✓**   |    **✓**    |    **✓**    |    **✓**    | ![image](https://cloud.githubusercontent.com/assets/5047891/18350302/892c2bfa-75d5-11e6-9f38-964d8f58dc1a.png) |
| `example(1,1,1)`                               |   **✓**  |    **∅**   |    **✓**    |    **✓**    |    **✓**    | ![image](https://cloud.githubusercontent.com/assets/5047891/18350320/a53d25b0-75d5-11e6-89e2-98d2ed4ba95d.png) |

