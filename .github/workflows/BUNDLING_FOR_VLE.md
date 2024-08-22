# BUNDLING FOR OU VLE

Things to note when building for OU VLE / HTML5.zip:

- filenames only allow [a-zA-Z-_]; spaces in filenames throw an error;
- the `index.html` pages in `lab/`, `tree/` and `notebooks/` need updating: `<link rel="manifest" href="/app.webmanifest" /> ` needs to be set to `<link rel="manifest" href="/app.webmanifest" crossorigin="use-credentials" />` to handle the auth;
- delete the `book/` directory from the bundle if it's the complete Github pages bundle;
- to create the zip, unzip and untar the asset uploaded by the action, `cd` into it and then (after any mods e.g. to `index.html` files), zip  the directory as root into a `.zop` archive file, e.g. `zip -r ../jupyterlite-dist-tm112.zip .`
