# OU TM112 Jupyter Notebook Activities

Notebooks and resources for the Open University Course [TM112 Introduction to computing and information technology 2](http://www.open.ac.uk/courses/modules/tm112) Block 2 Week 6 geolocation activities.

Notebooks are provided for the following activities:

- Activity 6.16 - Cell Tower Lookup.ipynb
- Activity 6.20 - Locating Nearby Wifi Hotspots.ipynb
- Activity 6.22 - 6.25 Geocoding.ipynb
  - Activity 6.22 - Geocoding an address using an API
  - Activity 6.23 - Geocoding addresses and postcodes
  - Activity 6.24 - Geocoding an IP address
  - Activity 6.25 - Reverse geocoding a postcode
- Activity 6.27 - Creating annotated interactive maps with Python
- Activity 6.29 - Modelling uncertainty in simple trilateration.ipynb

You should be able to find the notebooks in either the `notebooks` folder or the `work` folder depending on which notebook server you are running.

You can run and inspect these notebooks in various ways.

For TM112 students, the recommended option is the Open Compute Lab - if you arrived here via the Open Compute Lab, you can now go to the work folder to run the notebooks. If you have not used a Jupyter notebook before, we recommend you start with Using Jupyter Notebooks.ipynb

Alternatively, you can run live interactive notebook activities inside your browser using JupyterLite. Access the JupyterLite materials here: [`ouseful-demos/jupyterlite-tm112-demo`](https://ouseful-demos.github.io/jupyterlite-tm112-demo/lab/index.html)

__When running the notebooks in JupyterLite, use the `Python 3.11 XPython` kernel for all module notebooks *except* the *Activity 6.29 - Modelling uncertainty* notebook which MUST use the *Pyodide* kernel.__

A non-executable, non-interactive, static (read-only) HTML preview of the materials here: https://ouseful-demos.github.io/jupyterlite-tm112-demo/book_output

## Localization Package

The `localization` package has been rebuilt and pushed to PyPi as `localization-patched`. It will run in the `pyodide` kernel, but does not currently run in Xeus-Python due to a missing `shapely` related dependency.

The `localization-patched` package is built from the package source in the [`ouseful-demos/jupyterlite-tm112-demo`](https://github.com/ouseful-demos/jupyterlite-tm112-demo) GitHub repo.
