<img alt="Logo" src="logo.svg" width=300>

# [Vector Express](https://www.smidyo.com/vector-express) - Free Vector Conversion, Analyzing and Processing API

### Recent updates
- /ungroup/ processor now has a "selector" parameter
- New /change-attribute/ and /xslt/ processor
- New /fix-illustrator-svg-font-names/ processor
- New /cad/ convertor for svg -> dxf conversion
- New /linearmovement/ analyzer
- New /ungroup/, /convert-to-path/ and /boolean-operation/ processors
- Updated SVGO to 2.3.0
- Config option for SVGO

Read more: https://www.smidyo.com/vector-express
API Frontend: https://vector.express

Vector Express is a free-to-use API for converting, analyzing and processing vector files. Made with love by [@Smidyo](https://twitter.com/smidyo)

It runs a combination of different conversion programs that can be chained together to convert between a wide area of formats.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     | ✓  | ✓   | ✓   | ✓   | ✓   | ✓    | ✓   | ✓   | ✓  | ✓   |
| Out    |    |     | ✓   | ✓   | ✓   |      | ✓   | ✓   | ✓  | ✓   |


Feel free to use it in your project. It does not support CORS, so you need to run it through/on a back-end.

### Table of Contents
* 📦  [**Libraries**](#libraries)
* 🏃‍  [**Quickstart**](#quickstart)
* 🪄  [**Convert**](#convert)
  * [Get possible conversion paths between formats](#convert-get-formats)
  * [Convert a file](#convert-file)
  * [Available convertors](#available-convertors)
    * [/cad/](#convertor-cad)
    * [/cadlib/](#convertor-cadlib)
    * [/gs/](#convertor-gs)
    * [/hp2xx/](#convertor-hp2xx)
    * [/libcdr/](#convertor-libcdr)
    * [/librsvg/](#convertor-librsvg)
    * [/pdf2svg/](#convertor-pdf2svg)
    * [/pstoedit/](#convertor-pstoedit)
    * [/svgo/](#convertor-svgo)
    * [/uniconvertor/](#convertor-uniconvertor)
* 🔍  [**Analyze**](#analyze)
  * [Analyze a file](#analyze-file)
  * [Available analyzers](#available-analyzers)
    * [/svg/groups/](#analyzer-svg-groups)
    * [/svg/linearmovement/](#analyzer-svg-linearmovement)
* ⚙️ [**Process**](#process)
  * [Process a file](#process-file)
  * [Available processors](#available-processors)
    * [/svg/boolean-operation/](#processor-svg-boolean-operation)
    * [/svg/change-attribute/](#processor-change-attribute)
    * [/svg/convert-to-path/](#processor-svg-convert-to-path)
    * [/svg/exclude-groups/](#processor-svg-exclude-groups)
    * [/svg/fix-illustrator-svg-font-names/](#processor-svg-fix-illustrator-svg-font-names)
    * [/svg/include-only-groups/](#processor-svg-include-only-groups)
    * [/svg/ungroup/](#processor-svg-ungroup)
    * [/svg/xpath/](#processor-svg-xpath)
* 📄  [**Get a file**](#get-file)
* 📨  [**Using an existing file**](#using-existing-file)
* 🛑  [**Limits**](#limits)
* 🔼  [**Increase limits?**](#increase-limits)
* 💡  [**Feature request?**](#feature-request)
* 🙊  [**Feedback**](#feedback)

## 📦 <a name="libraries">Libraries</a>

- [Node.js - @smidyo/vectorexpress-nodejs](https://github.com/smidyo/vectorexpress-nodejs)


## 🏃‍♀️ <a name="quickstart">Quickstart</a>

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/890e639ae3094e3617c0)

1. GET the a compatible conversion `path` for your input format and desired output format.

`curl https://vector.express/api/v2/public/convert/dxf/auto/svg/`

2. POST to the the first path, with your file as the body

`curl --data-binary @myvector.dxf https://vector.express/api/v2/public/convert/dxf/cadlib/svg/`

3. GET the file from the `resultUrl`

`curl https://vector.express/api/v2/public/files/[id].svg --output converted.svg`



## 🪄 <a name="convert">Convert</a>

### <a name="convert-get-formats">Get possible conversion paths between formats</a>

By requesting the conversion paths you can find out the most efficient path between your formats.

`GET https://vector.express/api/v2/public/convert/ext/auto/ext`


### <a name="convert-file">Convert a file</a>

You can up chain to three programs in the conversion path, and even configure them. See below for all programs and their options.

`POST https://vector.express/api/v2/public/convert/ext/prog1/ext/prog2/ext?prog1-opt=val&prog2-opt=val`


### Available convertors

#### <a name="convertor-cad">/cad/</a>

Custom CAD-based converter for svg to AutoCAD conversion.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    |     |     |     |     |      |     |     |    | ✓   |
| Out    |    |     | ✓   | ✓   |     |      |     |     |    |     |

| Option | Type | Description |
|:-------|:-----|:------------|
|nojoin|Boolean|If enabled, do not join adjacent lines|
|version|String|Which AutoCAD version to use<br/>`R32` / `R27` / `R24` / `R21` / `R18` / `R15` / `R14`|

#### <a name="convertor-cadlib">/cadlib/</a>

Custom CadLib based converter for AutoCAD files.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    |     | ✓   | ✓   |     |      |     |     |    |     |
| Out    |    |     |     | ✓   |     |      |     |     |    | ✓ *1|

*1 Arcs and curves are converted to lines

| Option | Type | Description |
|:-------|:-----|:------------|
|cadlib-epsilon|Number||
|cadlib-arc-segments|Number|Line segment amount on arcs|
|cadlib-arc-segments-minimum|Number|The minimum amount of segments on arc|
|cadlib-space-strategy|String|Which ACAD space to prefer for export<br/>`prefer_native_space` / `prefer_paper_space` / `prefer_model_space`|


#### <a name="convertor-gs">/gs/</a>

[Ghostscript](https://www.ghostscript.com/) based converter.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     | ✓  |     |     |     | ✓   |      | ✓   |     | ✓  |     |
| Out    |    |     |     |     | ✓   |      | ✓   |     | ✓  |     |


#### <a name="convertor-hp2xx">/hp2xx/</a>

[hp2xx](https://www.gnu.org/software/hp2xx/) based converter for postscript.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    |     |     |     |     | ✓    | ✓   |     |    |     |
| Out    |    |     |     |     | ✓   |      |     |     |    | ✓   |


#### <a name="convertor-libcdr">/libcdr/</a>

[libcdr](https://wiki.documentfoundation.org/DLP/Libraries/libcdr) based converter for CorelDRAW files.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    | ✓   |     |     |     |      |     |     |    |     |
| Out    |    |     |     |     |     |      |     |     |    | ✓   |


#### <a name="convertor-librsvg">/librsvg/</a>

[librsvg](https://github.com/GNOME/librsvg) based converter for converting SVG files to various formats.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    |     |     |     |     |      |     |     |    | ✓   |
| Out    |    |     |     |     | ✓   |      | ✓   |     | ✓  |     |


#### <a name="convertor-pdf2svg">/pdf2svg/</a>

[pdf2svg](https://github.com/dawbarton/pdf2svg) based converter for PDF files to SVG.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    |     |     |     |     |      | ✓   |     |    |     |
| Out    |    |     |     |     |     |      |     |     |    | ✓   |

| Option | Type | Description |
|:-------|:-----|:------------|
|pdf2svg-page|Number|Which page to export|


#### <a name="convertor-pstoedit">/pstoedit/</a>

[pstoedit](http://www.calvina.de/pstoedit/) for converting eps files to dxf.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    |     |     |     | ✓   |      |     |     |    |     |
| Out    |    |     |     | ✓   |     |      |     |     |    |     |

| Option | Type | Description |
|:-------|:-----|:------------|
|pstoedit-page|Number|Which page to export|
|pstoedit-xscale|Number|X scale|
|pstoedit-yscale|Number|Y scale|
|pstoedit-xshift|Number|X shift|
|pstoedit-yshift|Number|Y shift|
|pstoedit-centered|Boolean|Center drawing|
|pstoedit-rgb|Boolean|


#### <a name="convertor-svgo">/svgo/</a>

[SVGO](https://github.com/svg/svgo) is an svg optimizer.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    |     |     |     |     |      |     |     |    | ✓   |
| Out    |    |     |     |     |     |      |     |     |    | ✓   |

| Option | Type | Description |
|:-------|:-----|:------------|
|svgo-config|String|JSON string of [configuration](https://github.com/svg/svgo#configuration), representing the default export configuration object. This overrides all other configuration parameters if set|
|svgo-configSvgo2Syntax|Boolean|Enable this to use the SVGO 2.0 configuration syntax, otherwise v1 syntax will be used|
|svgo-enable|String|Which plugins to enable (see SVGO docs)|
|svgo-disable|String|Which plugins to disable (see SVGO docs)|
|svgo-indent|String||
|svgo-pretty|Boolean|Prettify code|
|svgo-precision|Number|Precision|
|svgo-multipass|Boolean||


#### <a name="convertor-uniconvertor">/uniconvertor/</a>

[Uniconvertor](https://sk1project.net/uc2/) based converter for various formats.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    |     |     |     | ✓   |      |     | ✓   |    | ✓   |
| Out    |    |     |     |     |     |      | ✓   | ✓   |    | ✓   |



## <a name="analyze">🔍 Analyze</a>

### <a name="analyze-file">Analyze a file</a>

You can analyze vector files using this endpoint. The result is always a json file. The analyzers can be configured through the query string.

`POST https://vector.express/api/v2/public/analyze/ext/analyzer`

`POST https://vector.express/api/v2/public/analyze/ext/analyzer1?analyzer-opt1=val&analyzer-opt2=val`

### <a name="available-analyzers">Available analyzers</a>

#### <a name="analyzer-svg-groups">/svg/groups/</a>

Returns a list of all groups in an SVG.

#### <a name="analyzer-svg-linearmovement">/svg/linearmovement/</a>

Simulates 2D linear movement along all paths in the SVG. This can be used for 2D CNC estimation, for example laser cutting, routing, knife cutting and more.

| Option | Type | Description |
|:-------|:-----|:------------|
|linearmovement-jerk|Number|An arbitrary unit setting the amount of jerk in the movement. Higher number = less deceleration around tight corners.|
|linearmovement-mm-per-s|Number|The speed to move along the paths, in mm/s.|
|linearmovement-drawing-unit|String|Which unit the SVG is drawn in.<br/>`mm` / `in` / `pt` / `px`|
|linearmovement-path-info|Boolean|If enabled, information about individual paths and path segments are included in the outputs.|



## <a name="process">⚙️ Process</a>

### <a name="process-file">Process a file</a>

You can process vector files using this endpoint. The processors can be configured through the query string.

`POST https://vector.express/api/v2/public/process/ext/processor`

`POST https://vector.express/api/v2/public/process/ext/processor?processor-opt1=val&processor-opt2=val`

### <a name="available-processors">Available processors</a>

#### <a name="processor-svg-boolean-operation">/svg/boolean-operation/</a>

Performs a boolean operation on _target_ element(s) using _tool_ element(s).

Keep in mind that this *will not work* on grouped elements and/or non-path elements. Please run the `convert-to-path` and `ungroup` processors on the drawing first.

| Option | Type | Description |
|:-------|:-----|:------------|
|boolean-operation-operation|String|Which operation to perform. Default is `union`.<br/>`union` / `difference` / `intersection` / `exclusion` / `division` / `cut-path` / `combine`  / `break-apart`|
|boolean-operation-tool-paths|String|[XPath](https://www.w3schools.com/xml/xpath_syntax.asp) selector* to define the elements to use as the tool. Default is "//svg:path".|
|boolean-operation-target-paths|String|[XPath](https://www.w3schools.com/xml/xpath_syntax.asp) selector* to define the elements to use as the target. Default is "//svg:path".|

\* Please note that elements need to be prefixed with `svg:` to match the SVG namespace. Also remember to URL-encode your xpath, otherwise it may not work. E.g. to select all elements elements with a style to be filled with white: `//svg:path[contains(@style,'#ffffff')` -> `//svg:path[contains(%40style,'%23ffffff')`

#### <a name="processor-svg-convert-to-path">/svg/change-attribute/</a>

Allows modification of attributes or inline CSS properties on certain elements. This can be used to set fill, stroke and many other parameters. You can also use an existing attribute value to set the new one. (For example, setting an outline to match the fill of a path) 

| Option | Type | Description |
|:-------|:-----|:------------|
|change-attribute-set-attr|String|Which attribute to set. Required.|
|change-attribute-target-elements|String|Which set of element types to affect, separated with a comma. Required.|
|change-attribute-to-value|String|What value to set the attribute to.|
|change-attribute-to-attr-value|String|This can be set to get the value from another attribute on the same element. If this is set, "to-value" is ignored.|
|change-attribute-to-attr-value-fallback|String|An optional fallback to set the attribute to if the attribute from "to-attr-value" has no value.|
|change-attribute-override|String|If this is set, the value will always be set. If used in combination with not setting "to-value" or "to-attr-value", it will remove the attribute.|

#### <a name="processor-svg-convert-to-path">/svg/convert-to-path/</a>

Converts elements (text*, circle, rectangles, etc.) to paths. If your SVG file is exported from Adobe Illustrator, consider running the [fix-illustrator-svg-font-names](#processor-svg-fix-illustrator-svg-font-names) processor first.

| Option | Type | Description |
|:-------|:-----|:------------|
|convert-to-path-selector|String|[XPath](https://www.w3schools.com/xml/xpath_syntax.asp) selector** to define which elements to convert. Default is "//svg:*".|

\* A selection of open source fonts are compatible. Contact us if you need to convert an unsupported font.<br/>
\** Please note that elements need to be prefixed with `svg:` to match the SVG namespace. Also remember to URL-encode your xpath, otherwise it may not work. E.g. to select all elements elements with a style to be filled with white: `//svg:path[contains(@style,'#ffffff')` -> `//svg:path[contains(%40style,'%23ffffff')`

#### <a name="processor-svg-exclude-groups">/svg/exclude-groups/</a>

Excludes certain groups from an SVG.

| Option | Type | Description |
|:-------|:-----|:------------|
|exclude-groups-groups|String|A list of group ID's to exclude, separated using the NUL syntax (%00)|

#### <a name="processor-svg-fix-illustrator-svg-font-names">/svg/fix-illustrator-svg-font-names/</a>

Attempts a correction of Adobe Illustrator's [incorrectly exported](https://gitlab.com/inkscape/inbox/-/issues/3673) font-family tag attribute/CSS property. For example, it will turn `font-family="OpenSans-Bold"` into `font-family="OpenSans" font-weight="bold"`.

This is required for the [convert-to-path](#processor-svg-convert-to-path) processor to work.

#### <a name="processor-svg-include-only-groups">/svg/include-only-groups/</a>

Includes only certain groups from an SVG.

| Option | Type | Description |
|:-------|:-----|:------------|
|include-only-groups–groups|String|A list of group ID's to include, separated using the NUL syntax (%00)|

#### <a name="processor-svg-ungroup">/svg/ungroup/</a>

Ungroups all elements and nested SVG's recursively. If you require a higher depth than 10, you can make multiple calls.

| Option | Type | Description |
|:-------|:-----|:------------|
|ungroup-depth|Number|The depth to ungroup, 1 - 10. Default is 10.|
|ungroup-selector|Number|[XPath](https://www.w3schools.com/xml/xpath_syntax.asp) selector* to select certain groups or nested SVG's. If this is set, "depth" is ignored|

\* Please note that elements need to be prefixed with `svg:` to match the SVG namespace. Also remember to URL-encode your xpath, otherwise it may not work.

#### <a name="processor-svg-xpath">/svg/xpath/</a>

Returns a section of an SVG file using the [XPath](https://www.w3schools.com/xml/xpath_syntax.asp) syntax.

| Option | Type | Description |
|:-------|:-----|:------------|
|xpath-xpath|String|The XPath selector|
|xpath-text-output|Boolean|If true, the resulting file is a .txt, otherwise it is an .xml file|
|xpath-add-root|Boolean|Whether or not to add the root element|

#### <a name="processor-svg-xslt">/svg/xslt/</a>

Exclude certain elements with XSLT. This can be used to remove unwanted defs, specific elements and more.

| Option | Type | Description |
|:-------|:-----|:------------|
|xslt-exclude-xpath|String|The [XPath](https://www.w3schools.com/xml/xpath_syntax.asp) syntax selector to exclude from the document.|


## 📄 <a name="get-file">Get a file</a>

After getting the result you can retrieve your files here.

`GET https://vector.express/api/v2/public/files/filename.ext`



## 📨 <a name="using-existing-file">Using an existing file</a>

You can also use an already uploaded or resulting file through the `use-file` query string.

`POST https://vector.express/api/v2/public/convert/ext/prog1/ext/prog2?use-file=filename.ext`



## 🛑 <a name="limits">Limits</a>

Currently the public API is limited to 60 requests per hour, and a maximum output filesize of 10mb and certain CPU and memory limitations.



## 🔼 <a name="increase-limits">Increase limits?</a>

We offer a pay-as-you go plan that removes the rate limit and increases maximum file to 40 MB. Read more here: https://www.smidyo.com/vector-express



## 💡 <a name="feature-request">Feature request?</a>

Get in touch with us and we'll see what we can do!



## 🙊 <a name="feedback">Feedback</a>

Feel free to open up an issue, or just tweet at us @smidyo.

You can also shoot us a mail at frank@smidyo.com!
