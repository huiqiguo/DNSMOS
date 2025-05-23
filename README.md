# **Deep Noise Suppression Performance Evaluation Based On Mean Opinion Score (DNSMOS)**

## **Overview**
Subjective evaluation of speech quality is the most reliable way to evaluate speech enhancement methods, since speech quality should be optimised for subjective human perception in the first place. The DNSMOS model utilises a set of deep neural networks with a self-teaching structure to evaluate speech quality and generate an indicative Mean Opinion Score (MOS). More specifically, it measures signal quality (SIG), background noise quality (BAK), as well as overall audio quality (OVRL). The DNSMOS model is trained on and evaluated against the scores generated using the ITU-T P.808 subjective testing framework, which is taken to be the ground truth in this project. Evaluation of audio quality using P.808 scores is known to be sensitive to noise and struggle with generalising to audio samples that vary significantly from training data. Hence, with multi-stage training, the DNSMOS model aims to be more robust to noise and generalise more accurately to a wide variety of datasets.

## **Prerequisites**
1. Python 3.6 and above. Download the installer from https://www.python.org/downloads/.
2. Install the required Python libraries by running ```pip install -r requirements.txt``` from the project's directory in Command Prompt.

## **Usage**
### **1. Import Audio Samples**
* Download the test datasets from this [link](https://dysononline-my.sharepoint.com/:u:/g/personal/huiqi_guo_dyson_com/ERjd1k7ci-JKjBIAbldXxscBnNLUpZgnzg8W3wdQFQksSg?e=fcN22w) and move the extracted datasets folder into your project folder. This folder contains 3 subfolders of audio samples which the DNSMOS model was evaluated on. 
* Alternatively, create an empty datasets folder and store the audio samples that you want to evaluate in .wav format inside the folder.

### **2. Generate CSV Files**
* From your terminal, run the dnsmos_local.py script to generate CSV files containing MOS scores for the specified dataset, e.g. ```python dnsmos_local.py -t ./datasets/emotionalspeech -o ./csv/emotionalspeech.csv```. The ```-t``` flag indicates the file path to the audio samples from Step 1, whereas the ```-o``` flag specifies the path of the output CSV file.
* Optionally, the ```-p``` flag can be added to use the personalised DNSMOS model instead of the general one, e.g. ```python dnsmos_local.py -t ./datasets/emotionalspeech -o ./csv/emotionalspeech_personalised.csv -p```. Both models differ in the parameters used for polynomial fitting, as can be seen in the dnsmos_local.py script.
* By default, the model randomly selects 250 audio samples from the folder for analysis. This value can be easily adjusted in the dnsmos_local.py script. Refer to the comments in the script for more information.

### **3. Evaluate the Generated MOS Scores**
* To evaluate the performance of the DNSMOS model, the generated MOS scores can be compared against the P.808 MOS scores, which are taken to be the ground truth in this project. 
* Edit the runEval.py script to include the names of the CSV files generated in Step 2.
* The runEval.py script in turn utilises the performance.py script, which computes the Mean Squared Error (MSE) between the generated MOS scores and the P.808 MOS scores and prints it in the terminal. Should a different error metric be preferred, the performance.py script can be easily modified.

## **Findings**
* The findings.csv file summarises some preliminary findings from the default test datasets. 
* Comparing the raw scores and the polyfit scores, which are both automatically generated in Step 2 above, the performance for emotional speech and mono sounds is better with the polyfit model.
* The OVRL, SIG and BAK scores are the most accurate indicators of audio quality for mono sounds, emotional speech and read speech respectively. Comparing these scores with the ones generated from the personalised DNSMOS model, as indicated by ```(p)``` in the column headers, the regular model performed better than the personalised model for both read speech and emotional speech.

## **Credits and Legal Notices**
This project includes code and data adapted from the [DNS-Challenge](https://github.com/microsoft/DNS-Challenge.git) repository by Microsoft. Microsoft and its contributors grant a license to the Microsoft documentation and other content in the repository under the [Creative Commons Attribution 4.0 International Public License](https://creativecommons.org/licenses/by/4.0/legalcode), see the [LICENSE](LICENSE) file, and grant a license to any code in the repository under the [MIT License](https://opensource.org/licenses/MIT), see the [LICENSE-CODE](LICENSE-CODE) file.

Microsoft, Windows, Microsoft Azure and/or other Microsoft products and services referenced in the documentation may be either trademarks or registered trademarks of Microsoft in the United States and/or other countries. The licenses for this project do not grant you rights to use any Microsoft names, logos, or trademarks. Microsoft's general trademark guidelines can be found at http://go.microsoft.com/fwlink/?LinkID=254653.

Privacy information can be found at https://privacy.microsoft.com/en-us/.

Microsoft and any contributors reserve all other rights, whether under their respective copyrights, patents, or trademarks, whether by implication, estoppel or otherwise.

### **Dataset Licenses**
MICROSOFT PROVIDES THE DATASETS ON AN "AS IS" BASIS. MICROSOFT MAKES NO WARRANTIES, EXPRESS OR IMPLIED, GUARANTEES OR CONDITIONS WITH RESPECT TO YOUR USE OF THE DATASETS. TO THE EXTENT PERMITTED UNDER YOUR LOCAL LAW, MICROSOFT DISCLAIMS ALL LIABILITY FOR ANY DAMAGES OR LOSSES, INLCUDING DIRECT, CONSEQUENTIAL, SPECIAL, INDIRECT, INCIDENTAL OR PUNITIVE, RESULTING FROM YOUR USE OF THE DATASETS.

The datasets are provided under the original terms that Microsoft received such datasets. See below for more information about each dataset.

The datasets used in this project are licensed as follows:
1. Clean speech:
* https://librivox.org/; License: https://librivox.org/pages/public-domain/
* PTDB-TUG: Pitch Tracking Database from Graz University of Technology https://www.spsc.tugraz.at/databases-and-tools/ptdb-tug-pitch-tracking-database-from-graz-university-of-technology.html; License: http://opendatacommons.org/licenses/odbl/1.0/
* Edinburgh 56 speaker dataset: https://datashare.is.ed.ac.uk/handle/10283/2791; License: https://datashare.is.ed.ac.uk/bitstream/handle/10283/2791/license_text?sequence=11&isAllowed=y
* VocalSet: A Singing Voice Dataset https://zenodo.org/record/1193957#.X1hkxYtlCHs; License: Creative Commons Attribution 4.0 International
* Emotion data corpus: CREMA-D (Crowd-sourced Emotional Multimodal Actors Dataset)
https://github.com/CheyneyComputerScience/CREMA-D; License: http://opendatacommons.org/licenses/dbcl/1.0/
* The VoxCeleb2 Dataset http://www.robots.ox.ac.uk/~vgg/data/voxceleb/vox2.html; License: http://www.robots.ox.ac.uk/~vgg/data/voxceleb/
The VoxCeleb dataset is available to download for commercial/research purposes under a Creative Commons Attribution 4.0 International License. The copyright remains with the original owners of the video. A complete version of the license can be found here.
* VCTK Dataset: https://homepages.inf.ed.ac.uk/jyamagis/page3/page58/page58.html; License: This corpus is licensed under Open Data Commons Attribution License (ODC-By) v1.0.
http://opendatacommons.org/licenses/by/1.0/

2. Noise:
* Audioset: https://research.google.com/audioset/index.html; License: https://creativecommons.org/licenses/by/4.0/
* Freesound: https://freesound.org/ Only files with CC0 licenses were selected; License: https://creativecommons.org/publicdomain/zero/1.0/
* Demand: https://zenodo.org/record/1227121#.XRKKxYhKiUk; License: https://creativecommons.org/licenses/by-sa/3.0/deed.en_CA

3. RIR datasets: OpenSLR26 and OpenSLR28:
* http://www.openslr.org/26/
* http://www.openslr.org/28/
* License: Apache 2.0

### **Code License**
MIT License

Copyright (c) Microsoft Corporation.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.