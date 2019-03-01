.. _solarapplication:

Solar applications
==================

------------
Introduction
------------

The beamformed data is a key tool for solar observations. It consists on a single- or multiple-beam dynamic spectrum of the Sun, that can be recorded in Stokes I or in full Stokes (IQUV). When multiple beams are used we call this configuration "tied array beam" (TAB), which is explained in section dynamicspectrum_. While the single dynamic spectrum and the Stokes polarization are explained in section tab_ and in section stokes_ respectively.

As introduced in section :ref:`Beam-formed data format <bfdataformat>`, the format of the beamformed data is an HDF5 file containing the meta-data and a link within it to the time-frequency data stored in a separate binary file. Each file is labelled as follows::

   <ObsID>_SAP<number>_BEAM<number>_S<number>_P<number>.h5
   <ObsID>_SAP<number>_BEAM<number>_S<number>_P<number>.raw

where

+ <OBsID> is the observation ID number
+ SAP<number> refers to the station beam, and <number> is three digits.  e.g., the first station beam is labelled as SAP000
+ BEAM<number> refers to each coherent tied-array beam, incoherent beam or Fly's Eye beam and <number> is three digits starting with 000. Here, the  order is important: Coherent tied-array beams are numbered first with specified  pointing directions numbered in order of specification ahead of those calculate d from tied-array "rings". Incoherent Stokes beams are numbered next and Fly's Eye beams numbered last.  No numbers are reserved for particular cases, so all available beams will be numbered sequentially and the experiment setup needs to be known to determine which numbers refer to which coherent, incoherent or fly's eye beam.
+ S<number> is the Stokes parameter and <number> is 0-3 for Stokes I,Q,U,V respectively.
+ P<number> is a part number used to differentiate files when subsets of sub-bands are recorded separately. <number> is three digits with 000 referring to the first <X> sub-bands, 001 refering to the next <X> sub-bands and so on. If all sub-bands are recorded to the same file's only 000 is used.

.. _dynamicspectrum:

----------------
Dynamic spectrum
----------------

In the case of a simple Stokes I solar dynamic spectrum, the data-set will consist of only two files, relative to one SAP, with only BEAM000 and with only S0 for Stokes I. An example data set filename is the following::

   L646181_SAP000_B000_S0_P000_bf.h5
   L646181_SAP000_B000_S0_P000_bf.raw

A python routine may be used to plot the dynamic spectrum with the proper time and frequency labeling. The list of modules to be imported are the following::

   import numpy
   import matplotlib
   from pylab import figure,imshow,xlabel,ylabel,title,close,colorbar
   import h5py
   import datetime
   import numpy as np
   import matplotlib.pyplot as plt
   from matplotlib.ticker import MaxNLocator
   from matplotlib.dates import date2num
   from matplotlib import dates

The input file can be specified and the data array dimension may be read in python as follows::

   obsid = 'L646181' 
   file = obsid + '_SAP000_B000_S0_P000_bf.h5'
   f = h5py.File( file, 'r' )
   from tempfile import mkdtemp
   import os.path as path
   data_shape = f['SUB_ARRAY_POINTING_000/BEAM_000/STOKES_0'].shape

At this stage, from the shape of the data array we can construct the frequency and time axis of the dynamic spectrum. ::

   t_lines = data_shape[0]
   f_lines = data_shape[1]
   
   total_time = f.attrs.values()[22] #in seconds
   lines_per_sec = t_lines/total_time
   
   start_min = 0  
   end_min = total_time/60
   
   start_line = int( (start_min/(total_time/60.))*t_lines ) 
   end_line = int( (end_min/(total_time/60.))*t_lines ) 
   
   start_freq_ds = f.attrs.values()[30] #in MHz
   end_freq_ds = f.attrs.values()[8]
   
   t_resolution = (total_time/t_lines)*1000 #in milliseconds
   f_resolution = (end_freq_ds - start_freq_ds)/f_lines

.. _stokes:

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Polarization and stokes parameters
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Beam-formed dynamic spectra recorded in full Stokes mode returns four different files from S0 to S3::

   L646181_SAP000_B000_S0_P000_bf.h5
   L646181_SAP000_B000_S0_P000_bf.raw
   L646181_SAP000_B000_S1_P000_bf.h5
   L646181_SAP000_B000_S1_P000_bf.raw
   L646181_SAP000_B000_S2_P000_bf.h5
   L646181_SAP000_B000_S2_P000_bf.raw
   L646181_SAP000_B000_S3_P000_bf.h5
   L646181_SAP000_B000_S3_P000_bf.raw

Each of the files can be plotted as explained in the previous section. S0 is the total intensity I, while S1, S2 and S3 are Stokes Q, U, and V respectively.

.. _tab:

-----------------------
Tied array beam imaging
-----------------------

A powerful application of the beam-formed LOFAR observations are the tied-array-beam (TAB) observations. This observing mode allows to use the beam returned from multiple core stations combined coherently to form multiple tied-array beams, each with a different pointing direction within the overall station beam but covering the same range of subbands as the main beam (SAP). A large number of TABs can be formed, limited by the computation and processor load on the correlator and ability to keep up with data writing to disk.  In practice this means that the total number of TABs which can be formed depends upon the number of subbands (across all SAPs) and the number of stations used.  For example, 222 TABs can be formed using 161 subbands and the six superterp stations, or 171 TABs can be formed using 258 subbands and 24 core stations.  Both examples are Stokes I only and near the limits of the current system.   If all Stokes parameters are desired, then the total number of TABs possible in each of these examples would need to be divided by four. An example of a solar application of this method can be seen in the work of `Morosan et al (2014) <http://adsabs.harvard.edu/abs/2014A%26A...568A..67M>`_.

The data product for a TAB observation will be a series of beam-formed LOFAR files as the number of TAB beams. For the example observation below, 129 TABs are used. ::

   L646173_SAP000_B000_S0_P000_bf.h5
   L646173_SAP000_B000_S0_P000_bf.raw
   L646173_SAP000_B001_S0_P000_bf.h5
   L646173_SAP000_B001_S0_P000_bf.raw
   ...
   L646173_SAP000_B129_S0_P000_bf.h5
   L646173_SAP000_B129_S0_P000_bf.h5
