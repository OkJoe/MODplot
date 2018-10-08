# MODplot-the plotting routine from DAT 
Python MOD analyser. 

Process rough workflow: 
Parse reads MOD_file, removes 'bad' events due to lumi blocks, trigger effects, JQC, Fastjet comparison, then creates a .dat file of the hardest jet objects. The events are filtered and processed as the followings. 

A lumiblock is a smallest unit of time for storing information produced by the collider (and does not necessarily coincide with the time of a circulation of the two beams). A lumiblock is labeled either "good" or "bad" by CERN, and only information produced in a "good" lumiblock shall be used. A list of good lumiblocks, labeled by their run #s and ls #s (or "LumiBlock" as shown in .mod files from MODProducer), are stored in ```2011lumibyls.csv``` (for 2011 opendata, which can be downloaded at http://opendata.cern.ch/record/1051). Multiple events are stored in a lumiblock. 

```parse.py``` loads the labels of all the "good" lumiblocks as appearing in  ```2011lumibyls.csv```. Then it calls ```analyze_MOD``` in ```analyze_MOD.py``` to process the MOD files in the input directory file by file. 

The events in the MOD file is selected by the followings, assuming a ```BeginEvent``` tag and an ```EndEvent``` tag encapsulate every event. 

If the event is not in a "good" lumiblock, it is ommited. 

If the event lacks any of the ```Trig```, ```AK5```, or ```PFC``` section, it is ommited (it is assumed that a legend line exist before each section). 

If the highest AK5 pT is in a trigger range with the corresponding trigger unfired, the event is ommited. 

The loose JQC (Jet Quality Criteria) is checked against the event and the event is rejected if it failed the pass. Otherwise the event is assigned with a jet quality of 1, 2, or 3, for loose, medium or strong. 

The PFCs (Particle Flow Candidate) are then read into fastjet program and various jet information of the event is produced. The fastjet-produced jets are then compared with the hardest AK5 jets, and the event is rejected if the two fail to match. 

Plot_dat then generates plots from this 

For the current version, make sure to input the .dat file, and copy the effective_luminosity_by_trigger.csv file under MODplot directory. 
