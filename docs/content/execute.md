---
jupytext:
  cell_metadata_filter: -all
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: '0.8'
    jupytext_version: 1.4.2
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# Execute and cache your pages

Jupyter Book can automatically run and cache any notebook pages. Notebooks can either
be run each time the documentation is built, or cached locally so that notebooks
will only be re-run when the code cells in a notebook have changed.

Cacheing behavior is controlled with the `execute:` section
[in your `_config.yml` file](../customize/config). See
the sections below for each configuration option and its effect.

```{tip}
If you'd like to execute code that is in your markdown files, you can use the
`{code-cell}` directive in MyST markdown. See {doc}`../file-types/myst-notebooks`
for more information.
```

## Trigger notebook execution

By default, Jupyter Book will execute any content files that have a notebook structure,
and that are missing at least one output. This is equivalent to the following
configuration in _config.yml`:

```yaml
execute:
  execute_notebooks: auto
```

This will only execute notebooks that are missing at least one output. If
the notebook has *all* of its outputs populated, then it will not be executed.

**To force the execution of all notebooks, regardless of their outputs**, change the
above configuration value to:

```yaml
execute_notebooks: force
```

**To cache execution outputs with [jupyter-cache]**, change the above configuration
value to:

```yaml
execute:
  execute_notebooks: cache
```

See {ref}`execute/cache` for more information.

**To turn off notebook execution**,change the
above configuration value to:

```yaml
execute:
  execute_notebooks: 'off'
```

(execute/exclude)=
## Exclude files from execution

**To exclude certain file patterns from execution**, use the following
configuration:

```yaml
execute:
  exclude_patterns:
    - 'pattern1'
    - 'pattern2'
    - '*pattern3withwildcard'
```

Any file that matches one of the items in `exclude_patterns` will not be
executed.

(execute/cache)=
## Caching the notebook execution

You may also **cache the results of executing a notebook page** using [jupyter-cache]. In
this case, when a page is executed, its outputs will be stored in a local database.
This allows you to be sure that the outputs in your documentation are up-to-date,
while saving time avoiding unnecessary re-execution. It also allows you to store your
`.ipynb` files in your `git` repository *without their outputs*, but still leverage
a cache to save time when building your site.

When you re-build your site, the following will happen:

* Notebooks that have not seen changes to their **code cells** since the last build
  will not be re-executed. Instead, their outputs will be pulled from the cache
  and inserted into your site.
* Notebooks that **have any change to their code cells** will be re-executed, and the
  cache will be updated with the new outputs.

To enable caching of notebook outputs, use the following configuration:

```yaml
execute:
  execute_notebooks: cache
```

By default, the cache will be placed in the parent of your build folder. Generally,
this is in `_build/.jupyter_cache`.

You may also specify a path to the location of a jupyter cache you'd like to use:

```yaml
execute:
  cache: path/to/mycache
```

The path should point to an **empty folder**, or a folder where a
**jupyter cache already exists**.

[jupyter-cache]: https://github.com/executablebooks/jupyter-cache "the Jupyter Cache Project"

## Execution FAQs

### How does execution deal with relative paths in my code?

The behavior of relative paths in your code is slightly different depending on
whether you're executing with `auto` or with `cache`.

* **If executing with `auto`** notebooks will execute in the folder where they
  exist and relative paths will work.
* **If executing with `cache`** then the notebook will execute in a temporary
  folder, and relative paths will not work. Support for relative paths is planned
  for the future.

### How can I include code that raises errors?

In some cases, you may want to intentionally show code that doesn't work (e.g., to show
the error message). To do this, add a `raises-exception` tag to your code cell. This
can be done via a Jupyter interface, or via the `{code-cell}` directive like so:

````md
```{code-cell}
---
tags: [raises-exception]
---
print(thisvariabledoesntexist)
```
````

Which produces:

```{code-cell}
---
tags: [raises-exception]
---
print(thisvariabledoesntexist)
```

## Execution statistics

As notebooks are executed, certain statistics are stored in the environment by MyST-NB.
The simplest way to access and visualise this data is using the `nb-exec-table` directive.

:::{seealso}
The [MyST-NB documentation](myst-nb:execute/statistics), for creating you own directives to manipulate this.
:::

````md
```{nb-exec-table}
```
````

produces:

```{nb-exec-table}
```
