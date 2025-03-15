# EEC201_Final_Project
Team Golf: Ender Li, Sebastian Diaz 

### Abstract (Seb)




### Run Instructions

Link to the Colab project: https://colab.research.google.com/drive/1hs1_b0xaXeUPWici3pcfugvv-dD5AyN2?usp=sharing

To run the project, you can click on the link below, and it will show all the results in graph and text. If there is any error showing up, click "Runtime" => "Disconnect and delecet runtime" => "Run all" If uthe ser wants to run an individual function, make sure to run the initialization first, and make sure to run```!git clone https://github.com/endli215/EEC201_Final_Project.git``` and ```%cd EEC201_Final_Projec``` only one time

### Audio File in this github
In this Github repository, there is mostly audio for training and testing audio that is used to match the training audio. In the initial setup for the program, Training_data and Test_data are used to get the software working. Then, Training_Data_ext and Test_data_ext (extended data) are used to test the accuracy. For the Eleven, Zero, Five, Twelve is used for speaker and speech recognition. In Google Colab ```!git clone https://github.com/endli215/EEC201_Final_Project.git``` and ```%cd EEC201_Final_Projec``` are used to clone all the audio files to Colab and change the directory to Project. All the audio files have the same naming invention throughout all the test and training. Its SX.wave and X is identifiers of each of the speakers, for example, S1.wav in Five_Training is saying Speaker 1 is saying the word Five and it is the same speaker of S1.Wav in Five_test.
### Speech Processing and Feature
Speech signals are quasi-stationary, meaning that when analyzed using the Short-Time Fourier Transform (STFT) with a sufficiently short frame, their frequency characteristics remain mostly Stationary. Sometimes, from the spectrogram, you might be  able to recognize the word, but it is not sufficient enough for speaker recognition. Since humans have a good ear to recognize other human voices, we can apply filters to emulate what the human ear can recognize at certain frequencies(Mel frequency wrapping) and use the filtered signal to compute MFCC (Mel-Frequency Cepstrum Coefficients) to further more characterize the speaker and speech.

<div align="center">
  <img src="https://github.com/user-attachments/assets/1b4cd0fc-38e5-416e-ae89-0c4837229ce8" alt="Figure 1" width="2000"/>

Figure 1: Block Diagram of the feature to get the MFCC


  <img src="https://github.com/user-attachments/assets/4c564ba8-bbf8-4c9d-a8a6-a921f4fed033" alt="Figure 1" width="2000"/>

Figure 2


</div>

### Mel Spectrum and Cepstrum （Seb）

### Vector Quantization(LBG)

### Test on Testing and Training Data

### Notch Filter (Seb)

### Speaker and Speech recognition 

### Result
