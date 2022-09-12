### Getting Started

## Cellular Traffic collection in a real environment

The equipment we used to build the real-world dataset includes:
- 1 mobile device equipped with Linphone (open-source softphone suppoorting RTCP-XR protocol) and a LTE-Advanced mobile SIM;
- 1 standard PC equipped with: i) Linphone softphone, ii) Wireshark to capture the network traffic in .pcap format.

The area where we performed experiments is near Salerno city (Italy). The mobile operator is Vodafone. Currently the radio coverage is almost entirely in LTE-Advanced (also marketed as 4G+) as can be seen in the following picture.


<img src="[https://user-images.githubusercontent.com/16385982/189587559-d93204dd-02ac-44c0-9224-2c77752d4547.png">

## Dataset Construction

The Dataset contains network traffic gathered in a real cellular environment.  
The whole dataset includes 16 sub-datasets divided per codec and per network scenario:
- 8 codecs: G.722, G.729, GSM, G.711, Mpeg4-16, OPUS, Speex-8, Speex-16.
- 2 network scenarios:  
   **Mobile** (a cellular device communicates from a moving car at an average speed of 60 Km/H);  
   **Fixed** (a cellular device communicates being fixed in a place).

Each sub-dataset is the result of a post-processing stage on the raw pcap files produced by Wireshark.  
Each sub-dataset contains 6 temporal features:
- MOS (Mean Opinion Score) --> measures the call quality (expressed in a pure value between 1 and 5)
- BW (Bandwidth) --> measures the bandwidth consumed by a voice call ( expressed in kb/s)
- RTT (Round Trip Time) --> measures the interval beetwen a sent and a received packet (expressed in ms)
- JTR (Jitter) --> measures the inter-packet jitter (expressed in ms)
- DJB (De-jittering Buffer) --> measures the buffer lenght used to reduce jitter (expressed in ms)
- SNR (Signal-to-Noise ratio) --> measures the objective quality of the communication channel (expressed in dB)

Fro the sake of simplicity, each sub-dataset is in .txt where values are separated by commas (see the following example): 

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
https://colab.research.google.com/drive/1xd9BxLasPSi1VaXLSPM7ew_8agM6rkap?usp=sharing

After uploaded a sub-dataset (e.g. mob_g722.txt, meaning that the traffic is collected within the mobile scenario and the codec used is G.722), set the parameters in the first "cell" of the Python code:  

- filename --> insert the name of the uploaded file (e.g. "mob_g722.txt")
- train_size --> you can choose the training size (the test size is set accordingly) 
- param --> you can define the size of your ML network (number of dense neurons, number of LSTM units, etc.)  
- epochs --> you can set the number of epochs  
- models --> you can choose one of the implemented ML techniques for time series prediction: Long-Short Term Memory (code: 0), Convolutional Neural Networks (code: 1), Gated Recurrent Units (code: 2), Random Forest (code: 3), Multilayer Perceptron (code: 4), Gradient Boosting (code: 5) 
- cols_to_predict --> you can choose one among the available features to predict (MOS, BW, RTT, JIT, DJB, SNR)

Other parameters can be modified in the second "cell" of the Python code:
- n_past --> is the number of past values used in the training set. Currently, we automate such a choice through the usage of the Akaike Information Criterion which selects the optimal value of n_past by minimizing the prediction error. In case users want to set manually such a number, it suffices to manually set the n_past variable (e.g., n_past=10)
- scaler --> to homogeneize the values of all features we implement a MinMax scaler so that all the values are normalized in the (0,1) range. USers are free to change the scaler or to leave features at their original values if needed

With the default values set, you have just to upload the file, set its name and run.

## Output

Output files include:  
- TXT files containing time series predictions per technique --> e.g. the output file mob_g722_cnn.txt is a 6-column file containing prediction per each feature obtained by applying the CNN technique. Such files have been used to produce the prediction plots embedded in the output. Once exported, such files can be obviously used to reproduce the plots eventually through different plot tools.  
- TXT files containing performance indicators per technique --> e.g. the output file mae_cnn.txt contains the Mean Absolute Error obtained after applying CNN technique per feature; similarly, the output file mse_cnn.txt contains the Mean Squared Error obtained after applying CNN technique per feature. Please note that such performance are also displayed directly in the output code.  
- Information about training time per each technique (directly shown into the output code). 

