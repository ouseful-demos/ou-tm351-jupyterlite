# README — JupyterLite

JupyterLite is a Jupyter environment that runs purely in the browser using WebAssembly ([WASM]( https://en.wikipedia.org/wiki/WebAssembly)) based kernels.

Once the environment has loaded, notebooks will run in the browser without the need for a network connection or an external Jupyter server. *A network connection __will__ be required if the notebook code makes network requests, of course.*

Two styles of user interface are available:

- [document centric "Jupyter notebok" style UI](https://ouseful-demos.github.io/jupyterlite-tm112-demo/tree)
- [IDE style JupyterLab environment](https://ouseful-demos.github.io/jupyterlite-tm112-demo/lab)

Direct links to the exectuable JupyterLite and static pre-run HTML book versions of the notebooks:

- Activity 6.16 - Cell Tower Lookup.ipynb (JupyterLite [lab](https://ouseful-demos.github.io/jupyterlite-tm112-demo/lab/?path=work%2FActivity+6.16+-+Cell+Tower+Lookup.ipynb), [nb](https://ouseful-demos.github.io/jupyterlite-tm112-demo/notebooks/?path=work%2FActivity+6.16+-+Cell+Tower+Lookup.ipynb); [html](https://ouseful-demos.github.io/jupyterlite-tm112-demo/book_output/Activity%206.16%20-%20Cell%20Tower%20Lookup.html))
- Activity 6.20 - Locating Nearby Wifi Hotspots.ipynb (JupyterLite [lab](https://ouseful-demos.github.io/jupyterlite-tm112-demo/lab/?path=work%2FActivity+6.20+-+Locating+Nearby+Wifi+Hotspots.ipynb), [nb](https://ouseful-demos.github.io/jupyterlite-tm112-demo/notebooks/?path=work%2FActivity+6.20+-+Locating+Nearby+Wifi+Hotspots.ipynb); [html](https://ouseful-demos.github.io/jupyterlite-tm112-demo/book_output/Activity%206.20%20-%20Locating%20Nearby%20Wifi%20Hotspots.html))
- Activity 6.22 - 6.25 Geocoding.ipynb (JupyterLite [lab](https://ouseful-demos.github.io/jupyterlite-tm112-demo/lab/?path=work%2FActivity+6.22+-+6.25+-+Geocoding.ipynb), [nb](https://ouseful-demos.github.io/jupyterlite-tm112-demo/notebooks/?path=work%2FActivity+6.22+-+6.25+-+Geocoding.ipynb); [html](https://ouseful-demos.github.io/jupyterlite-tm112-demo/book_output/Activity%206.22%20-%206.25%20-%20Geocoding.html))
  - Activity 6.22 - Geocoding an address using an API
  - Activity 6.23 - Geocoding addresses and postcodes
  - Activity 6.24 - Geocoding an IP address
  - Activity 6.25 - Reverse geocoding a postcode
- Activity 6.27 - Creating annotated interactive maps with Python (JupyterLite [lab](https://ouseful-demos.github.io/jupyterlite-tm112-demo/lab/?path=work%2FActivity+6.27+-+Creating+annotated+interactive+maps+with+Python.ipynb), [nb](https://ouseful-demos.github.io/jupyterlite-tm112-demo/notebooks/?path=work%2FActivity+6.27+-+Creating+annotated+interactive+maps+with+Python.ipynb); [html](https://ouseful-demos.github.io/jupyterlite-tm112-demo/book_output/Activity%206.27%20-%20Creating%20annotated%20interactive%20maps%20with%20Python.html))
- Activity 6.29 - Modelling uncertainty in simple trilateration.ipynb (JupyterLite [lab](https://ouseful-demos.github.io/jupyterlite-tm112-demo/lab/?path=work%2FActivity+6.29+-+Modelling+uncertainty+in+simple+trilateration.ipynb), [nb](https://ouseful-demos.github.io/jupyterlite-tm112-demo/notebooks/?path=work%2FActivity+6.29+-+Modelling+uncertainty+in+simple+trilateration.ipynb); [html](https://ouseful-demos.github.io/jupyterlite-tm112-demo/book_output/Activity%206.29%20-%20Modelling%20uncertainty%20in%20simple%20trilateration.html))

Edits and updates to notebook files are persisted using browser storage, although there are a few quirks in storage behaviour:

- if you *create* a new notebook or file and save it, it will be saved to browser storage; if you close the browser and then revisit the same JupyterLite site / URL, the new or modified files will be available to you. If you then delete the notebook, it will be deleted from notebook storage;

- if you modify a notebook or file that was shipped as part of the original JupyterLite distribution and save it, the modified file is saved to browser storage. Quitting and revisiting the site will once again show you the modified file. However, if you delete the file, your modified file is deleted and you will be re-presented with the original file, as originally distributed;

- if the contents of the original distribution are updated (that is, the files shipped with the JupyterLite repository), delete all your modified files (or clear the browser storage), open browser dev tools and do a hard/forced reload of the site to refresh the cached, distriibuted files.

## Using `requests` etc.

When running notebooks using JupyterLite, code that executes network requests runs according the browser security sandbox policies. If a webserver has not been configured to accept requests from *any* origin, `*`,(that is, cross-origin requests), then requests made from a notebook using `requests` may fail.

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
