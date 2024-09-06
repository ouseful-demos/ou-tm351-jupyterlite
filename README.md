# OU TM351 JupyteLite Demo

A proof of concept in-browser JupyterLite site offering partial support for the Open University module [TM351 Data Management and Analysis](http://www.open.ac.uk/courses/modules/tm351).

*JupyterLite is a Jupyter environment that runs purely in the browser using WebAssembly ([WASM]( https://en.wikipedia.org/wiki/WebAssembly)) based kernels.*

This environment can be made available as an `HTML5.zip` resource uploaded to the OU VLE ([*JupyterLite in OU VLE* docs](https://opencomputinglab.github.io/jupyterlite_in_moodle_vle/)).

Once the environment has loaded, notebooks will run in the browser without the need for a network connection or an external Jupyter server. *A network connection __will__ be required if the notebook code makes network requests, of course.*

Two styles of user interface are available:

- [document centric "Jupyter notebok" style UI](https://ouseful-demos.github.io/jupyterlite-tm112-demo)
- [IDE style JupyterLab environment](https://ouseful-demos.github.io/jupyterlite-tm112-demo/lab)

Direct links to the exectuable JupyterLite and static pre-run HTML book versions of the notebooks:

-  JupyterLab opening to [`tm351-examples.ipynb` notebook](https://ouseful-demos.github.io/ou-tm351-jupyterlite/lab/index.html?path=tm351_examples.ipynb);

- notebook home page](https://ouseful-demos.github.io/ou-tm351-jupyterlite/tree/index.html);

- notebook UI opening to  [`tm351-examples.ipynb` notebook](https://ouseful-demos.github.io/ou-tm351-jupyterlite/tree/index.html?path=tm351_examples.ipynb).

Edits and updates to notebook files are persisted using browser storage, although there are a few quirks in storage behaviour:

- if you *create* a new notebook or file and save it, it will be saved to browser storage; if you close the browser and then revisit the same JupyterLite site / URL, the new or modified files will be available to you. If you then delete the notebook, it will be deleted from notebook storage;

- if you modify a notebook or file that was shipped as part of the original JupyterLite distribution and save it, the modified file is saved to browser storage. Quitting and revisiting the site will once again show you the modified file. However, if you delete the file, your modified file is deleted and you will be re-presented with the original file, as originally distributed;

- if the contents of the original distribution are updated (that is, the files shipped with the JupyterLite repository), delete all your modified files (or clear the browser storage), open browser dev tools and do a hard/forced reload of the site to refresh the cached, distributed files.

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

The TM351 demo JupyterLite environment ships a single Python kernel:

- Xeus XPython 3.11
- Pyodide

The Xeus XPython environment provides a custom Python environment with predefined packages installed as specified in an [`environment.yml`](https://github.com/ouseful-demos/jupyterlite-tm112-demo/blob/main/environment.yml) file in the repository root directory. Packages specified in the main part of that config file are installed, along with their dependencies, from a conda source. Packages installed from PyPi by the `pip` context *do not* have their dependencies installed. Only packages with a `py3-none-any` build can be installed via the `pip` context. Packages cannot be installed from notebooks; if they are required, they must be installed into the XPython environment at the JupyterLite installation build time.

The Pyodide kernel provides a [`pyodide` (Python WASM)](https://pyodide.org/en/stable/) environment. Universal Python packages (that is, ones that run on `any` platform) can be installed into this kernel using `%pip install PACKAGENAME` cell magic.

The experimental [`WebR`](https://github.com/r-wasm/jupyterlite-webr-kernel) kernel is also undergoing brook of concept testing for M348 [here](https://ouseful-demos.github.io/jupyterlite-m348-demo/) [[repo](https://github.com/ouseful-demos/jupyterlite-m348-demo)].


### Other available kernels

Several other kernels are also available for Jupyterlite, including:

- the Xeus framework includes support for the installation of Javascript, SQLite, P5 (Processing) and Lua kernels. The wider Xeus initiative has support for an R kernel, but this is not yet available for JupyterLite.
