<img alt="Logo" src="logo.svg" width=300>

# [Vector Express](https://www.smidyo.com/vector-express) - Free Vector Conversion, Analyzing and Processing API

### Recent updates
- New /cad/ convertor for svg -> dxf conversion
- New /linearmovement/ analyzer
- New /ungroup/, /convert-to-path/ and /boolean-operation/ processors
- Updated SVGO to 2.3.0
- Config option for SVGO
- More configuration options for the CadLib converter

Read more: https://www.smidyo.com/vector-express
API Frontend: https://vector.express

Vector Express is a free-to-use API for converting, analyzing and processing vector files. Made with love by [@Smidyo](https://twitter.com/smidyo)

It runs a combination of different conversion programs that can be chained together to convert between a wide area of formats.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     | ✓  | ✓   | ✓   | ✓   | ✓   | ✓    | ✓   | ✓   | ✓  | ✓   |
| Out    |    |     |     | ✓   | ✓   |      | ✓   | ✓   | ✓  | ✓   |


Feel free to use it in your project. It does not support CORS, so you need to run it through/on a back-end.

## 📦 Libraries

- [Node.js - @smidyo/vectorexpress-nodejs](https://github.com/smidyo/vectorexpress-nodejs)


## 🏃‍♀️ Quickstart

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/890e639ae3094e3617c0)

1. GET the a compatible conversion `path` for your input format and desired output format.

`curl https://vector.express/api/v2/public/convert/dxf/auto/svg/`

2. POST to the the first path, with your file as the body

`curl --data-binary @myvector.dxf https://vector.express/api/v2/public/convert/dxf/cadlib/svg/`

3. GET the file from the `resultUrl`

`curl https://vector.express/api/v2/public/files/[id].svg --output converted.svg`



## 🪄 Convert

### Get possible conversion paths between formats

By requesting the conversion paths you can find out the most efficient path between your formats.

`GET https://vector.express/api/v2/public/convert/ext/auto/ext`


###  Convert a file

You can up chain to three programs in the conversion path, and even configure them. See below for all programs and their options.

`POST https://vector.express/api/v2/public/convert/ext/prog1/ext/prog2/ext?prog1-opt=val&prog2-opt=val`


### Available convertors

#### /cad/

Custom CAD-based converter for svg to AutoCAD conversion.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    |     |     |     |     |      |     |     |    | ✓   |
| Out    |    |     | ✓   | ✓   |     |      |     |     |    |     |

| Option | Type | Description |
|:-------|:-----|:------------|
|nojoin|Boolean|If enabled, do not join adjacent lines|
|version|String|Which AutoCAD version to use<br/>`R32` / `R27` / `R24` / `R21` / `R18` / `R15` / `R14`|

#### /cadlib/

Custom CadLib based converter for AutoCAD files.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    |     | ✓   | ✓   |     |      |     |     |    |     |
| Out    |    |     |     | ✓   |     |      |     |     |    | ✓ *1|

*1 Arcs and curves are converted to lines

| Option | Type | Description |
|:-------|:-----|:------------|
|epsilon|Number||
|arc-segments|Number|Line segment amount on arcs|
|arc-segments-minimum|Number|The minimum amount of segments on arc|
|space-strategy|String|Which ACAD space to prefer for export<br/>`prefer_native_space` / `prefer_paper_space` / `prefer_model_space`|


#### /gs/

[Ghostscript](https://www.ghostscript.com/) based converter.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     | ✓  |     |     |     | ✓   |      | ✓   |     | ✓  |     |
| Out    |    |     |     |     | ✓   |      | ✓   |     | ✓  |     |


#### /hp2xx/

[hp2xx](https://www.gnu.org/software/hp2xx/) based converter for postscript.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    |     |     |     |     | ✓    | ✓   |     |    |     |
| Out    |    |     |     |     | ✓   |      |     |     |    | ✓   |


#### /libcdr/

[libcdr](https://wiki.documentfoundation.org/DLP/Libraries/libcdr) based converter for CorelDRAW files.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    | ✓   |     |     |     |      |     |     |    |     |
| Out    |    |     |     |     |     |      |     |     |    | ✓   |


#### /librsvg/

[librsvg](https://github.com/GNOME/librsvg) based converter for converting SVG files to various formats.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    |     |     |     |     |      |     |     |    | ✓   |
| Out    |    |     |     |     | ✓   |      | ✓   |     | ✓  |     |


#### /pdf2svg/

[pdf2svg](https://github.com/dawbarton/pdf2svg) based converter for PDF files to SVG.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    |     |     |     |     |      | ✓   |     |    |     |
| Out    |    |     |     |     |     |      |     |     |    | ✓   |

| Option | Type | Description |
|:-------|:-----|:------------|
|page|Number|Which page to export|


#### /pstoedit/

[pstoedit](http://www.calvina.de/pstoedit/) for converting eps files to dxf.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    |     |     |     | ✓   |      |     |     |    |     |
| Out    |    |     |     | ✓   |     |      |     |     |    |     |

| Option | Type | Description |
|:-------|:-----|:------------|
|page|Number|Which page to export|
|xscale|Number|X scale|
|yscale|Number|Y scale|
|xshift|Number|X shift|
|yshift|Number|Y shift|
|centered|Boolean|Center drawing|
|rgb|Boolean|


#### /svgo/

[SVGO](https://github.com/svg/svgo) is an svg optimizer.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    |     |     |     |     |      |     |     |    | ✓   |
| Out    |    |     |     |     |     |      |     |     |    | ✓   |

| Option | Type | Description |
|:-------|:-----|:------------|
|config|String|JSON string of [configuration](https://github.com/svg/svgo#configuration), representing the default export configuration object. This overrides all other configuration parameters if set|
|configSvgo2Syntax|Boolean|Enable this to use the SVGO 2.0 configuration syntax, otherwise v1 syntax will be used|
|enable|String|Which plugins to enable (see SVGO docs)|
|disable|String|Which plugins to disable (see SVGO docs)|
|indent|String||
|pretty|Boolean|Prettify code|
|precision|Number|Precision|
|multipass|Boolean||


#### /uniconvertor/

[Uniconvertor](https://sk1project.net/uc2/) based converter for various formats.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    |     |     |     | ✓   |      |     | ✓   |    | ✓   |
| Out    |    |     |     |     |     |      | ✓   | ✓   |    | ✓   |



## 🔍 Analyze

### Analyze a file

You can analyze vector files using this endpoint. The result is always a json file. The analyzers can be configured through the query string.

`POST https://vector.express/api/v2/public/analyze/ext/analyzer`

`POST https://vector.express/api/v2/public/analyze/ext/analyzer1?analyzer-opt1=val&analyzer-opt2=val`

### Available analyzers

#### /svg/groups/

Returns a list of all groups in an SVG.

#### /svg/linearmovement/

Simulates 2D linear movement along all paths in the SVG. This can be used for 2D CNC estimation, for example laser cutting, routing, knife cutting and more.

| Option | Type | Description |
|:-------|:-----|:------------|
|jerk|Number|An arbitrary unit setting the amount of jerk in the movement. Higher number = less deceleration around tight corners.|
|mm-per-s|Number|The speed to move along the paths, in mm/s.|
|drawing-unit|String|Which unit the SVG is drawn in.<br/>`mm` / `in` / `pt` / `px`|
|path-info|Boolean|If enabled, information about individual paths and path segments are included in the outputs.|



## ⚙️ Process

### Process a file

You can process vector files using this endpoint. The processors can be configured through the query string.

`POST https://vector.express/api/v2/public/process/ext/processor`

`POST https://vector.express/api/v2/public/process/ext/processor?processor-opt1=val&processor-opt2=val`

### Available processors

#### /svg/exclude-groups/

Excludes certain groups from an SVG.

| Option | Type | Description |
|:-------|:-----|:------------|
|groups|String|A list of group ID's to exclude, separated using the NUL syntax (%00)|

#### /svg/include-only-groups/

Includes only certain groups from an SVG.

| Option | Type | Description |
|:-------|:-----|:------------|
|groups|String|A list of group ID's to include, separated using the NUL syntax (%00)|

#### /svg/ungroup/

Ungroups all elements recursively. If you require a higher depth than 10, you can make multiple calls.

| Option | Type | Description |
|:-------|:-----|:------------|
|depth|Number|The depth to ungroup, 1 - 10. Default is 10.|

#### /svg/convert-to-path/

Converts elements (text*, circle, rectangles, etc.) to paths.  

| Option | Type | Description |
|:-------|:-----|:------------|
|selector|String|CSS-style selector** to define which elements to convert. Default is "*".|

\* A selection of open source fonts are compatible. Contact us if you need to convert an unsupported font.
\** A sub-set of CSS2 selectors are supported. You can select by class and id, as well as exact attribute values. If selecting by fill/stroke defined in a `style` attribute, it may be beneficial to first run the SVG through the `/svgo/` converter using the `convertStyleToAttrs` plugin enabled.

#### /svg/boolean-operation/

Performs a boolean operation on _target_ element(s) using _tool_ element(s).

Keep in mind that this will not work on grouped elements and/or non-path elements. Please run the `convert-to-path` and `boolean-operation` processors on the drawing first.

| Option | Type | Description |
|:-------|:-----|:------------|
|operation|String|Which operation to perform. Default is `union`.<br/>`union` / `difference` / `intersection` / `exclusion` / `division` / `cut-path` / `combine`  / `break-apart`|
|tool-paths|String|CSS-style selector* to define the elements to use as the tool. Default is "*".|
|target-paths|String|CSS-style selector* to define the elements to use as the target. Default is "*".|

\* A sub-set of CSS2 selectors are supported. You can select by class and id, as well as exact attribute values. If selecting by fill/stroke defined in a `style` attribute, it may be beneficial to first run the SVG through the `/svgo/` converter using the `convertStyleToAttrs` plugin enabled.

#### /svg/xpath/

Returns a section of an SVG file using the xpath syntax.

| Option | Type | Description |
|:-------|:-----|:------------|
|xpath|String|The xpath query|
|text-output|Boolean|If true, the resulting file is a .txt, otherwise it is an .xml file|
|add-root|Boolean|Whether or not to add the root element|



## 📄 Get a file

After getting the result you can retrieve your files here.

`GET https://vector.express/api/v2/public/files/filename.ext`



## 📨 Using an existing file 

You can also use an already uploaded or resulting file through the `use-file` query string.

`POST https://vector.express/api/v2/public/convert/ext/prog1/ext/prog2?use-file=filename.ext`



## 🛑 Limits

Currently the public API is limited to 60 requests per hour, and a maximum output filesize of 10mb and certain CPU and memory limitations.



## 🔼 Increase limits?

We offer a pay-as-you go plan that removes the rate limit and increases maximum file to 40 MB. Read more here: https://www.smidyo.com/vector-express



## 💡 Feature request?

Get in touch with us and we'll see what we can do!



## 🙊 Feedback

Feel free to open up an issue, or just tweet at us @smidyo.

You can also shoot us a mail at frank@smidyo.com!
