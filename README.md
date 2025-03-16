# EEC201_Final_Project
Team Golf: Ender Li, Sebastian Diaz 

### Abstract (Seb)
This project explores the implementation of speech recognition using Mel Cepstrum coefficients and the Linde-Buzo-Gray (LBG) clustering algorithm. We detail the step-by-step process of computing Mel Cepstrum coefficients, implementing the LBG algorithm, and integrating both techniques for effective speech recognition. The system is trained on a basic dataset and tested to evaluate its performance, achieving an accuracy of 71%. Our results demonstrate the feasibility of using Mel Cepstrum features and LBG clustering for speech recognition, providing insights into its effectiveness.



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

Test 4 shows the completed function to compute the Mel Cepstrum Coefficients of the speech signal. Here we give an explanation of how the code works:
#### 1. Windowing the Speech Signal
Speech is a non-stationary signal, meaning its characteristics change over time. To analyze it effectively, we segment it into short overlapping frames. Each frame is typically 5–100 ms long, with 50% overlap between consecutive frames to ensure smooth transitions. To minimize spectral leakage, we apply a Hamming window to each frame. We use the numpy function `np.hamming(n)` to create a hamming filter. Where \(n\) is the frame length. This step reduces discontinuities at the frame edges.

#### 2. Computing the Spectrum
After windowing, we apply the Fast Fourier Transform (FFT) to each frame to convert the signal from the time domain to the frequency domain. We then compute the magnitude spectrum by taking the magnitude of the FFT: `X(k) = |FFT(x(n))|` where `X(k)` represents the magnitude spectrum and `x(n)` is the windowed speech signal.

#### 3. Mel Frequency Wrapping
The human auditory system perceives frequency non-linearly, with higher sensitivity to lower frequencies. To model this, we use a set of Mel filter banks, which consist of overlapping triangular filters spaced according to the Mel scale. 


- Below 1000 Hz, filters are linearly spaced.
- Above 1000 Hz, filters are logarithmically spaced.
<div align="center">
<img width="844" alt="Screenshot 2025-03-15 at 5 25 38 PM" src="https://github.com/user-attachments/assets/cb78a8f9-3b76-4713-891b-9087b6423e8d" />

Figure 3 :Plot of Mel Filter Banks

</div>

Each filter sums the energy in its respective frequency range, transforming the linear frequency spectrum into the Mel frequency spectrum. To create the Mel filter banks we use the function `librosa.filters.mel` from the `librosa` library. We then normalize all the triangles to have a maximum value of 1. 

We then apply the Mel filter banks to every window to get the Mel spectrum. By the end of the calculation we will have a matrix of MxN where M is the number of windows from the original signal and N is the amount of Mel filter banks. 

#### 4. Logarithm and Discrete Cosine Transform (DCT)
Next, we take the logarithm of the Mel-filtered energies. This step aligns with human auditory perception, where loudness is perceived logarithmically rather than linearly.

Finally, we compute the Discrete Cosine Transform (DCT) to obtain the Mel Cepstrum Coefficients (MFCCs). The DCT removes correlations between the Mel-scaled features and produces a compact representation. The equation for DCT is shown below

<img width="673" alt="Screenshot 2025-03-15 at 3 36 03 PM" src="https://github.com/user-attachments/assets/76856d35-f05f-4b8e-83a0-a64b5e9c295f" />

where `k` is the number of Mel filter banks, and `c_n` represents the `n`-th cepstral coefficient. We use the function `dct()` to compute the Discrete Cosine Transform for use. 

We calculate 20 coefficients but remove the first coefficient to eliminate the influence of overall loudness. This ensures that our model captures only the distinct features of speech, rather than variations in volume. The output of our code is MxN where M is the amount of windows when window framing and N is the Mel Cepstrum Coefficients.
<div align="center">
<img width="538" alt="Screenshot 2025-03-15 at 5 29 42 PM" src="https://github.com/user-attachments/assets/9b602bc0-2c2b-46a5-8037-22324e4a8420" />
  
Figure 4: Plot of Mel Cepstrum Coefficients
</div>

