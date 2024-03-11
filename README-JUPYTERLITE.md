# README — JupyterLite

JupyterLite is a Jupyter enviroment that runs purely in the browser using WASM based kernels.

Once the environment has loaded, notebooks will run in the browser without the need for a network connection or an external Jupyter server. *A network connection __will__ be required if the notebook code makes network requests, of course.*

Edits and updates to notebook files are persisted using browser storage, although there are a few quirks in storage behaviour:

- if you *create* a new notebook or file and save it, it will be saved to browser storage; if you close the borwser and then revisit the same JupyterLite site / URL, the new or modified files will be available to you. If you then delete the notebook, it will be deleted from notebook storage;

- if you modify a notebook or file that was shipped as part of the original JupyterLite distribution and save it, the modified file is saved to browser storage. Quitting and revisiting the site will once again show you the modified file. However, if you delete the file, you're modified file is deleted and you will be re-presented with the original file, as originally distributed;

- if the contents of the original distribution are updated (that is, the files shipped with the JupyterLite repository), delete all your modified files (or clear the browser storage), open browser dev tools and do a hard/forced reload of the site to refresh the cached, distriibuted files.

## Using `requests` etc.

When running notebooks using JupyterLite, code that exectues network requests runs according the browser security sandbox policies. If a webserver has not been configured to accept requests from *any* origin, `*`,(that is, cross-origin requests), then requests made from a notebook using `requests` may fail.

In such a case, you may be able to make a request via a proxy with the appropriate header settings.

In the TM112 demo notebooks, the free `corsproxy.io` service is used to proxy certain HTTP GET requests using the following wrapper:

```python
import requests
from urllib.parse import quote

def cors_proxy(url):
    """CORS proxy for GET resources."""
    _url = f"https://corsproxy.io/?{quote(url)}"
    return requests.get(_url).content.decode().strip()

response = cors_proxy(URL)
```

## Available Kernels

The TM112 demo JupyterLite environment ships two kernels:

- pyodide
- Xeus XPython 3.11

The pyodide kernel is installed via the [`requirments.txt`](https://github.com/ouseful-demos/jupyterlite-tm112-demo/blob/main/requirements.txt) file in the repository root directory. The `jupyterlite-xeus` requirement loads custom Xeus kernels as specified in the `environment.yml` file.

The `pyodide` kernel uses the [`pyodide.org`](https://pyodide.org/en/stable/) WASM Python distribution. Packages that are not part of the base distribution may be installed from PyPi using `%pip` magic. A wide variety of packages are available as part of the pyodide environment, including packages such as `shapely` that do not have a `py3-none-any` build on PyPi available. Packages available in pyodide are listed [here](https://pyodide.org/en/stable/usage/packages-in-pyodide.html).

The Xeus XPython environment provides a custom Python environment with predefined packages installed as specified in an [`environment.yml`](https://github.com/ouseful-demos/jupyterlite-tm112-demo/blob/main/environment.yml) file in the repository root directory. Packages specified in the main part of that config file are installed, along with their dependencies, from a conda source. Packages installed from PyPi by the `pip` context *do not* have their dependencies installed. Only packages with a `py3-none-any` build can be installed via the `pip` context. Packages cannot be installed from notebooks; if they are required, they must be installed into the XPython environment at the JupyterLite installation build time.

### Other available kernels

Several other kernels are also available for Jupyterlite. The Xeus framework includes support for the installation of Javascript, SQLite, P5 (Processing) and Lua kernels. The wider Xeus initiative has support for an R kernel, but this is not yet available for JupyterLite. However, the experimental [`WebR`](https://github.com/r-wasm/jupyterlite-webr-kernel) kernel does provide an R kernel for JupyterLite.
