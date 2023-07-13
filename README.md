### Getting Started

## Multivariate Time Series characterization and forecasting of VoIP traffic in real mobile networks

We release an original real-world dataset used to perform the so-called "Multivariate time series prediction" possible both via statistical techniques (e.g. VAR) and Machine/Deep Learning (ML/DL) techniques. The dataset contains several features of cellular traffic organized into time series. The goal is to exploit statistical and learning-based techniques to predict the future behavior of a given feature. The reference paper for this dataset is "Multivariate Time Series characterization and forecasting of VoIP traffic in real mobile networks", by M. Di Mauro et al., published on IEEE Transactions on Network and Service Management (available here:) and also available on ArXiv here: 

## Cellular Traffic collection in a real environment

The equipment we used to build the real-world dataset includes:
- 1 mobile device equipped with Linphone (open-source softphone suppoorting RTCP-XR protocol) and a LTE-Advanced mobile SIM;
- 1 standard PC equipped with: i) Linphone softphone, ii) Wireshark to capture the network traffic in .pcap format.

The area where we performed experiments is near Salerno city (Italy), with a density of about 2000 people/sqKM.
The mobile operator is Vodafone. Currently (Mar. 2023), the radio coverage is almost entirely in LTE-Advanced (also marketed as 4G+).

## Dataset Construction

The Dataset contains network traffic gathered in a real cellular environment.  
The whole dataset includes 16 sub-datasets divided per codec and per network scenario:
- 8 codecs: G.722, G.729, GSM, G.711, Mpeg4-16, OPUS, Speex-8, Speex-16.
- 2 network scenarios:  
   **Mobile** (a cellular device communicates from a moving car at an average speed of 60 Km/H);  
   **Fixed** (a cellular device communicates being fixed in a place).

You download both:
- raw files (.pcap) available at: https://drive.google.com/file/d/1-r2Xd1VK6r7O_1KaXVYPus1Rcj6TF9DF/view?usp=sharing
- processed files (.txt) available at: https://github.com/mariodim/ml_mobile_dataset/blob/main/ML_TimeSeries_DATASETS.zip

Please note that all the experiments performed in the paper only refer to the Mobile scenario.

Each sub-dataset is the result of a post-processing stage on the raw pcap files produced by Wireshark.  
Each sub-dataset contains 6 temporal features:
- MOS (Mean Opinion Score) --> measures the call quality (expressed in a pure value between 1 and 5)
- BW (Bandwidth) --> measures the bandwidth consumed by a voice call ( expressed in kb/s)
- RTT (Round Trip Time) --> measures the interval beetwen a sent and a received packet (expressed in ms)
- JTR (Jitter) --> measures the inter-packet jitter (expressed in ms)
- DJB (De-jittering Buffer) --> measures the buffer lenght used to reduce jitter (expressed in ms)
- SNR (Signal-to-Noise ratio) --> measures the objective quality of the communication channel (expressed in dB)

Each processed sub-dataset is in .txt where values are separated by commas (see the following example): 

Time,MOS,BW,RTT,JTR,BUF,SNR  
1,4.5,83.888,0,60,67,27  
2,4.5,87.312,0,60,67,27  
3,4.5,85.6,0,60,67,27  
4,4.482136,85.6,99,44,54,38  
5,4.482136,83.888,99,44,54,38  
6,4.482136,87.312,99,44,54,38  
7,4.482136,83.888,99,44,54,38  
8,4.482136,87.312,99,44,54,38  
...

## Usage

Each sub-dataset must be uploaded into our Python routine available at:  
https://colab.research.google.com/drive/1pe-p8yEP8QaVgWcOpVZ2ZwweJEqAjHhu?authuser=1#scrollTo=m7tbPrVrhyJB

After uploaded a sub-dataset (e.g. mob_g722.txt, meaning that the traffic is collected within the mobile scenario and the codec used is G.722), set the parameters in the first "cell" of the Python code:  

- filename --> insert the name of the uploaded file (e.g. "mob_g722.txt");
- methods --> you can choose one of the implemented techniques for time series prediction by setting True or False;
- param --> you can define the size of your ML network (number of dense neurons, number units, epochs, etc.);
- perc_train --> you can choose the percentage of training size (the test size is set accordingly);
- n_past --> is the number of past values used in the training set;
- n_fut --> is the number of future samples to be predicted (default = 1)

With the default values set, you have just to upload the file, set its name and run.

## Output

Output files include:  

- TXT files containing time series predictions per technique --> e.g. the output file mob_g722_cnn.txt is a 12-column file in this format: column #1 contains original values of MOS, column #2 contains predicted values of MOS, column 2 contains original values of BW, column #2 contains predicted values of BW, and so forth. Once exported, such files can be obviously used to reproduce the plots through different plot tools;
 
- Information about RMSE, MAE, MAPE per each technique

- Information about training time per each technique (directly shown in the output code).

