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

modify the following two lines in the makefile to link to the LHAPDF version we want to use:
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
use condor_q to monitor the job status
After all the jobs complete, you can use following command to get the final results:
```
./finish DY_LO LO.txt 
```
