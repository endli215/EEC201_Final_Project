# EEC201_Final_Project
Team Golf: Ender Li, Sebastian Diaz 

### Abstract (Seb)
This project explores the implementation of speech recognition using Mel Cepstrum coefficients and the Linde-Buzo-Gray (LBG) clustering algorithm. We detail the step-by-step process of computing Mel Cepstrum coefficients, implementing the LBG algorithm, and integrating both techniques for effective speech recognition. The system is trained on a basic dataset and tested to evaluate its performance, achieving an accuracy of X%. Our results demonstrate the feasibility of using Mel Cepstrum features and LBG clustering for speech recognition, providing insights into its effectiveness.



### Run Instructions

Link to the Colab project: https://colab.research.google.com/drive/1hs1_b0xaXeUPWici3pcfugvv-dD5AyN2?usp=sharing

To run the project, you can click on the link below, and it will show all the results in graph and text. If there is any error showing up, click "Runtime" => "Disconnect and delecet runtime" => "Run all" If uthe ser wants to run an individual function, make sure to run the initialization first, and make sure to run```!git clone https://github.com/endli215/EEC201_Final_Project.git``` and ```%cd EEC201_Final_Projec``` only one time

### Audio File in this github
In this Github repository, there is mostly audio for training and testing audio that is used to match the training audio. In the initial setup for the program, Training_data and Test_data are used to get the software working. Then, Training_Data_ext and Test_data_ext (extended data) are used to test the accuracy. For the Eleven, Zero, Five, Twelve is used for speaker and speech recognition. In Google Colab, ```!git clone https://github.com/endli215/EEC201_Final_Project.git``` and ```%cd EEC201_Final_Projec``` are used to clone all the audio files to Colab and change the directory to Project. All the audio files have the same name throughout the test and training. Its SX.wave and X are identifiers of each of the speakers; for example, S1.wav in Five_Training is saying Speaker 1 is saying the word Five, and it is the same speaker of S1.Wav in Five_test.
### Speech Processing and Feature
Speech signals are considered quasi-stationary, meaning that when analyzed using the Short-Time Fourier Transform (STFT) with a sufficiently short frame length, their frequency characteristics remain relatively stationary. While a spectrogram may provide some insight into the spoken content, it is generally insufficient for speaker recognition. Given that humans can possess and distinguish voices, signal processing techniques can be employed to emulate this perceptual capability. One such approach is Mel frequency wrapping, which applies a filterbank designed to reflect the human auditory system's sensitivity to different frequencies. The filtered signal is then used to compute Mel-Frequency Cepstral Coefficients (MFCCs), which serve as a robust representation for both speech and speaker characterization.

<div align="center">
  <img src="https://github.com/user-attachments/assets/1b4cd0fc-38e5-416e-ae89-0c4837229ce8" alt="Figure 1" width="2000"/>

Figure 1: Block Diagram of the feature to get the MFCC
</div>
According to Figure 1, the initial step in the process involves generating the spectrogram of the speech signal. To achieve this, we first imported the audio file from GitHub and plotted it in the time domain. This visualization provides insight into the characteristics of the signal. As illustrated in Figure 2, the left-hand side displays both the original real-time audio signal and its corresponding spectrogram. From the real-time signal, it is evident that for the majority of the time, the amplitude remains low and nearly stationary. The presence of stationary input can make it challenging to distinguish variations in the spectrogram.<br />

For instance, in audio samples such as S12.wav and S11.wav, removing the stationary portions of the signal significantly enhanced the distinctiveness of the spectrogram. Consequently, before proceeding with frame blocking and windowing for spectrogram generation, we applied a pre-processing step to remove segments where the signal amplitude remained below 5% of the peak value for  durations exceeding 0.2 seconds. This ensured that only the portions containing audible speech were retained for speech recognition analysis.
To compute the spectrogram, we utilized the ```scipy.signal.stft``` function in Python. This function allows the selection of key parameters, including the frame size N, the number of overlapping samples N−M, the type of windowing function, and the number of FFT points. In our second test, we set N=256N, number of overlapped samples= 156 (N−M=256-100), a Kaiser window, and 512 FFT points. The function outputs the Short-Time Fourier Transform (STFT) of the time-domain signal, which we then used to generate the spectrogram, as shown in Figure 2.<br />

In subsequent tests, we also computed the STFT manually by implementing frame blocking, windowing, and the Fast Fourier Transform (FFT) without relying on ```scipy.signal.stft```. The results obtained through this approach closely matched those generated using the built-in function, demonstrating the validity of our methodology. 
<div align="center">
  <img src="https://github.com/user-attachments/assets/4c564ba8-bbf8-4c9d-a8a6-a921f4fed033" alt="Figure 1" width="2000"/>

Figure 2 Original Audio signal and Spectrogram Vs Trimed signal and Spectrogram


</div>

### Mel Spectrum and Cepstrum （Seb）

### Vector Quantization(LBG)

### Test on Testing and Training Data

### Notch Filter (Seb)

### Speaker and Speech recognition 

### Result
