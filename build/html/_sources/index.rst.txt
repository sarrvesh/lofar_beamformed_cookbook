.. Beamforming Cookbook documentation master file, created by
   sphinx-quickstart on Thu Feb 28 14:29:01 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to LOFAR Beamforming Cookbook
=====================================

.. only:: html

   Beamforming means "pointing" an antenna array and shaping the beam by giving each antenna the right delay, then adding the signals. Beam-formers in place at the LOFAR stations and the correlator apply phase offsets to the signal to "point" beam(s) in particular directions and track known sources. The beam-formed data from LOFAR are used for applications requiring high time and/or frequency resolution, such as pulsars, dynamic spectra of solar and planetary emission, and scintillation. This cookbook provides a description of the beam-forming capabilities of LOFAR including the format of recorded data and the reading of these data using Python. A "dynamic spectrum toolkit" developed for reduction and visualisation of these data is also demonstrated.

.. toctree::
   :maxdepth: 1
   :caption: Contents:
   
   introduction
   solarapplications
   pulp
   
