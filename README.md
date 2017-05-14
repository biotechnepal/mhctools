[![Build Status](https://travis-ci.org/hammerlab/mhctools.svg?branch=master)](https://travis-ci.org/hammerlab/mhctools) [![Coverage Status](https://coveralls.io/repos/hammerlab/mhctools/badge.svg?branch=master)](https://coveralls.io/r/hammerlab/mhctools?branch=master) [![DOI](https://zenodo.org/badge/18834/hammerlab/mhctools.svg)](https://zenodo.org/badge/latestdoi/18834/hammerlab/mhctools)

# mhctools
Python interface to running command-line and web-based MHC binding predictors.

## Example

```python
from mhctools import NetMHCpan
# Run NetMHCpan for alleles HLA-A*01:01 and HLA-A*02:01
predictor = NetMHCpan(alleles=["A*02:01", "hla-a0101"])

# scan the short proteins 1L2Y and 1L3Y for epitopes
protein_sequences = {
  "1L2Y": "NLYIQWLKDGGPSSGRPPPS",
  "1L3Y": "ECDTINCERYNGQVCGGPGRGLCFCGKCRCHPGFEGSACQA"
}

binding_predictions = predictor.predict_subsequences(protein_sequences, peptide_lengths=[9])

# flatten binding predictions into a Pandas DataFrame
df = binding_predictions.to_dataframe()

# epitope collection is sorted by percentile rank
# of binding predictions
for binding_prediction in binding_predictions:
  if binding_prediction.affinity < 100:
    print("Strong binder: %s" % (binding_prediction,))
```
## API

The following MHC binding predictors are available in `mhctools`:
* `NetMHC3`: requires locally installed version of [NetMHC 3.x](http://www.cbs.dtu.dk/services/NetMHC-3.4/)
* `NetMHC4`: requires locally installed version of [NetMHC 4.x](http://www.cbs.dtu.dk/services/NetMHC/)
* `NetMHC`: a wrapper function to automatically use `NetMHC3` or `NetMHC4` depending on what's installed.
* `NetMHCpan`: requires locally installed version of [NetMHCpan](http://www.cbs.dtu.dk/services/NetMHCpan/)
* `NetMHCIIpan`: requires locally installed version of [NetMHCIIpan](http://www.cbs.dtu.dk/services/NetMHCIIpan/)
* `NetMHCcons`: requires locally installed version of [NetMHCcons](http://www.cbs.dtu.dk/services/NetMHCcons/)
* `IedbMhcClass1`: Uses IEDB's REST API for class I binding predictions.
* `IedbMhcClass2`: Uses IEDB's REST API for class II binding predictions.
* `RandomBindingPredictor`: Creates binding predictions with random IC50 and percentile rank values.

Every binding predictor is constructed with an `alleles` argument specifying the HLA type for which to make predictions. Predictions are generated by calling the `predict` method with a dictionary mapping sequence IDs or names to amino acid sequences.

Additionally there is a module for running the [NetChop](http://www.cbs.dtu.dk/services/NetChop) proteosomal cleavage predictor:
* `NetChop`: requires locally installed version of [NetChop-3.1](http://www.cbs.dtu.dk/services/NetChop/)
