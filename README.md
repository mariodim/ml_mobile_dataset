### Getting Started

## Cellular Traffic collection in a real environment

The equipment we used to build the real-world dataset includes:
- 1 mobile device equipped with Linphone (open-source softphone suppoorting RTCP-XR protocol);
- 1 standard PC equipped with: i) Linphone softphone, ii) Wireshark to capture the network traffic in .pcap format.

## Dataset Construction

The Dataset contains network traffic gathered in a real cellular environment. 
The whole dataset includes 16 sub-datasets divided per codec and per network scenario:
- 8 codecs: G.722, G.729, GSM, G.711, Mpeg4-16, OPUS, Speex-8, Speex-16.
- 2 network scenarios: **Mobile** (a cellular device communicates from a moving car at an average 60 Km/H speed);                                                                  **Fixed** (a cellular device communicates being fixed in a place).

Each sub-dataset is the result of a post-processing stage on the raw pcap files produced by Wireshark.
Each sub-dataset contains 6 temporal features_
- MOS (Mean Opinion Score) --> measures the call quality (expressed in a pure value between 1 and 5)
- BW (Bandwidth) --> measures the bandwidth consumed by a voice call ( expressed in kb/s)
- RTT (Round Trip Time) --> measures the interval beetwen a sent and a received packet (expressed in ms)
- JTR (Jitter) --> measures the inter-packet jitter (expressed in ms)
- DJB (De-jittering Buffer) --> measures the buffer lenght used to reduce jitter (expressed in ms)
- SNR (Signal-to-Noise ratio) --> measures the objective quality of the communication channel (expressed in dB)

*Example:*

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
