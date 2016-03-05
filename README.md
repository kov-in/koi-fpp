# koi-fpp
False positive probabilities for all KOIs in the Q1-Q17 (DR24) Kepler table.

This is work in progress; caveat emptor until the paper is accepted; proper citation/attribution info will be posted here when available.

The `data` folder contains the following files:

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
`isochrones` fits to all the KOI stars. [TDM note: created by `summarize_starprops.py`]
* `positional_probability.csv`: table of positional probability
data from Steve Bryson.  This should correspond to the "positional
probability" table at the NASA Exoplanet Archive.

Running the `make_finale_fpptable.py` script uses the data from these files
as well as the DR24 table to produce the `fpp_final_table.csv` file in the top
level of this repository.

