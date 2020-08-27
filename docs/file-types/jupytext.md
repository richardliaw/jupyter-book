(file-types:custom)=
# Custom notebook formats and Jupytext

You can designate additional file types to be converted to Notebooks, and then executed / parsed in the same manner as regular Notebooks.

```yaml
sphinx:
  config:
    nb_custom_formats:
        .mysuffix: mylibrary.converter_functio
```

- The string should be a Python function that will be loaded by `import mylibrary.converter_function`
- The function should take a file's contents (as a `str`) and return an [nbformat.NotebookNode](nbformat:api)

If the function takes additional keyword arguments, then you can specify these as dictionary in a second argument.
For example this is what the default conversion would look like:

```yaml
sphinx:
  config:
    nb_custom_formats:
        .ipynb:
            - nbformat.reads
            - as_version: 4
```

:::{important}

By default, Markdown cells in the Notebook will be parsed using the same MyST parser configuration as for other Markdown files.

But, if this is incompatible with your file format, then you can specify for the Markdown to be parsed as **strictly CommonMark**, using a third argument:

```yaml
sphinx:
  config:
    nb_custom_formats:
        .ipynb:
            - nbformat.reads
            - as_version: 4
            - true
```

:::

Finally, for text-based formats, MyST-NB also searches for an optional `source_map` key in the output Notebook's metadata.
This key should be a list mapping each cell to the starting line number in the original source file, for example for a notebook with three cells:

```json
{
  "metadata": {
    "source_map": [10, 21, 53]
  }
}
```

This mapping allows for "true" error reporting, as described in [](myst-nb:start/error-reporting).

## Using Jupytext

[Jupytext](https://jupytext.readthedocs.io/en/latest/) is an excellent Python tool for two-way conversion,
between Jupyter Notebook `.ipynb` files and
[a variety of text-based files](https://jupytext.readthedocs.io/en/latest/formats.html).

Jupyter Book natively supports the Jupytext file format: [notebooks with MyST Markdown](./myst-notebooks.md), but you can add other formats like [RMarkdown](https://rmarkdown.rstudio.com/) or Python files

The configuration looks like:

```yaml
sphinx:
  config:
    nb_custom_formats:
        .ipynb:
            - jupytext.read
            - fmt: Rmd
```

:::{important}
It is necessary to use `jupytext<=1.4.0,>=1.5.0`, due to an incompatibility with the `myst-parser` version in `jupytext=1.4`.
:::

(file-types:custom:jupytext)=
## Convert a Jupytext file into a MyST notebook

Alternatively If you'd like to convert your pre-existing Jupytext files into the MyST Notebook format,
to use directly in your book, install Jupytext and then run the following command:

```bash
jupytext --to myst path/to/yourfile
```

Note that you may also pass a wildcard that will be used to convert multiple
files. For example:

```bash
jupytext --to myst ./*.py
```

See [the Jupytext CLI documentation](https://jupytext.readthedocs.io/en/latest/using-cli.html) for more information.
