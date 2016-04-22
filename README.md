## koi-fpp
False positive probabilities for all KOIs in the Q1-Q17 (DR24) Kepler table.
The one-stop-shop table for the summarized FPP results is [fpp_final_table.csv](https://raw.githubusercontent.com/timothydmorton/koi-fpp/master/fpp_final_table.csv).

If you're curious about individual KOIs and want to browse the diagnostic/result
plots, please go [here](http://phn14.astro.princeton.edu/koi-fpp).

If you're really curious, *all* the data files produced by these
 calculations are currently hosted at Princeton.  To browse the
 results from a given KOI, visit
 `tigress-web.princeton.edu/~tmorton/koi-fpp/K?????.??`, with the
 appropriate KOI identifier.  These directories contain the entire
 `vespa` and `isochrones` outputs for every KOI (including the stellar
 posteriors for the single-, binary- and triple-star fits).  If you
 have specific questions, please feel free to contact me.

Reproducing results
----------

You can run your own FPP calculations using all of the same
data and constraints that I used for this work.  If you wish to 
do this, first test out the calculation for the KOI for which I 
provide example data in this repository (KOI-7016.01/Kepler-452b).

First, clone this repository and hop on in:

    git clone https://github.com/timothydmorton/koi-fpp
    cd koi-fpp

Then define some temporary enivronment variables (assuming you're
calling this from inside this repository):

    export KOI_FPPDIR="example_data"
    export JROWE_DIR="example_data/photometry"

Then, set up the python environment using the `environment.yml` file in the top
level of this repository, as follows.  [If you do not have `conda` available, install [miniconda](http://conda.pydata.org/docs/install/quick.html) first.] 

    conda env create -f environment.yml
    source activate koifpp

This should install all the required packages.
Now from within this environment you can run the following: 

    koifpp-config K07016.01

This will set up the `vespa` and `isochrones` config files in a
`K07016.01` directory under `KOI_FPPDIR`.  It will also do the
trapezoid shape fitting to the photometry.  Take a look at the
`fpp.ini` and `star.ini` files to see what the inputs look like.  You
can then run the calculation as follows:

    cd $KOI_FPPDIR
    calcfpp K07016.01

Note that the stellar parameter inference (and thus the FPP
calculation) will be much more reliable if you have
`multinest`/`pymultinest` installed.  The calculations will run just
using the default `emcee` sampler, but I stronly recommend using
`multinest` becaues of the inherently multimodal nature of the
problem.  Follow [these
instructions](http://astrobetter.com/wiki/MultiNest+Installation+Notes)
to install.

In order to get the data necessary for all the KOI FPP calculations beyond 
this single example, you will need the following:

* [TRILEGAL starfield simulations](https://zenodo.org/record/108768/).  Define a `KOI_FPPDIR` environment variable to be somewhere sensible to you, and unpack this `starfield` directory within that.
* [Kepler photometry](https://zenodo.org/record/50147).  This is in two separate tarballs (Zenodo max file size is 2Gb), so unpack them both and combine their contents (all the koi*.n sub-directories) into a single directory.  Define a `JROWE_DIR` environment variable referring to this location.

Then you can run the `koifpp-config` and `calcfpp` commands for your favorite KOI(s).

Other Data
------

The `data` directory contains the following files:

* `fpp_all.txt`: concatenation of all the `results.txt` files from
all the successful `vespa` calculations. [TDM note: created by 
`summarize_fpp.py` script in `/tigress/tmorton/kepler`]
* `fpp_err.txt`: concatenation of all the exceptions raised by all
the failed `vespa` calculations. [TDM note: created by 
`summarize_fpp.py` script]
* `ttvdata.txt`: table of whether TTV information was used
to create the folded Kepler photometry used for the trapezoid fits.
[TDM note: created by `compile_ttv.py`]
* `starprops_all.txt`: summarized information from all the single-star
`isochrones` fits to all the KOI stars. [TDM note: created by `summarize_starprops.py`
after running `starfit-summarize` on all KOIs]
* `positional_probability.csv`: table of positional probability
data from Steve Bryson.  This should correspond to the "positional
probability" table at the NASA Exoplanet Archive.

Running the `make_finale_fpptable.py` script uses the data from these files
as well as the DR24 table to produce the `fpp_final_table.csv` file in the top
level of this repository.

The `fpp-results-analysis.ipynb` and `starprop-analysis.ipynb` notebooks
contain all the commands to make the figures & tables that live in
the `document` directory, with the exception of the Santerne+ (2015)
comparison table, which is created by `santerne_compare.ipynb`.

The manuscript PDF can be generated by entering the `document` directory
and running `make`.

