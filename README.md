# Sublime Text 4 Python Language Server setup

1. Create a virtual environment and copy its resulting path.
2. `pip install -U pip wheel`, upgrading pip is required because pyls has
conflicting version requirements.
3. `pip install -u requirements.txt`
4. Install Sublime Text 4, Package Manager, and LSP
5. Open LSP Settings and update it with the following:

```json
{
    "clients": {
        "pyls": {
            "enabled": true,
            "command": [
                "<PATH_TO_ENV>/bin/pyls",
                // Uncomment for debugging
                // "--log-file",
                // "/Users/yeray/code/personal/pyls/pyls.log",
                // "--verbose"
            ],
            "selector": "source.python",
            "settings": {
                "pyls.plugins.pycodestyle.enabled": false, // enabled by default, use flake8
                "pyls.plugins.flake8.enabled": true,  // flake8 is included in pyls
                "pyls.plugins.flake8.executable": "<PATH_TO_ENV>/bin/flake8", // Required in Mac OS X
                "pyls.configurationSources": [
                  "flake8",   // discovered in ~/.config/flake8, setup.cfg, tox.ini and flake8.cfg
                ],
                "pyls.plugins.jedi_rename.enabled": true,  // included in pyls, disabled by default
                // File formatter, invoke via LSP: Format file
                "pyls.plugins.autopep8.enabled": false,  // enabled by default, disabled to use black
                "pyls.plugins.black.enabled": true,  // from pyls-black
                // mypy
                "pyls.plugins.mypy_ls.enabled": true,  // from mypy-ls
            },
        },
    },
}
```

6. When working on a project you can also define settings:

```json
{
    "settings": {
        "LSP": {
            "pyls": {
                "enabled": true,
                "env": {
                    "PYTHONPATH": "<PATH_TO_PROJECT_ENV>/lib/python3.7/site-packages"
                },
                "settings": {
                    "pyls.plugins.flake8.executable": "<PATH_TO_PROJECT_ENV>/bin/flake8",
                }
            }
        }
    },
	"folders":
	[
		{
			"path": "/Users/yeray/code/personal/lunr.py"
		}
	]
}
```

7. Verify it works
	- Open a Python project, you should see flake8 and mypy errors automatically,
otherwise use `LSP: Toggle Diagnostics Panel`.
	- `LSP: Format File` will run Black.
	- `LSP: Rename` to rename a symbol.

## Troubleshooting

If things are not working as expected uncomment the extra arguments in the pyls
`command` settings, in a terminal `tail` the log to gain insight on the possible
problems. The server restarts on changes to the configuration, but sometimes you
may need to restart Sublime Text.

## Projects and docs

- http://lsp.sublimetext.io/language_servers/#pyls
- https://github.com/palantir/python-language-server 
- https://github.com/rupert/pyls-black
- https://github.com/Richardk2n/mypy-ls
