VERSION FIRST DRAFT

The end-goal of this script is to demonstrate the feasibility of
calculating the statistical error associated with calculating FSRS
parameters and how to extrapolate that uncertainty through the
interval calculation process, such that users can only be given
intervals which are not too long due to poor FSRS calibration.

As of the time of writing, it assumes that the statistics associated
with a single review are equivalent to the statistics of a radiaition
detection event in a radiation detector, and thus follows a Poisson
distribution with λ=1.

This is the equivalent of an n-dimensional detector array which is
sparse where virtually all detectors have 0 events.

-----------------------------

Feed into the script N reviews ( > 2 * N_PARAMS^2 ).

It will then begin running trials:

For each trial, the following occurs:
    1) Randomly and equally split the reviews into 3 equal, distinct,
        subsets:
        a) A Fitting group
        b) A Checking group
        c) A Testing group

    2a) From the Fitting group, calculate the FSRS parameters, and
    also the statistical error associated with said parameters

    2b) From the Checking group, calculate the FSRS paramters

    2c) For each of the test reviews in the Testing group, evaluate
        a) the predicted interval from review from Fitting group's
            FSRS params
        b) the amount of statistical error associated with above
        c) the predicted interval from review from Checking group's
            FSRS params
        d) A "Z-score" of (c - a) / b, i.e. how many sigmas away the
        two values are.

    3) Save Z-scores from previous step for later statistical analysis.

After running the trials, it will take the Z-score array, and then some data
analysis is done to see how well it follows a Gaussian distribution. 
If it does indeed follow a Gaussian distribution (at high numbers of
trials, where it should be clearly obvious if it does or doesn't)
then that means that we have succesfully found an algorithm to
calculate the statsitical uncertainty associated with calculating an
interval for a given review from the data used to fit the FSRS
parameters and doing the review.

The data analysis on the Z-score, as well as visual data, is shown to
the user.

Additional data of a pure Gaussian sampling for the same number
of samples as N_TRIALS is also shown to the user.

If my hypothesis is correct, then the Z-score array should always be
as Gaussian-y or better than the pure guassian sampling, and mu
should be statistically equal to 0, and sigma should be statistically
equal to 1.

----------------

As of right now, all data is semi-deterministic toy data so
everything looks Gaussian no matter what you do, so no meaningful
tests can be run aside from checking the validity of the program
itself to avoid bugs and ensure smooth operation.


------------------

To execute:

    git clone https://github.com/snorpdorp/fsrs_statistical_uncertainty_propagation_test.git
    cd fsrs_statistical_uncertainty_propagation_test/
    python3 -m venv .venv
    source .venv/bin/activate
    python3 -m pip install -r requirements.txt
    ./statistical_uncertainty_propagation.py