### Vector Quantization(LBG)
In our speech recognition, we have used the Linde-Buzo-Gray (LBG) algorithm, which is a well-known method for vector quantization. Vector quantization is a process used in signal processing and data compression to represent high-dimensional data with a limited set of representative vectors called centroids or codebook vectors. The goal of the LBG algorithm is to partition the data into clusters and optimize the centroids such that the distortion (or quantization error) between the data points and their assigned centroids is minimized. The def function is called ```lbg_vector_quantization(num_centroids, cepstrum, esp)``` it can customize the number of centriods the function is outputting, and we can customize each esp in different cases to get the maximum accuracy, which is excellent.

The process begins with the initialization of a codebook, typically containing a single centroid, and this codebook is then iteratively split to create more centroids until the desired number is reached(So the number of centroids is always 2^N). During the assignment step, each data point is assigned to the centroid closest to it. Following this, the update step recalculates the centroids by computing the mean of the data points that are assigned to each centroid. The algorithm continues this process until convergence is achieved, which occurs when the distortion between consecutive iterations becomes sufficiently small, indicating that the centroids have stabilized, as shown in Figure 3 below as a block diagram.

<div align="center">
  
  <img src="https://github.com/user-attachments/assets/bbb97aaa-6a11-4098-903c-8d00056c7a38" alt="Figure 1" width="500"/>
  
  Figure 5: Flow diagram of LBG algorithm (Adopted from Rabiner and Juang, 1993)
  
</div>
To visualize the centroids and the cepstrum, we have plotted them in a two-dimensional (2D) scatter plot. The cepstrum and centroid are originally in a 19-dimensional (19D) space, as we utilized 19 Mel-frequency cepstral coefficients (MFCCs) from the Mel filter bank. Figure 4 presents a scatter plot of MFCC-4 and MFCC-5, which has been selected to create the 2D representation. Each speaker in the dataset is represented by four centroids, which correspond to the cluster points associated with each individual speaker. These centroids are crucial for later applications, including speech and speaker recognition, as they serve as distinct markers for identifying and differentiating speakers based on their acoustic features.

<div align="center">

  <img src="https://github.com/user-attachments/assets/d4279434-b817-439a-b9ba-619e57d297c0" alt="Figure 1" width="750"/>
  
  Figure 6: Scatter plot and centroid of mfcc-4 and mfcc-5
  
</div>

### Notch Filter (Seb)
We can apply a notch filter at different frequencies to either improve or hinder the accuracy of our speech recognition algorithm:  

- Improvement: By removing unwanted noise in the speech signals, the notch filter can enhance accuracy.  
- Hindrance: If key frequencies that distinguish speech files are removed, recognition accuracy may decrease.  

To evaluate the effect of the notch filter, we compute the accuracy of the speech recognition algorithm after filtering the signal. The process remains largely the same:  

1. Apply the notch filter to the speech signal before further processing.  
2. Compute the Mel Cepstrum Coefficients (MFCCs) as described earlier.  
3. Perform LBG clustering to obtain centroids.  
4. Evaluate the recognition accuracy using the filtered speech data.  

To implement the notch filter, we modify the existing code from Test 4, which computes the cepstrum coefficients. The key difference is that we add code to filter the speech signal before computing MFCCs. To implement the notch filter we use the function `iirnotch` from `scipy.signal` to create the notch filter. We then use `filtfilt` function from the same library to apply our notch filter that we generated to our speech signal. The steps for Mel Spectrum and Cepstrum computation remain unchanged and are described in the section **Mel Spectrum and Cepstrum**.

### Speaker and Speech recognition 
After the LBG algorithm processes the speech of each individual speaker, it generates a unique centroid for each speaker’s voice. In the context of speech and speaker recognition, the generated centroid can be compared to centroids from other speakers. Since each centroid represents the distinct acoustic features of a particular speaker, we can compute the distance between the centroids of different speakers and their respective speech samples. The centroid with the minimum distance to the test sample will correspond to the recognized speaker and the speech. This approach leverages the uniqueness of each centroid, ensuring that the correct speaker and speech are identified based on the closest match in centroid space.
### Result
