.. _pulp:

Pulsar Pipeline (PulP)
======================

This chapter gives brief overview of the LOFAR Pulsar Pipeline (PulP) and its processing steps. More details will be given during the PulP lecture and tutorial at the 5th LOFAR Data Processing School at ASTRON (Dwingeloo) on September 17-21, 2018. The cookbook will be later updated with the more detailed information.

--------
Overview
--------

In essence the pulsar pipeline is very simple. It reads the raw input data from an observation of a known pulsar, processes it in predetermined way depending on a beamformed observing mode, and creates the output tarballs for further ingest in the Long Term Archive (LTA). Most of the code have auxiliary functions for bookkeeping, figuring out where the raw data are, to read the observational parset file to know the observing parameters, and also feedback file with the metadata info for the LTA ingest. In a nutshell the core processing per beam or per beam/frequency part consists of a) either reading the raw data directly, or converting it to PSRFITS format, b) zapping the RFI, c) dedispersing/folding the time series using the given ephemeris file (.par), and d) creating the output data in the form of folded archives (.ar and/or .pfd) together with different diagnostic plots. Many of the processing steps can be modified by setting extra pipeline options or by turning them on/off. This core processing can be done with both PRESTO and dspsr/PSRCHIVE depending on user preference. By default, processing with both software packages are executed. The processing behavior can be modified either via command-line options when PulP is run manually, or via pipeline settings in MoM or xml-file when pipeline is run by the LOFAR automated system.

-------
History
-------

+ Over the years there were several major upgrades. The original non-Python (csh/bash) pipeline **pulp.sh** was written by Anastasia Alexov in ~2010 and was specifically designed to be run on the first LOFAR cluster CEP1.
+ Then, next Python version of the pipeline, PulP, was written from scratch by Vlad Kondratiev in early 2012 to have better structured and easily maintainable code with access to many available Python packages. And also to adapt it to the new CEP2 cluster.
+ In Mar-Apr 2014 the first major upgrade was done related to the switch to the new correlator **Cobalt** and to incorporate PulP into the LOFAR  automated framework. Before that the Pulp was always run manually (mostly by Vlad) to process the data recorded on CEP2. Only starting from the Cycle 3 (i.e. at the end of 2014) the data could be ingested to the LTA from the central system.
+ From the end of 2016 other major changes were done to accommodate switching to the new CEP4 cluster with the new way of running pipelines in the automated mode. Usage of Slurm, Docker containers, and Global File system was incorporated into the code.
+ PulP is still implemented in the LOFAR framework as a "black box" via the wrapper **pulsar\_pipeline.py** that calls the main {\tt pulp.py} from the PulP itself. This slows down bug fixing and further improvements quite significantly, and the plan is to truly incorporate Pulp into the central system as it is the case for imaging pipeline(s).

