# d3-dsv

这个模块提供了一个针对分隔符文件/字符串的解析和格式化工具，大多数情况下是[comma-](https://en.wikipedia.org/wiki/Comma-separated_values) (CSV) 或 `tab` - 分割(TSV)。这种扁平的格式在表格类数据中很流行如 `Excel`， 并且比 `JSON` 更节省空间。这个模块的实现基于 [RFC 4180](http://tools.ietf.org/html/rfc4180)。

`CSV` 与 `TSV` 的转换以及格式化是内置的，比如解析：

```js
d3.csvParse("foo,bar\n1,2"); // [{foo: "1", bar: "2"}, columns: ["foo", "bar"]]
d3.tsvParse("foo\tbar\n1\t2"); // [{foo: "1", bar: "2"}, columns: ["foo", "bar"]]
```

或者格式化:

```js
d3.csvFormat([{foo: "1", bar: "2"}]); // "foo,bar\n1,2"
d3.tsvFormat([{foo: "1", bar: "2"}]); // "foo\tbar\n1\t2"
```

使用不同的分隔符，比如 "|" 来作为分割字符，则使用 [d3.dsvFormat](#dsvFormat):

```js
var psv = d3.dsvFormat("|");

console.log(psv.parse("foo|bar\n1|2")); // [{foo: "1", bar: "2"}, columns: ["foo", "bar"]]
```

为了在浏览器中方便的加载 `CSV` 文件，参考 [d3-request](https://github.com/xswei/d3-request) 的 [d3.csv](https://github.com/xswei/d3-request#csv) 和 [d3.tsv](https://github.com/xswei/d3-request#tsv) 方法。

## Installing

`NPM` 安装： `npm install d3-dsv`。此外还可以下载 [latest release](https://github.com/d3/d3-dsv/releases/latest)。可以直接从 [d3js.org](https://d3js.org) 以 [standalone library](https://d3js.org/d3-dsv.v1.min.js) 单独的标准库或者作为 [D3 4.0](https://github.com/d3/d3) 的一部分引入。支持 `AMD`, `CommonJS` 以及基础的标签引入形式，如果使用标签引入则会暴露 `d3` 全局变量:

```html
<script src="https://d3js.org/d3-dsv.v1.min.js"></script>
<script>

var data = d3.csvParse(string);

</script>
```

[在浏览器中测试 d3-dsv.](https://tonicdev.com/npm/d3-dsv)

## API Reference

<a name="csvParse" href="#csvParse">#</a> d3.<b>csvParse</b>(<i>string</i>[, <i>row</i>]) [<>](https://github.com/d3/d3-dsv/blob/master/src/csv.js#L5 "Source")

等价于 [dsvFormat](#dsvFormat)(",").[parse](#dsv_parse).

<a name="csvParseRows" href="#csvParseRows">#</a> d3.<b>csvParseRows</b>(<i>string</i>[, <i>row</i>]) [<>](https://github.com/d3/d3-dsv/blob/master/src/csv.js#L6 "Source")

等价于 [dsvFormat](#dsvFormat)(",").[parseRows](#dsv_parseRows).

<a name="csvFormat" href="#csvFormat">#</a> d3.<b>csvFormat</b>(<i>rows</i>[, <i>columns</i>]) [<>](https://github.com/d3/d3-dsv/blob/master/src/csv.js#L7 "Source")

等价于 [dsvFormat](#dsvFormat)(",").[format](#dsv_format).

<a name="csvFormatRows" href="#csvFormatRows">#</a> d3.<b>csvFormatRows</b>(<i>rows</i>) [<>](https://github.com/d3/d3-dsv/blob/master/src/csv.js#L8 "Source")

等价于 [dsvFormat](#dsvFormat)(",").[formatRows](#dsv_formatRows).

<a name="tsvParse" href="#tsvParse">#</a> d3.<b>tsvParse</b>(<i>string</i>[, <i>row</i>]) [<>](https://github.com/d3/d3-dsv/blob/master/src/tsv.js#L5 "Source")

等价于 [dsvFormat](#dsvFormat)("\t").[parse](#dsv_parse).

<a name="tsvParseRows" href="#tsvParseRows">#</a> d3.<b>tsvParseRows</b>(<i>string</i>[, <i>row</i>]) [<>](https://github.com/d3/d3-dsv/blob/master/src/tsv.js#L6 "Source")

等价于 [dsvFormat](#dsvFormat)("\t").[parseRows](#dsv_parseRows).

<a name="tsvFormat" href="#tsvFormat">#</a> d3.<b>tsvFormat</b>(<i>rows</i>[, <i>columns</i>]) [<>](https://github.com/d3/d3-dsv/blob/master/src/tsv.js#L7 "Source")

等价于 [dsvFormat](#dsvFormat)("\t").[format](#dsv_format).

<a name="tsvFormatRows" href="#tsvFormatRows">#</a> d3.<b>tsvFormatRows</b>(<i>rows</i>) [<>](https://github.com/d3/d3-dsv/blob/master/src/tsv.js#L8 "Source")

等价于 [dsvFormat](#dsvFormat)("\t").[formatRows](#dsv_formatRows).

<a name="dsvFormat" href="#dsvFormat">#</a> d3.<b>dsvFormat</b>(<i>delimiter</i>) [<>](https://github.com/d3/d3-dsv/blob/master/src/dsv.js#L30)

根据指定的 *delimiter* 构造一个新的 `DSV` 解析以及格式化。*delimiter* 必须是一个单字符(*i.e.*, 一个单一的 16位的代码单元); 所以 `ASCII` 可以作为分隔符，而 `emoji` 不可以。

<a name="dsv_parse" href="#dsv_parse">#</a> *dsv*.<b>parse</b>(<i>string</i>[, <i>row</i>]) [<>](https://github.com/d3/d3-dsv/blob/master/src/dsv.js#L34 "Source")

解析指定的 *string*, 返回解析后的行对象数组。

与 [*dsv*.parseRows](#dsv_parseRows) 不同, 这个方法要求 `DSV` 内容的第一行包含一组列名，这些列名将会被作为最终返回的数组的属性名。例如解析如下的 `CSV` 文件:

```
Year,Make,Model,Length
1997,Ford,E350,2.34
2000,Mercury,Cougar,2.38
```

返回的 JavaScript 数组为:

```js
[
  {"Year": "1997", "Make": "Ford", "Model": "E350", "Length": "2.34"},
  {"Year": "2000", "Make": "Mercury", "Model": "Cougar", "Length": "2.38"}
]
```

返回的数组会包含一个 `columns` 属性表示输入数据的原始列名次序(因为解析之后成为对象类型，对象类型的属性迭代次序是不可靠的)。例如:

```js
data.columns; // ["Year", "Make", "Model", "Length"]
```

如果没有指定 *row* 转换函数，则字段值将会是字符串。为了安全起见，没有将其自动转换为数值、日期或者其他的形式。在某些场景中，JavScript 会自动强制将字符串转为数值类型(例如使用 `+` 操作符), 但是更好的方式是使用 *row* 转换函数。

如果指定了 *row* 转换函数则会为每一行调用指定的函数，函数的参数依次为: 每一列的数据(`d`, 对象形式), 当前列的索引 `i` 以及列名数组。如果返回值为 `null` 或者 `undefined` 则当前行会被跳过并且最终不会出现在 *dsv*.parse 的解析结果中，返回值会作为当前行对象。例如:

```js
var data = d3.csvParse(string, function(d) {
  return {
    year: new Date(+d.Year, 0, 1), // lowercase and convert "Year" to Date
    make: d.Make, // lowercase
    model: d.Model, // lowercase
    length: +d.Length // lowercase and convert "Length" to number
  };
});
```

注意: 使用 `+` 比 [parseInt](https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/parseInt) 或 [parseFloat](https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/parseFloat) 通常刚快但也更严格. 例如, `"30px"` 使用 `+` 返回 `NaN`, 而使用 `parseInt` 和  `parseFloat` 返回 `30`.

<a name="dsv_parseRows" href="#dsv_parseRows">#</a> <i>dsv</i>.<b>parseRows</b>(<i>string</i>[, <i>row</i>]) [<>](https://github.com/d3/d3-dsv/blob/master/src/dsv.js#L43 "Source")

解析指定的 *string*, 返回一个数组的数组，每行数据以数组的形式表示.

与 [*dsv*.parse](#dsv_parse) 不同, 这个方法将第一行识别为数据而非列名，因此 `DSV` 内容中不应该再在第一行给出列名. 每一行会被解析为数组而不是对象. 并且数组的长度是可变的(取决于当前行可以被拆分为多少份). 例如解析如下没有列名行的 `CSV` 文件:

```
1997,Ford,E350,2.34
2000,Mercury,Cougar,2.38
```

返回的 JavaScript 数组为:

```js
[
  ["1997", "Ford", "E350", "2.34"],
  ["2000", "Mercury", "Cougar", "2.38"]
]
```

如果没有指定 *row* 转换函数，则字段值将会是字符串。为了安全起见，没有将其自动转换为数值、日期或者其他的形式。在某些场景中，JavScript 会自动强制将字符串转为数值类型(例如使用 `+` 操作符), 但是更好的方式是使用 *row* 转换函数。

如果指定了 *row* 转换函数则会为每一行调用指定的函数，函数的参数依次为: 每一列的数据(`d`, 数组形式), 当前列的索引 `i` 以及列名数组。如果返回值为 `null` 或者 `undefined` 则当前行会被跳过并且最终不会出现在 *dsv*.parseRows 的解析结果中，返回值会作为当前行对象。例如:

```js
var data = d3.csvParseRows(string, function(d, i) {
  return {
    year: new Date(+d[0], 0, 1), // convert first colum column to Date
    make: d[1],
    model: d[2],
    length: +d[3] // convert fourth column to number
  };
});
```

事实上，*row* 类似于将 [map](https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/Array/map) 和 [filter](https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/Array/filter) 应用到返回的结果中.

<a name="dsv_format" href="#dsv_format">#</a> <i>dsv</i>.<b>format</b>(<i>rows</i>[, <i>columns</i>]) [<>](https://github.com/d3/d3-dsv/blob/master/src/dsv.js#L105 "Source")

将指定的 *rows* 对象数组格式化为 `DSV` 字符串。这个操作是 [*dsv*.parse](#dsv_parse) 的逆操作。行与行之间使用 `\n` 分开并且行内列之间使用指定的分隔符分开(比如 `CSV` 使用 `,`)。如果行内某些数值已经包含分隔符或者换行符则使用双引号 `"` 来表示转义.

如果 *columns* 没有指定，则构成头行的列名的列表是由各行中所有对象的所有属性决定的，并且列名的次序是不确定的。如果指定了 *columns*，则其应该是由一组表示列名的字符串数组，并且列次序也会被确定。例如:

```js
var string = d3.csvFormat(data, ["year", "make", "model", "length"]);
```

每一个行对象上的所有字段都会被强制转换为字符串。为了能对格式化操作进行更多的控制比如字段如何被格式化，首先遍历行对象数组，然后使用 [*dsv*.formatRows](#dsv_formatRows)。

<a name="dsv_formatRows" href="#dsv_formatRows">#</a> <i>dsv</i>.<b>formatRows</b>(<i>rows</i>) [<>](https://github.com/d3/d3-dsv/blob/master/src/dsv.js#L114 "Source")

将指定行数组的数组格式化为字符串。这个操作是 [*dsv*.parseRows](#dsv_parseRows) 的逆操作。行与行之间使用 `\n` 分割，行内列与列之间使用指定的分隔符分割(比如 `CSV` 使用 `,`)。如果值中已经包含分隔符或者换行符则使用双引号表示转义。

在显式的指定列名称时可以先使用 [*array*.map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) 将对象数组转为数组的数组。例如:

```js
var string = d3.csvFormatRows(data.map(function(d, i) {
  return [
    d.year.getFullYear(), // Assuming d.year is a Date object.
    d.make,
    d.model,
    d.length
  ];
}));
```

如果你乐意也可以使用 [*array*.concat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat) 来生成一个带有列名的数组，这样最终格式化字符串的第一行会包含列名:

```js
var string = d3.csvFormatRows([[
    "year",
    "make",
    "model",
    "length"
  ]].concat(data.map(function(d, i) {
  return [
    d.year.getFullYear(), // Assuming d.year is a Date object.
    d.make,
    d.model,
    d.length
  ];
})));
```

### Content Security Policy

如果 [content security policy](http://www.w3.org/TR/CSP/) 到位的话，请注意 [*dsv*.parse](#dsv_parse) 可能会导致安全问题，因为其实现使用了 `不安全的 - eval`(这种方式解析更快)，可以参考 [源码](https://github.com/d3/d3-dsv/blob/master/src/dsv.js)，在不能保证安全时可以使用 [*dsv*.parseRows](#dsv_parseRows) 替代。

### Byte-Order Marks

`DSV` 文件有时候以 [byte order mark (BOM)](https://en.wikipedia.org/wiki/Byte_order_mark) 开始；例如从 `Microsoft Excel` 保存的 `UTF-8` 编码的 `CSV` 包含 `BOM`。在 `web` 上通常不是问题因为 [UTF-8 解码算法](https://encoding.spec.whatwg.org/#utf-8-decode) 会自动删除 `BOM`. 但是在 `Nodejs` 中使用 `UTF-8` 编码时 [不会移除 `BOM`](https://github.com/nodejs/node-v0.x-archive/issues/1918)。

如果 `BOM` 没有被移除则文本的第一个字符为一个零宽度的不间断空格。所以一个带有 `BOM` 的 `CSV` 由 [d3.csvParse](#csvParse) 解析时第一列的名称会以一个零宽度的不间断空格开头，这个很难被发现因为打印时不会被看见。

在解析之前移除 `BOM` 考虑使用 [strip-bom](https://www.npmjs.com/package/strip-bom).

## Command Line Reference

### dsv2dsv

<a name="dsv2dsv" href="#dsv2dsv">#</a> <b>dsv2dsv</b> [<i>options…</i>] [<i>file</i>]

Converts the specified DSV input *file* to DSV (typically with a different delimiter or encoding). If *file* is not specified, defaults to reading from stdin. For example, to convert to CSV to TSV:

```
csv2tsv < example.csv > example.tsv
```

To convert windows-1252 CSV to utf-8 CSV:

```
dsv2dsv --input-encoding windows-1252 < latin1.csv > utf8.csv
```

<a name="dsv2dsv_help" href="dsv2dsv_help">#</a> dsv2dsv <b>-h</b>
<br><a href="dsv2dsv_help">#</a> dsv2dsv <b>--help</b>

Output usage information.

<a name="dsv2dsv_version" href="dsv2dsv_version">#</a> dsv2dsv <b>-V</b>
<br><a href="dsv2dsv_version">#</a> dsv2dsv <b>--version</b>

Output the version number.

<a name="dsv2dsv_out" href="dsv2dsv_out">#</a> dsv2dsv <b>-o</b> <i>file</i>
<br><a href="dsv2dsv_out">#</a> dsv2dsv <b>--out</b> <i>file</i>

Specify the output file name. Defaults to “-” for stdout.

<a name="dsv2dsv_input_delimiter" href="dsv2dsv_input_delimiter">#</a> dsv2dsv <b>-r</b> <i>delimiter</i>
<br><a href="dsv2dsv_input_delimiter">#</a> dsv2dsv <b>--input-delimiter</b> <i>delimiter</i>

Specify the input delimiter character. Defaults to “,” for reading CSV. (You can enter a tab on the command line by typing ⌃V.)

<a name="dsv2dsv_input_encoding" href="dsv2dsv_input_encoding">#</a> dsv2dsv <b>--input-encoding</b> <i>encoding</i>

Specify the input character encoding. Defaults to “utf8”.

<a name="dsv2dsv_output_delimiter" href="dsv2dsv_output_delimiter">#</a> dsv2dsv <b>-w</b> <i>delimiter</i>
<br><a href="dsv2dsv_output_delimiter">#</a> dsv2dsv <b>--output-delimiter</b> <i>delimiter</i>

Specify the output delimiter character. Defaults to “,” for writing CSV. (You can enter a tab on the command line by typing ⌃V.)

<a name="dsv2dsv_output_encoding" href="dsv2dsv_output_encoding">#</a> dsv2dsv <b>--output-encoding</b> <i>encoding</i>

Specify the output character encoding. Defaults to “utf8”.

<a name="csv2tsv" href="#csv2tsv">#</a> <b>csv2tsv</b> [<i>options…</i>] [<i>file</i>]

Equivalent to [dsv2dsv](#dsv2dsv), but the [output delimiter](#dsv2dsv_output_delimiter) defaults to the tab character (\t).

<a name="tsv2csv" href="#tsv2csv">#</a> <b>tsv2csv</b> [<i>options…</i>] [<i>file</i>]

Equivalent to [dsv2dsv](#dsv2dsv), but the [input delimiter](#dsv2dsv_output_delimiter) defaults to the tab character (\t).

### dsv2json

<a name="dsv2json" href="#dsv2json">#</a> <b>dsv2json</b> [<i>options…</i>] [<i>file</i>]

Converts the specified DSV input *file* to JSON. If *file* is not specified, defaults to reading from stdin. For example, to convert to CSV to JSON:

```
csv2json < example.csv > example.json
```

Or to convert CSV to a newline-delimited JSON stream:

```
csv2json -n < example.csv > example.ndjson
```

<a name="dsv2json_help" href="dsv2json_help">#</a> dsv2json <b>-h</b>
<br><a href="dsv2json_help">#</a> dsv2json <b>--help</b>

Output usage information.

<a name="dsv2json_version" href="dsv2json_version">#</a> dsv2json <b>-V</b>
<br><a href="dsv2json_version">#</a> dsv2json <b>--version</b>

Output the version number.

<a name="dsv2json_out" href="dsv2json_out">#</a> dsv2json <b>-o</b> <i>file</i>
<br><a href="dsv2json_out">#</a> dsv2json <b>--out</b> <i>file</i>

Specify the output file name. Defaults to “-” for stdout.

<a name="dsv2json_input_delimiter" href="dsv2json_input_delimiter">#</a> dsv2json <b>-r</b> <i>delimiter</i>
<br><a href="dsv2json_input_delimiter">#</a> dsv2json <b>--input-delimiter</b> <i>delimiter</i>

Specify the input delimiter character. Defaults to “,” for reading CSV. (You can enter a tab on the command line by typing ⌃V.)

<a name="dsv2json_input_encoding" href="dsv2json_input_encoding">#</a> dsv2json <b>--input-encoding</b> <i>encoding</i>

Specify the input character encoding. Defaults to “utf8”.

<a name="dsv2json_output_encoding" href="dsv2json_output_encoding">#</a> dsv2json <b>-r</b> <i>encoding</i>
<br><a href="dsv2json_output_encoding">#</a> dsv2json <b>--output-encoding</b> <i>encoding</i>

Specify the output character encoding. Defaults to “utf8”.

<a name="dsv2json_newline_delimited" href="dsv2json_newline_delimited">#</a> dsv2json <b>-n</b>
<br><a href="dsv2json_newline_delimited">#</a> dsv2json <b>--newline-delimited</b>

Output [newline-delimited JSON](https://github.com/mbostock/ndjson-cli) instead of a single JSON array.

<a name="csv2json" href="#csv2json">#</a> <b>csv2json</b> [<i>options…</i>] [<i>file</i>]

Equivalent to [dsv2json](#dsv2json).

<a name="tsv2json" href="#csv2json">#</a> <b>tsv2json</b> [<i>options…</i>] [<i>file</i>]

Equivalent to [dsv2json](#dsv2json), but the [input delimiter](#dsv2json_input_delimiter) defaults to the tab character (\t).

### json2dsv

<a name="json2dsv" href="#json2dsv">#</a> <b>json2dsv</b> [<i>options…</i>] [<i>file</i>]

Converts the specified JSON input *file* to DSV. If *file* is not specified, defaults to reading from stdin. For example, to convert to JSON to CSV:

```
json2csv < example.json > example.csv
```

Or to convert a newline-delimited JSON stream to CSV:

```
json2csv -n < example.ndjson > example.csv
```

<a name="json2dsv_help" href="json2dsv_help">#</a> json2dsv <b>-h</b>
<br><a href="json2dsv_help">#</a> json2dsv <b>--help</b>

Output usage information.

<a name="json2dsv_version" href="json2dsv_version">#</a> json2dsv <b>-V</b>
<br><a href="json2dsv_version">#</a> json2dsv <b>--version</b>

Output the version number.

<a name="json2dsv_out" href="json2dsv_out">#</a> json2dsv <b>-o</b> <i>file</i>
<br><a href="json2dsv_out">#</a> json2dsv <b>--out</b> <i>file</i>

Specify the output file name. Defaults to “-” for stdout.

<a name="json2dsv_input_encoding" href="json2dsv_input_encoding">#</a> json2dsv <b>--input-encoding</b> <i>encoding</i>

Specify the input character encoding. Defaults to “utf8”.

<a name="json2dsv_output_delimiter" href="json2dsv_output_delimiter">#</a> json2dsv <b>-w</b> <i>delimiter</i>
<br><a href="json2dsv_output_delimiter">#</a> json2dsv <b>--output-delimiter</b> <i>delimiter</i>

Specify the output delimiter character. Defaults to “,” for writing CSV. (You can enter a tab on the command line by typing ⌃V.)

<a name="json2dsv_output_encoding" href="json2dsv_output_encoding">#</a> json2dsv <b>--output-encoding</b> <i>encoding</i>

Specify the output character encoding. Defaults to “utf8”.

<a name="json2dsv_newline_delimited" href="json2dsv_newline_delimited">#</a> json2dsv <b>-n</b>
<br><a href="json2dsv_newline_delimited">#</a> json2dsv <b>--newline-delimited</b>

Read [newline-delimited JSON](https://github.com/mbostock/ndjson-cli) instead of a single JSON array.

<a name="csv2json" href="#csv2json">#</a> <b>csv2json</b> [<i>options…</i>] [<i>file</i>]

Equivalent to [json2dsv](#json2dsv).

<a name="tsv2json" href="#csv2json">#</a> <b>tsv2json</b> [<i>options…</i>] [<i>file</i>]

Equivalent to [json2dsv](#json2dsv), but the [output delimiter](#json2dsv_output_delimiter) defaults to the tab character (\t).
