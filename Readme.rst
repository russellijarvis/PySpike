PySpike
=======

.. image:: https://badge.fury.io/py/pyspike.png
    :target: http://badge.fury.io/py/pyspike
.. image:: https://travis-ci.org/mariomulansky/PySpike.svg?branch=master
    :target: https://travis-ci.org/mariomulansky/PySpike

PySpike is a Python library for the numerical analysis of spike train similarity. 
Its core functionality is the implementation of the bivariate ISI_ and SPIKE_ distance [#]_ [#]_ as well as SPIKE-Synchronization_ [#]_.
Additionally, it provides functions to compute multivariate profiles, distance matrices, as well as averaging and general spike train processing.
All computation intensive parts are implemented in C via cython_ to reach a competitive performance (factor 100-200 over plain Python).

PySpike provides the same fundamental functionality as the SPIKY_ framework for Matlab, which additionally contains spike-train generators, more spike train distance measures and many visualization routines.

All source codes are available on `Github <https://github.com/mariomulansky/PySpike>`_  and are published under the BSD_License_.

.. [#] Kreuz T, Haas JS, Morelli A, Abarbanel HDI, Politi A, *Measuring spike train synchrony.* J Neurosci Methods 165, 151 (2007) `[pdf] <http://wwwold.fi.isc.cnr.it/users/thomas.kreuz/images/Kreuz_JNeurosciMethods_2007_Spike-Train-Synchrony.pdf>`_

.. [#] Kreuz T, Chicharro D, Houghton C, Andrzejak RG, Mormann F, *Monitoring spike train synchrony.* J Neurophysiol 109, 1457 (2013) `[pdf] <http://wwwold.fi.isc.cnr.it/users/thomas.kreuz/images/Kreuz_JNeurophysiol_2013_SPIKE-distance.pdf>`_

.. [#] Kreuz T, Mulansky M and Bozanic N, *SPIKY: A graphical user interface for monitoring spike train synchrony*, J Neurophysiol, in press (2015)

Important Changelog
-----------------------------

With version 0.2.0, the :code:`SpikeTrain` class has been introduced to represent spike trains.
This is a breaking change in the function interfaces.
Hence, programs written for older versions of PySpike (0.1.x) will not run with newer versions.


Requirements and Installation
-----------------------------

PySpike is available at Python Package Index and this is the easiest way to obtain the PySpike package.
If you have `pip` installed, just run

.. code:: bash

   sudo pip install pyspike

to install pyspike.
PySpike requires `numpy` as minimal requirement, as well as a C compiler to generate the binaries.

Install from Github sources
...........................

You can also obtain the latest PySpike developer version from the github repository.
For that, make sure you have the following Python libraries installed:

- numpy
- cython
- matplotlib (for the examples)
- nosetests (for running the tests)

In particular, make sure that cython_ is configured properly and able to locate a C compiler, otherwise PySpike will use the much slower Python implementations.

To install PySpike, simply download the source, e.g. from Github, and run the :code:`setup.py` script:

.. code:: bash

    git clone https://github.com/mariomulansky/PySpike.git
    cd PySpike
    python setup.py build_ext --inplace

Then you can run the tests using the `nosetests` test framework:

.. code:: bash

    nosetests

Finally, you should make PySpike's installation folder known to Python to be able to import pyspike in your own projects.
Therefore, add your :code:`/path/to/PySpike` to the :code:`$PYTHONPATH` environment variable.


Examples
-----------------------------

The following code loads some exemplary spike trains, computes the dissimilarity profile of the ISI-distance of the first two :code:`SpikeTrain` s, and plots it with matplotlib:

.. code:: python

    import matplotlib.pyplot as plt
    import pyspike as spk
    
    spike_trains = spk.load_spike_trains_from_txt("PySpike_testdata.txt",
                                                  edges=(0, 4000))
    isi_profile = spk.isi_profile(spike_trains[0], spike_trains[1])
    x, y = isi_profile.get_plottable_data()
    plt.plot(x, y, '--k')
    print("ISI distance: %.8f" % isi_profile.avrg())
    plt.show()


The following example computes the multivariate ISI-, SPIKE- and SPIKE-Sync-profile for a list of spike trains using the :code:`isi_profile_multi`, :code:`spike_profile_multi`, :code:`spike_sync_profile_multi` functions:

.. code:: python

    spike_trains = spk.load_spike_trains_from_txt("PySpike_testdata.txt",
                                                  edges=(0, 4000))
    avrg_isi_profile = spk.isi_profile_multi(spike_trains)
    avrg_spike_profile = spk.spike_profile_multi(spike_trains)
    avrg_spike_sync_profile = spk.spike_sync_profile_multi(spike_trains)

More examples with detailed descriptions can be found in the `tutorial section <http://mariomulansky.github.io/PySpike/#tutorial>`_.

===============================================================================

*The work on PySpike was supported by the European Comission through the Marie
Curie Initial Training Network* `Neural Engineering Transformative Technologies
(NETT) <http://www.neural-engineering.eu/>`_ *under the project number 289146.*


**Python/C Programming:**
 - Mario Mulansky

**Scientific Methods:**
 - Thomas Kreuz
 - Daniel Chicharro
 - Conor Houghton
 - Nebojsa Bozanic
 - Mario Mulansky

.. _ISI: http://www.scholarpedia.org/article/Measures_of_spike_train_synchrony#ISI-distance
.. _SPIKE: http://www.scholarpedia.org/article/SPIKE-distance
.. _SPIKE-Synchronization: http://www.scholarpedia.org/article/Measures_of_spike_train_synchrony#SPIKE_synchronization
.. _cython: http://www.cython.org
.. _SPIKY: http://wwwold.fi.isc.cnr.it/users/thomas.kreuz/Source-Code/SPIKY.html
.. _BSD_License: http://opensource.org/licenses/BSD-2-Clause
