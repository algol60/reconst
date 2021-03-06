# Running the HTML to ReST converter

The HTML to ReST converter is run once to convert the HTML files in NetBeans helpsets to ReStructuredText files suitable for input to [Sphinx](https://www.sphinx-doc.org/).

The subset of HTML that is converted is fairly simple, but sufficient to copy text and images that can be tidied up later if necessary. It is not meant to perfect, just good enough.

The converter works by looking in a directory tree for package-info.java files containing lines that start with `@HelpSetRegistration`. The helpset definition file is pasrsed out, then the helpset XML files are parsed. The `tocitem` trees in the `-toc.xml` files are used to build a directory structure containing the resulting `.rst` files, and the `-map.xml` files are use to map helpIds to HTML files, which are converted to ReST and created in the right places in the new directory tree.

You'll need `make_rest.py` and `parsehelp.py`.

The `make_rest.py` script requires two parameters:

* --indir: the root of the Constellation source. This is where it starts looking for subdirectories containing `package-info.java` files.
* --outdir" an existing (preferably empty) directory to output the converted `.rst` files to.

Optional parameter:

* --index: specify this to create `index.rst` table-of-contents files for each directory level.

Constellation is designed to be modular: the Core provides the base code, then extra modules can be added. For example, a biology-specific or social-networking-specific set of modules can be layered in. These can provide their own set of help files.

At help build time, the help files are copied into a single directory tree. Generally, Core will have `index.rst` files which contain a `toctree::` directive. Other layers should not contain `index.rst` files, otherwise they'll overwrite Core (unless this is what you want, of course). Other layers can contain `index.rst` files, as long as they are in different places to Core's `index.rst` files.

For example:

```
python .\make_rest.py --indir D:\github\constellation\ --outdir D:\tmp\sphinx-out\
```

The `D:\tmp\sphinx-out` directory will contain the output files, including any images that were referenced from HTML.

When you're sure that the `.rst` files are suitable, you can delete the HTML documentation, the helpset `.xml` files and the `@HelpSetRegistration` annotations.
