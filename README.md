# Astro Homework

Homework 2 is in this other repo using Show your work (https://github.com/mtauraso/astr511-hw2)

This repository uses straight python notebooks and LaTeX, and should be runnable through vscode on windows

## Environment setup notes

`environment.yml` has the conda environment that is needed for these notebooks. I am using vscode's micromamba extension to create the environment into .micromamba at root (this directory is ignored by git)

Several packages are in environment.yml to allow vscode to render `%matplotlib widget` style plots. This is good when you need to draw a path around some points on a histogram and select things

## Practices I have learned and don't want to forget

### Have one Data table that lives on the filesystem
Download or file load it into memory. If you are downloading, save it to a location and load it later in the notebook. External databases are slow and variable in their performance. It is better to be able to decouple the waiting for an external resource from continuing to write code. TAP/TAP+ support async and login, so you can use one cell to submit a job, another cell to save the result to a file, and a third cell to load from the file. Data files should be kept in a data directory that is git ignored.

### Have one indexing scheme for all masks
Data masks (via `numpy.ma.masked_array`) should be indexed like your raw data always. Masks get combined with `numpy.logical_or()` which only takes two arguments. Masks are constructed by `numpy.logical_not(<selection condition>)`.

### Every plot uses the axis interface and gets a function that accepts a mask
The ideal interface to a plotting function looks like `plot_stuff(...data..., plotmask=None, ax=None)` All plotting commands within should work off the axis, and all data values should be masked inside the function with the plotmask and `numpy.ma.masked_array`.  Axis labels can go inside or outside the function, but one should default to putting them inside the function. The appropriate time to make a plot into a function is when you are tweaking the plot.

### Each cell that plots should use plt.subplot()
This practice makes the PDF output from the notebook clearer, and helps with the discipline of using the axis interface.

### You can create micromamba environments from the command pallete in vscode
Ctrl-Shift-P `Create environment`. You need to remove the old environment before creating a new one. The micromamba environment includes a package cache, so long as you do not delete it downloads of packages will be faster the second time. Every change in environment requires a VSCode reload. VSCode is good at picking the included micromamba environment.

### Interactive plots work from vscode
But you need ipympl, plotly, and ipywidgets in environment and its still touchy w.r.t. version numbers.

### Exporting ipynb to pdf also works
But you need nbconvert

### Dynamics packages like their C code
Gala/galpy both recommend pip install so they get to compile things. Gala doesn't support VS compiler, but a conda install still got it to use some C.
