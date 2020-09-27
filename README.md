<img alt="Logo" src="logo.png" width=300>

# Vector Express - Free Vector Converting API
### Update September 2020 - Version 2.0!

Visual interface:
https://vector.express

Vector Express 2.0 is a free website and API for converting vector files. Made with love by [@Smidyo](https://twitter.com/smidyo)

It runs a combination of different conversion programs that can be chained together to convert between a wide area of formats.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     | ✓  | ✓   | ✓   | ✓   | ✓   | ✓    | ✓   | ✓   | ✓  | ✓   |
| Out    |    |     |     | ✓   | ✓   |      | ✓   | ✓   | ✓  | ✓   |


Feel free to use it in your project. It does not support CORS, so you need to run it through/on a back-end.


## 🏃‍♀️ Quickstart

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/890e639ae3094e3617c0)

1. GET the conversion `path` from your source format to the target form

`curl https://vector.express/api/v2/public/convert/dxf/auto/svg/`

2. POST to the the first path, with your file as the body

`curl --data @myvector.dxf https://vector.express/api/v2/public/convert/dxf/cadlib/svg/`

3. GET the file from the `resultUrl`

`curl https://vector.express/api/v2/public/files/[id].svg --output converted.svg`


## 🔗 API

### Get possible conversion paths between formats

By requesting the conversion paths you can find out the most efficient path between your formats.

`GET https://vector.express/api/v2/public/convert/ext/auto/ext`


###  Convert a file

You can up chain to three programs in the conversion path, and even configure them. See below for all programs and their options.

`POST https://vector.express/api/v2/public/convert/ext/prog1/ext/prog2/ext?prog1-opt=val&prog2-opt=val`


### Get a file

`GET https://vector.express/api/v2/public/files/filename.ext`


### Convert using an existing file 

You can also convert using an already uploaded or converted file

`POST https://vector.express/api/v2/public/convert/ext/prog1/ext/prog2?use-file=filename.ext`


## ⚙️ Supported programs

### /cadlib/

Custom CadLib based converter for AutoCAD files.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    |     | ✓   | ✓   |     |      |     |     |    |     |
| Out    |    |     |     | ✓   |     |      |     |     |    | *1  |

*1 Arcs and curves are converted to lines

| Option | Type | Description |
|:-------|:-----|:------------|
|epsilon|Number||
|arc-segments|Number|Line segment amount on arcs|


### /gs/

[Ghostscript](https://www.ghostscript.com/) based converter.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     | ✓  |     |     |     | ✓   |      | ✓   |     | ✓  |     |
| Out    |    |     |     |     | ✓   |      | ✓   |     | ✓  |     |


### /hp2xx/

[hp2xx](https://www.gnu.org/software/hp2xx/) based converter for postscript.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    |     |     |     |     | ✓    | ✓   |     |    |     |
| Out    |    |     |     |     | ✓   |      |     |     |    | ✓   |


### /libcdr/

[libcdr](https://wiki.documentfoundation.org/DLP/Libraries/libcdr) based converter for CorelDRAW files.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    | ✓   |     |     |     |      |     |     |    |     |
| Out    |    |     |     |     |     |      |     |     |    | ✓   |


### /librsvg/

[librsvg](https://github.com/GNOME/librsvg) based converter for converting SVG files to various formats.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    |     |     |     |     |      |     |     |    | ✓   |
| Out    |    |     |     |     | ✓   |      | ✓   |     | ✓  |     |


### /pdf2svg/

[pdf2svg](https://github.com/dawbarton/pdf2svg) based converter for PDF files to SVG.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    |     |     |     |     |      | ✓   |     |    |     |
| Out    |    |     |     |     |     |      |     |     |    | ✓   |

| Option | Type | Description |
|:-------|:-----|:------------|
|page|Number|Which page to export|


### /pstoedit/

[pstoedit](http://www.calvina.de/pstoedit/) for converting EPS files to DXF.

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


### /svgo/

[SVGO](https://github.com/svg/svgo) is an SVG optimizer.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    |     |     |     |     |      |     |     |    | ✓   |
| Out    |    |     |     |     |     |      |     |     |    | ✓   |

| Option | Type | Description |
|:-------|:----:|:-----------:|
|enable|String|Which plugins to enable (see SVGO docs)|
|disable|String|Which plugins to disable (see SVGO docs)|
|indent|String||
|pretty|Boolean|Prettify code|
|precision|Number|Precision|
|multipass|Boolean||


### /uniconvertor/

[Uniconvertor](https://sk1project.net/uc2/) based converter for various formats.

| Format | ai | cdr | dwg | dxf | eps | hpgl | pdf | plt | ps | svg |
|:-------|:--:|:---:|:---:|:---:|:---:|:----:|:---:|:---:|:--:|:---:|
| In     |    |     |     |     | ✓   |      |     | ✓   |    | ✓   |
| Out    |    |     |     |     |     |      | ✓   | ✓   |    | ✓   |


## 🛑 Limits

Currently the public API is limited to 60 requests per hour, and a maximum output filesize of 10mb and certain CPU and memory limitations.


## Increase limits?

Get in touch with us and we'll see what we can do!


## 🙊 Feedback

Feel free to open up an issue, or just tweet at us @smidyo.

You can also shoot us a mail at frank@smidyo.com!
