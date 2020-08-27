# Custom Sphinx configuration

Jupyter Book uses the excellent documentation tool [Sphinx](http://www.sphinx-doc.org/),
to build your book and manage citations, cross-references, and extensibility.

While Jupyter Book comes pre-configured with several Sphinx extensions,
power-users may wish to add their own extensions and configuration.
This page describes how to do so.

:::{warning}
Adding your own Sphinx configuration and extensions may cause Jupyter Book to behave
unpredictably. Use at your own risk!
:::

## Custom Sphinx extensions

To enable your own Sphinx extensions when building a Jupyter Book, use the following
configuration in your `_config.yml` file:

```yaml
sphinx:
  extra_extensions:
   - extension1
   - extension2
```

Any extensions that are listed will be appended to the list of Sphinx extensions at
build time.

:::{note}
Make sure that you have your extension installed on your machine, or Sphinx won't know
how to build the extensions.
:::

### An example: `sphinx-tabs`

For example, let's say you'd like to include **tabbed content** in your book.
There is [a sphinx-extension for that](https://github.com/djungelorm/sphinx-tabs).
To enable it, we'll do the following:

* **Install `sphinx-tabs`**. Here's the command to do so:

  ```bash
  pip install sphinx-tabs
  ```

* **Add `sphinx-tabs` content to your book**. Here's an example with MyST markdown:

  `````md
  ````{tabs}
  ```{tab} Line one

  Beautiful is better than ugly. ✨

  ```
  ```{tab} Line two

  Explicit is better than implicit. ❗
  ```
  ```{tab} Line three

  Simple is better than complex. 😵
  ```
  ````
  `````

* **Activate `sphinx-tabs` in `_config.yml`**. [The `sphinx-tabs` documentation](https://github.com/djungelorm/sphinx-tabs#installation)
  says we activate it in sphinx by adding `extensions = ["sphinx_tabs.tabs"]`, so we'll
  add it to our Jupyter Book like so:

  ```yaml
  sphinx:
    extra_extensions:
    - sphinx_tabs.tabs
  ```

Now, Jupyter Book will know how to interpret the `{tabs}` directive
(and any other directives that `sphinx-tabs` supports).

For example, here is a rendered version of the tab code pasted above:

The [Zen of Python](https://www.python.org/dev/peps/pep-0020/), in 3 tabs.

````{tabs}
```{tab} Line one

Beautiful is better than ugly. ✨

```
```{tab} Line two

Explicit is better than implicit. ❗
```
```{tab} Line three

Simple is better than complex. 😵
```
````

## Custom CSS or Javascript

If you'd like to include custom CSS rules or Javascript scripts in your book,
you can do so by adding them to a folder called `_static` in your book's folder.
Any files that end in `.css` or `.js` in this folder will automatically be copied
into your built book HTML and linked in the header of each page.

For example, to include a custom CSS file `myfile.css` in a Jupyter Book folder with
the following structure:

```
mybook/
├── _config.yml
├── _toc.yml
└── page1.md
```

Add the static file here:

```
├── _config.yml
├── _toc.yml
├── page1.md
└── _static
    └── myfile.css
```

The rules should then automatically be applied to your site. In general, these
CSS and JS files will be loaded *after* others are loaded on your page, so they
should overwrite pre-existing rules and behaviour.

## Manual sphinx configuration

You may also directly override the key-value pairs that Sphinx normally has
you configure in `conf.py`. To do so, use the following section of `_config.yml`:

  ```yaml
  sphinx:
    config:
      key1: value1
      key2: value2
  ```

### Fine control of parsing and execution

As discussed in [the components of Jupyter Book](intro/jupyter-book-components), two of the principe components of Jupyter Book are sphinx extensions;
MyST-Parser, for Markdown parsing, and MyST-NB, for notebook execution and output rendering.

These two extensions are highly customisable *via* Sphinx configuration.
Some their configuration is already exposed in the `_config.yml`, but you can also directly set configuration, see:

* The [MyST-Parser configuration options](myst-parser:intro/config-options)
* The [MyST-NB configuration options](myst-nb:start/config-options)

### Defining TeX Macros

You can add LaTeX macros for the whole book by defining them under the `Macros` option of the `TeX` block. For example, the following two macros have been pre-defined in the Sphinx configuration

```yaml
sphinx:
  config:
    mathjax_config:
      TeX:
        Macros:
          "N": "\\mathbb{N}"
          "floor": ["\\lfloor#1\\rfloor", 1]
          "bmat" : ["\\left[\\begin{array}"]
          "emat" : ["\\end{array}\\right]"]
```

You can also define TeX macros for a specific file by introducing them at the beginning of the file under a `math` directive. For example

````md
```{math}

\newcommand\N{\mathbb{N}}
\newcommand\floor[1]{\lfloor#1\rfloor}
\newcommand{\bmat}{\left[\begin{array}}
\newcommand{\emat}{\end{array}\right]}
```
````

The commands can be used inside a `math` directive, `$$` or inline `$`, for example:

```md
$$
A = \bmat{} 1 & 1 \\ 2 & 1\\ 3 & 2 \emat{},\ b=\bmat{} 2\\ 3 \\ 4\emat{},\ \gamma = 0.5
$$
```

will be rendered as:

$$
A = \bmat{} 1 & 1 \\ 2 & 1\\ 3 & 2 \emat{},\ b=\bmat{} 2\\ 3 \\ 4\emat{},\ \gamma = 0.5
$$

:::{seealso}
[How MyST-Parser works with MathJax](myst-parser:syntax/mathjax),
and the [Math and Equations](myst-content/math) section.
:::

:::{important}
To have "bare" LaTeX rendered in HTML, you must either set in your `_config.yaml`:

```yaml
parse:
  myst_extended_syntax: true
```

or more specifically:

```yaml
sphinx:
  config:
    myst_amsmath_enable: true
```

Then you can include:

```latex
\begin{equation}
  \int_0^\infty \frac{x^3}{e^x-1}\,dx = \frac{\pi^4}{15}
\end{equation}
```

\begin{equation}
  \int_0^\infty \frac{x^3}{e^x-1}\,dx = \frac{\pi^4}{15}
\end{equation}

:::
