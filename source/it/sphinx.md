# Sphinx

## Sphinx Documentation
- Sphinx: <https://www.sphinx-doc.org/>
- reStructuredText (reST): <https://www.sphinx-doc.org/en/master/usage/restructuredtext/index.html>
- Configuration: <https://www.sphinx-doc.org/en/master/usage/configuration.html>
- Table of Contents: <https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#table-of-contents>

## Extensions and Themes
- ABlog: <https://ablog.readthedocs.io/>
- MyST - Markedly Structured Text: <https://myst-parser.readthedocs.io/>
- MyST-NB: <https://myst-nb.readthedocs.io/>
- recommonmark: <https://recommonmark.readthedocs.io/>
- Napoleon: <https://sphinxcontrib-napoleon.readthedocs.io/>
- Read the Docs Sphinx Theme: <https://sphinx-rtd-theme.readthedocs.io/>
- PyData Sphinx Theme: <https://pydata-sphinx-theme.readthedocs.io/>

## eniak.de Installation
- create repo on GitHub and clone it
- chane into the repo directory
- run `sphinx-quickstart` - say yes here:
```text
You have two options for placing the build directory for Sphinx output.
Either, you use a directory "_build" within the root path, or you separate
"source" and "build" directories within the root path.
> Separate source and build directories (y/n) [n]:

```

- delete make.bat - we do not work with windows
- config git with `git config pull.rebase true`
- install https://sphinx-rtd-theme.readthedocs.io/en/latest/
- install https://myst-nb.readthedocs.io/en/latest/
  - add it to extensions
  - turn off notebook building: `jupyter_execute_notebooks = "off"`
- turn off `prev_next_buttons`:
```python
html_theme_options = {
    "prev_next_buttons_location": None,
}
```