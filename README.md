pyseer
======

[SEER](https://github.com/johnlees/seer), reimplemented in python by
[Marco Galardini](https://github.com/mgalardini) and [John Lees](https://github.com/johnlees)

    pyseer kmers.gz phenotypes.tsv structure.tsv --min-af 0.01 --max-af 0.99 --cpu 15 --filter-pvalue 1E-8

[![Build Status](https://travis-ci.org/mgalardini/pyseer.svg?branch=master)](https://travis-ci.org/mgalardini/pyseer)

Motivation
----------

Kmers-based GWAS analysis is particularly well suited for bacterial samples,
given their high genetic variability. This approach has been for the first
time formally implemented by [Lees, Vehkala et al.](https://www.nature.com/articles/ncomms12797),
in the form of the [SEER](https://github.com/johnlees/seer) software.

The reimplementation presented here should be consistent with the
current version of seer. Based on our few
tests the results are going to be very similar to the
C++ implementation, though approximation at low p-values may cause small
differences. However, **no guarantee whatsoever is given that the
script works in the same way as the original**, especially for corner cases
and across millions of kmers.

Citation
--------

``Lees, John A., et al. "Sequence element enrichment analysis to determine
the genetic basis of bacterial phenotypes." Nature communications 7 (2016): 12797.``

``doi: 10.1038/ncomms12797``

Prerequisites
-------------

Between parenthesis the versions the script was tested against:

* python 3+ (3.5.3)
* numpy (1.13.3)
* scipy (1.0.0)
* pandas (0.21.0)
* statsmodels (0.8.0)

Installation
------------

Since everything fits in a script (`pyseer`), you can just place it in your `$PATH`,
make it executable (`chmod 755 pyseer`) and type:

    pyseer -h

**Note:** if you wish to use multithreading,
the script assumes that your default python's interpreter is at least version 3.
If that is not the case then edit the first line to:

    #!/usr/bin/env python3

Population structure
--------------------

`pyseer` accepts as input a tab-delimited square matrix of distances between samples, with
the first row and column listing sample names. Such a square matrix can be easily obtained
by piping the `mash dist` command into the provided `square_mash` script:

    mash dist samples.msh samples.msh | python square_mash > mash.tsv

**Note:** `square_mash` extracts the sample names as the string after the last `/` character
and up to the first full stop (`.`).

Testing
-------

While waiting on proper unit tests to be implemented, you can check that the script doesn't crash
by running:

    cd test/ && bash run_test.sh && cd ../

Notes
-----

SEER's features present in this script:

* binary and continuos phenotypes
* binary phenotypes: Firth regression upon failure of Newton-Raphson
* population structure correction
* MAF filtering
* kmers prefiltering
* multi-threading
* filtering of results based on LRT p-value
* covariates and intercept betas are reported
* user-defined covariates (NOTE: still experimental and possibly bugged)
* automatic determination of binary/continuous phenotypes

Absent features:

* reasons for failures are not reported

Additional features:

* Multidimensional scaling from squared [mash](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-016-0997-x) matrix
* List of samples without the kmer are also output

Copyright
---------

Copyright (C) <2017> EMBL-European Bioinformatics Institute

This program is free software: you can redistribute it and/or
modify it under the terms of the GNU General Public License as
published by the Free Software Foundation, either version 3 of
the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the   
GNU General Public License for more details.

Neither the institution name nor the name pyseer
can be used to endorse or promote products derived from
this software without prior written permission.
For written permission, please contact <marco@ebi.ac.uk>.

Products derived from this software may not be called pyseer
nor may pyseer appear in their names without prior written
permission of the developers. You should have received a copy
of the GNU General Public License along with this program.
If not, see <http://www.gnu.org/licenses/>.
