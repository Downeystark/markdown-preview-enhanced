[Back](/docs/advanced-export.md)

<!-- toc orderedList:0 -->

- [PDF Document](#pdf-document)
	- [Overview](#overview)
	- [Export Path](#export-path)
	- [Table of Contents](#table-of-contents)
	- [Syntax Highlighting](#syntax-highlighting)
	- [LaTeX Options](#latex-options)
		- [LaTeX Packages for Citations](#latex-packages-for-citations)
	- [Advanced Customization](#advanced-customization)
		- [LaTeX Engine](#latex-engine)
		- [Include](#include)
		- [Custom Templates](#custom-templates)
		- [Pandoc Arguments](#pandoc-arguments)
	- [Shared Options](#shared-options)

<!-- tocstop -->

# PDF Document
## Overview
To create a PDF document, you need to specify `pdf_document` output format in the front-matter of your document:  
```yaml
---
title: "Habits"
author: John Doe
date: March 22, 2005
output: pdf_document
---
```
## Export Path  
You can define the document export path by specifying `path` option. For example:    

```yaml
---
title: "Habits"
output:
  pdf_document:
    path: /Exports/Habits.pdf
---
```   
If `path` is not defined, then document will be generated under the same directory.

## Table of Contents
You can add a table of contents using the `toc` option and specify the depth of headers that it applies to using the `toc_depth` option. For example:  
```yaml
---
title: "Habits"
output:
  pdf_document:
    toc: true
    toc_depth: 2
---
```
If the table of contents depth isn’t explicitly specified then it defaults to 3 (meaning that all level 1, 2, and 3 headers will be included in the table of contents).   

*Attention:* this TOC is different from the `<!-- toc -->` generated by **Markdown Preview Enhanced**.  

You can add section numbering to headers using the `number_sections` option:
```yaml
---
title: "Habits"
output:
  pdf_document:
    toc: true
    number_sections: true
---
```

## Syntax Highlighting
The `highlight` option specifies the syntax highlighting style. Supported styles include “default”, “tango”, “pygments”, “kate”, “monochrome”, “espresso”, “zenburn”, and “haddock” (specify null to prevent syntax highlighting):    

For example:  
```yaml
---
title: "Habits"
output:
  pdf_document:
    highlight: tango
---
```
## LaTeX Options
Many aspects of the LaTeX template used to create PDF documents can be customized using top-level YAML metadata (note that these options do not appear underneath the `output` section but rather appear at the top level along with title, author, etc.). For example:    
```yaml
---
title: "Crop Analysis Q3 2013"
output: pdf_document
fontsize: 11pt
geometry: margin=1in
---
```
Available metadata variables include:   

| Variable  | Description  |
|---|---|
| papersize | paper size, e.g. `letter`, `A4` |
| lang  | Document language code |
| fontsize | Font size (e.g. 10pt, 11pt, 12pt) |
| documentclass | LaTeX document class (e.g. article) |
| classoption | Option for documentclass (e.g. oneside); may be repeated |
| geometry | Options for geometry class (e.g. margin=1in); may be repeated |
| linkcolor, urlcolor, citecolor	|Color for internal, external, and citation links (red, green, magenta, cyan, blue, black) |
| thanks | specifies contents of acknowledgments footnote after document title. |  

More available variables can be found [here](http://pandoc.org/MANUAL.html#variables-for-latex).

### LaTeX Packages for Citations
By default, citations are processed through `pandoc-citeproc`, which works for all output formats. For PDF output, sometimes it is better to use LaTeX packages to process citations, such as `natbib` or `biblatex`. To use one of these packages, just set the option `citation_package` to be `natbib` or `biblatex`, e.g.  
```yaml
---
output:
  pdf_document:
    citation_package: natbib
---
```

## Advanced Customization
### LaTeX Engine  
By default PDF documents are rendered using `pdflatex`. You can specify an alternate engine using the `latex_engine` option. Available engines are “pdflatex”, “xelatex”, and “lualatex”. For example:  
```yaml
---
title: "Habits"
output:
  pdf_document:
    latex_engine: xelatex
---
```

### Include
You can do more advanced customization of PDF output by including additional LaTeX directives and/or content or by replacing the core pandoc template entirely. To include content in the document header or before/after the document body you use the `includes` option as follows:  
```yaml
---
title: "Habits"
output:
  pdf_document:
    includes:
      in_header: header.tex
      before_body: doc_prefix.tex
      after_body: doc_suffix.tex
---
```

### Custom Templates
You can also replace the underlying pandoc template using the `template` option:
```yaml
---
title: "Habits"
output:
  pdf_document:
    template: quarterly_report.tex
---
```
Consult the documentation on [pandoc templates](http://pandoc.org/README.html#templates) for additional details on templates. You can also study the [default LaTeX template](https://github.com/jgm/pandoc-templates/blob/master/default.latex) as an example.

### Pandoc Arguments   
If there are pandoc features you want to use that lack equivalents in the YAML options described above you can still use them by passing custom `pandoc_args`. For example:  
```yaml
---
title: "Habits"
output:
  pdf_document:
    pandoc_args: [
      "--no-tex-ligatures"
    ]
---
```

## Shared Options
If you want to specify a set of default options to be shared by multiple documents within a directory you can include a file named `_output.yaml` within the directory. Note that no YAML delimeters or enclosing output object are used in this file. For example:    

**_output.yaml**
```yaml
pdf_document:
  toc: true
  highlight: zenburn
```
All documents located in the same directory as `_output.yaml` will inherit it’s options. Options defined explicitly within documents will override those specified in the shared options file.