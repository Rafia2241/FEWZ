# FEWZ
Prerequirement: Since we need to use the LHAPDF under slc7_amd64_gcc900, we need to set up the environment
```console
cmsrel CMSSW_12_0_1 
cd CMSSW_12_0_1/src 
cmsenv 
cd - 
```
To untar the FEWZ tarball, do as follows:
```
wget http://www.hep.anl.gov/fpetriello/FEWZ_3.1.rc.tar.gz
tar xf FEWZ_3.1.rc.tar.gz 
cd FEWZ_3.1.rc 
```

Modify the following two lines in the makefile to link to the LHAPDF version we want to use:
```
LHAPDF = on 
LHADIR = /cvmfs/cms.cern.ch/slc7_amd64_gcc900/external/lhapdf/6.3.0/lib 
```
then
```
make
```
For local run modify the PDF set 
```
'PDF set = ' 'NNPDF31_nnlo_as_0118_luxqed' 
```
(the local run is just for test the proper installation of FEWZ and for your better understanding of FEWZ, for higher order calculation, 
it's strongly not recommended to run locally)
Run the following commands for local run:
```
cd /YOURPATH/CMSSW_12_0_1/src
cmsenv
cd /YOURPATH/FEWZ_3.1.rc/bin
./local_run.sh z DY_LO input_z.txt histograms.txt _mytest . 
```
then following the instruction of FEWZ manual, run 
```
./finish.sh DY_LO/ LO._mytest 
```
For submitting the condor jobs 
```
./condor_prepare_cards.sh z DY_LO input_z.txt histograms.txt _lo . 
cd DY_LO 
condor_submit job_desc 
```
Use condor_q to monitor the job status
After all the jobs complete, you can use following command to get the final results:
```
./finish DY_LO LO.txt 
```
For Running Scale of Drell-Yan, the phase space will be divided according to the invariant mass of two leptons, and central value of the mass bin will be set as the the Renormalization and Factorization scales.
The steer.py is used to prepare all the corresponding sub-directory and condor stuff. 
The harvest.py is used for submission and combine/merge the results.

Preparing the condor stuffs for running scale
```
python steer.py -b z -n DY -e 13600 -m 50 80 1000 1000000 -R true --QCDOrder 2 --EWOrder 1
```
The command above will create a directory named "Running_scale_Dir" and it contains three sub-directory, mass bin 50-80, 80-1000 and 1000-1000000 

 Submit condor jobs for running scale
 ```
  python harvest.py -s true
 ```
 then merge results
 ```
  python harvest.py -m true -o NNLO
 ```
 
