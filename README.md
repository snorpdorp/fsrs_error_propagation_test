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

It will take the last N_TRIALS and use them for prediction testing
and remove them from the review pool

It will then run N_TRIALS.

For each trial, the following occurs:
    1) Choose a random review from the prediction testing pool and use
    that as the test review for this trial.

    2) Randomly evenly split the trials into one of two groups, a
    fitting group, and a testing group.

    3a) From the fitting group, calculate the FSRS parameters, and
    also the statistical error associated with said parameters, then
    propagate values and error to evaluate the test review for an
    interval, as well as expected statistical uncertainty asosciated
    with evaluation.

    3b) From the testing group, calculate the FSRS paramters, and
    then evaluate the test review for an interval.

    4) Calculate the Z-score for how far away the testing group's
    calculated interval is from the fitting group's calculated
    interval and calculated stasticial uncertainty. Save the Z-score
    into an array for later calculations.

After that, it will take the Z-score array, some data analysis is
done to see how well it follows a Gaussian distribution.  If it does
indeed follow a Gaussian distribution (at high numbers of trials,
where it should be clearly obvious if it does or doesn't) then that
means that we have succesfully found an algorithm to calculate the
statsitical uncertainty associated with calculated an interval for a
given review and a given fitting of FSRS parameters from other reviews.

The data analysis on the Z-score, as well as visual data, is shown to
the user.

Additional data of a pure Gaussian sampling for the same number
of samples as N_TRIALS is also shown to the user.

If my hypothesis is correct, then the Z-score array should always be
as Gaussian-y or better than the pure guassian sampling.

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
