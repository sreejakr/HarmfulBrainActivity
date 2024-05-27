<img width="796" alt="Screenshot 2024-05-27 at 14 43 02" src="https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/9061f9b4-fd0e-41dd-9b98-a77cf517629d"># Harmful Brain Activity Exploratory Data Analysis

Dataset - https://www.kaggle.com/competitions/hms-harmful-brain-activity-classification/overview

Description of the dataset (as mentioned in the above website)

{ From stethoscopes to tongue depressors, doctors rely on many tools to treat their patients. Physicians use electroencephalography with critically ill patients to detect seizures and other types of brain activity that can cause brain damage. Currently, EEG monitoring relies solely on manual analysis by specialized neurologists. While invaluable, this labor-intensive process is a major bottleneck. Not only can it be time-consuming, but manual review of EEG recordings is also expensive, prone to fatigue-related errors, and suffers from reliability issues between different reviewers, even when those reviewers are experts.

Competition host Sunstella Foundation was created in 2021 during the COVID pandemic to help minority graduate students in technology overcome challenges and celebrate their achievements. These students are vital to America's technology leadership and diversity. Through workshops, forums, and competitions, the Sunstella Foundation provides mentorship and career advice to support their success.

Automating EEG analysis will help doctors and brain researchers detect seizures and other types of brain activity that can cause brain damage, so that they can give treatments more quickly and accurately. The algorithms developed in this contest may also help researchers who are working to develop drugs to treat and prevent seizures.

There are six patterns of interest for this competition: seizure (SZ), generalized periodic discharges (GPD), lateralized periodic discharges (LPD), lateralized rhythmic delta activity (LRDA), generalized rhythmic delta activity (GRDA), or “other”. }

# Files
train.csv Metadata for the train set. The expert annotators reviewed 50 second long EEG samples plus matched spectrograms covering 10 a minute window centered at the same time and labeled the central 10 seconds. Many of these samples overlapped and have been consolidated. train.csv provides the metadata that allows you to extract the original subsets that the raters annotated.

eeg_id - A unique identifier for the entire EEG recording.
eeg_sub_id - An ID for the specific 50 second long subsample this row's labels apply to.
eeg_label_offset_seconds - The time between the beginning of the consolidated EEG and this subsample.
spectrogram_id - A unique identifier for the entire EEG recording.
spectrogram_sub_id - An ID for the specific 10 minute subsample this row's labels apply to.
spectogram_label_offset_seconds - The time between the beginning of the consolidated spectrogram and this subsample.
label_id - An ID for this set of labels.
patient_id - An ID for the patient who donated the data.
expert_consensus - The consensus annotator label. Provided for convenience only.
[seizure/lpd/gpd/lrda/grda/other]_vote - The count of annotator votes for a given brain activity class. The full names of the activity classes are as follows: lpd: lateralized periodic discharges, gpd: generalized periodic discharges, lrd: lateralized rhythmic delta activity, and grda: generalized rhythmic delta activity . A detailed explanations of these patterns is available here.
test.csv Metadata for the test set. As there are no overlapping samples in the test set, many columns in the train metadata don't apply.

eeg_id
spectrogram_id
patient_id
sample_submission.csv

eeg_id
[seizure/lpd/gpd/lrda/grda/other]_vote - The target columns. Your predictions must be probabilities. Note that the test samples had between 3 and 20 annotators.
train_eegs/ EEG data from one or more overlapping samples. Use the metadata in train.csv to select specific annotated subsets. The column names are the names of the individual electrode locations for EEG leads, with one exception. The EKG column is for an electrocardiogram lead that records data from the heart. All of the EEG data (for both train and test) was collected at a frequency of 200 samples per second.

test_eegs/ Exactly 50 seconds of EEG data.

train_spectrograms/ Spectrograms assembled EEG data. Use the metadata in train.csv to select specific annotated subsets. The column names indicate the frequency in hertz and the recording regions of the EEG electrodes. The latter are abbreviated as LL = left lateral; RL = right lateral; LP = left parasagittal; RP = right parasagittal.

test_spectrograms/ Spectrograms assembled using exactly 10 minutes of EEG data.

example_figures/ Larger copies of the example case images used on the overview tab.


-------------------------------------------------------------------------------------

# Labeling Method

It is mentioned in the dataset description that "*The expert annotators reviewed 50 second long EEG samples plus matched spectrograms covering 10 a minute window centered at the same time and labeled the central 10 seconds.*" 
Hence, I created a dataset where for each subject we extracted the middle 15 seconds between the offset to 50 seconds data.
The term "offset" is the given starting point of that particular EEG recording which is mentioned in the train metadata file.
For a 50-second window starting from the offset, the midpoint is calculated.

Suppose the offset is 0, midpoint of the 0 to 50-second range is at 25 seconds (50/2).
To capture the middle 15 seconds around this midpoint, we extract data from 17.5 seconds to 32.5 seconds. This ensures that we have a consistent 15-second segment centered around the midpoint.

I have taken 15 seconds and not just 10 seconds so that we have a little more information of the signal.

    # Extract the central 15-second data from the specified 50-second window
    # Since the data is recorded at 200 samples/second, 15 seconds correspond to 3000 samples
    # The central 15 seconds means skipping the first 17.5 seconds (3500 samples) from the 50-second window start



# Analysis

data_50.csv - This file contains only 50 subjects worth of raw data. We will be using only this data for analysis. It has been extracted based on unique EEG IDs.

This recording is non-invasive (taken over the scalp) and follows the 10-20 electrode placement system. The electrodes given in the dataset are placed in the following way. 
I have also mentioned the names of the functions associated with the output.

<img width="341" alt="image" src="https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/db564e68-226f-44f0-82e9-eae49ca1b0a8">

bads: This typically refers to channels marked as bad or noisy during data preprocessing.
ch_names: This is a list of EEG channel names, such as EEG Fp1, EEG F3, EEG C3, etc.
chs: This indicates the number of EEG channels (35 EEG channels).
custom_ref_applied: This specifies whether a custom reference has been applied to the data (False in this case).
highpass: This indicates the high-pass filter frequency applied to the EEG data (1.6 Hz).
lowpass: This indicates the low-pass filter frequency applied to the EEG data (30.0 Hz).
meas_date: This is the date and time when the EEG data was measured (January 1, 2016, at 19:39:33 UTC).
nchan: This specifies the total number of channels (35 channels).
projs: This typically refers to any signal processing projects applied to the data.
sfreq: This is the sampling frequency of the EEG data (512.0 Hz).
subject_info: This provides information about the subject, usually stored as a dictionary.

function - plot_montage():
<img width="648" alt="Screenshot 2024-05-26 at 14 47 45" src="https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/f21bc9a7-ab9d-47a7-ad20-a7d0f0c58c26">

function - plot_eeg_for_each_category()
Plots of sample EEGs for each category
![WhatsApp Image 2024-03-22 at 1 01 41 AM](https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/37a691a5-cc81-4fe1-a4cd-30a5d5ea2214)
![WhatsApp Image 2024-03-22 at 1 00 45 AM](https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/b829d300-818b-4bf0-b121-9c600ca6cbf3)
![WhatsApp Image 2024-03-22 at 12 59 53 AM](https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/e8140371-2116-4f49-aa41-52739211879a)
![WhatsApp Image 2024-03-22 at 12 59 27 AM](https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/08308322-3604-4ee4-bf41-1ab84d836948)
![WhatsApp Image 2024-03-22 at 12 58 59 AM](https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/743099bd-b0ca-4858-80b9-6bb145061eaf)
![WhatsApp Image 2024-03-22 at 12 58 24 AM](https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/2bc36d02-3fab-40ce-80e1-baa1d2692e2a)

------------------------------------------------------------------
function - plot_heatmap_for_one_file()
Function to plot heatmaps of a sample EEG for each category. 
![WhatsApp Image 2024-03-22 at 12 48 59 AM](https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/4b5967dc-80c2-411d-8eaa-51773bb3b57f)
![WhatsApp Image 2024-03-22 at 12 49 48 AM](https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/8a643e7b-3a8e-4a2d-abb0-30cccd130e03)
![WhatsApp Image 2024-03-22 at 12 50 34 AM](https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/919da5c7-f387-4af7-ad86-158b4968f12d)

These are the heatmaps generated from EEG data - Here we considered the first occurrence of any category and used the expert consensus in the same row for analysis.

Warmer colors (yellows, reds) usually indicate higher electrical activity or signal intensity. Cooler colors (blues, purples) represent lower activity.

Seizure: The heatmap shows a very uniform color with very subtle variations in intensity, indicating low amplitude fluctuations across the scalp. The signal intensity scale goes from -150 to 150 µV, which may suggest broad but not highly localized activity typical of certain types of seizures.
GPD: This heatmap has more variation with localized regions of increased activity, indicated by the yellow and green areas. The signal intensity scale is much narrower, from -20 to 20 µV, which suggests a less intense activity than the seizure category.
LRDA: Here, the activity is also more localized than in the seizure category, with the signal intensity ranging from -30 to 30 µV. The activity is lateralized, meaning it is predominantly on one side, which is characteristic of LRDA.
Other: This category represents a type of activity that does not fit into the other specified categories.
GRDA: Similar to LRDA, GRDA indicates rhythmic delta activity, but it's generalized across the scalp rather than being lateralized. The scale shows a range from -60 to 60 µV, and there are two main areas of increased activity.
LPD: This heatmap shows very defined areas of increased activity that are lateralized, typically characteristic of LPD. The range is from -60 to 40 µV, indicating significant variability in the localized brain regions.
Comparing the Heatmaps:

Scale Variations: The intensity scales vary, indicating different amplitudes of EEG activity, which could correspond to the severity or type of the conditions.
Pattern Variations: Seizure activity tends to be more global and less localized than the other categories. In contrast, GPD, LRDA, GRDA, and LPD show localized hotspots, which suggests specific brain regions are more active or affected
Localization: LRDA and LPD indicate lateralized activity, while GRDA is generalized. GPD can be either localized or more generalized, and the 'Other' category is too nonspecific without additional context.

------------------------------------------------------------------

function - plot_scalp_heatmaps_for_all_data()

![WhatsApp Image 2024-03-22 at 12 48 59 AM](https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/b9b09a3c-f74f-4728-98f9-070f49155f80)

![WhatsApp Image 2024-03-22 at 12 49 48 AM](https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/a6c8d9da-43f3-4675-8f35-afd2e6223424)

![WhatsApp Image 2024-03-22 at 12 50 34 AM](https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/6f695b8a-b8f0-4f2f-b61d-ce566579ab58)

Second part: For 50 subjects worth of data Here's a brief analysis of each category based on the heatmaps:

Category: Other - The distribution is relatively uniform, with a slight intensity increase at the top. This might indicate normal brain activity or a baseline EEG without specific focal features.
Category: LRDA (Localized Rhythmic Delta Activity) - There is a prominent area of increased intensity in the frontal region. LRDA often correlates with localized brain dysfunction, which could be reflected in the frontal lobe here.
Category: Seizure - Intense activity is observed at the top of the scalp. In the context of seizures, such a pattern could represent focal onset, with the possibility of the seizure activity spreading from that region.
Category: LPD (Lateralized Periodic Discharges) - This map shows a very focal area of high intensity, likely on the left side of the brain. LPDs are typically associated with acute or chronic focal brain dysfunction and can be observed in various clinical scenarios, including seizures.
Category: GRDA (Generalized Rhythmic Delta Activity) - The intensity is increased in the temporal regions, particularly on the right side. GRDA is often associated with widespread cerebral dysfunction and can sometimes be observed in encephalopathies or deep sleep stages.
Category: GPD (Generalized Periodic Discharges) - There's a generalized pattern with a mix of intensities, but without a clear focal point. GPDs can be seen in various clinical conditions and reflect a range of neural synchronization levels.

------------------------------------------------------------------
The below analysis is for a sample seizure EEG and a non-seizure EEG file. 

<img width="240" alt="image" src="https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/44dfdf6d-9d77-4c76-ae93-ba372147a344">

Normal State: The heatmap for the normal state shows a relatively uniform and balanced distribution of electrical activity across the scalp. 
Seizure State: The heatmap for the seizure state likely shows areas of heightened activity, which may represent the focal point or general distribution of seizure activity. The areas with increased intensity can suggest where in the brain the seizure may be occurring or spreading.

The "normal" state seems to have a lower overall EEG signal intensity compared to the "seizure" state, which is consistent with the increased electrical activity during a seizure.

------------------------------------------------------------------

<img width="477" alt="image" src="https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/b07c7ec1-2832-4f1d-926a-4f4aa6e72e68">

This is the entire dataset plotted against each sample. 
The seizure is present somewhere between 36235768th and 37579773th sample.
There are noticeable spikes in the amplitude at certain points. These spikes are particularly pronounced around the times marked as seizures, which indicates the presence of abnormal, high-amplitude electrical activity before and especially after the seizure occurs.
The seizure periods show a more erratic and heightened activity.


# Spectrogram Analysis

The spectogram file has 1136 rows and 401 columns. The column names indicate the frequency in hertz and the recording regions of the EEG electrodes.

LL = left lateral
RL = right lateral
LP = left parasagittal
RP = right parasagittal
![1](https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/757f9698-0489-4621-9875-3868e2198a60)

![2](https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/322d79fa-738e-434f-9178-5aad39935b24)

![3](https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/ef3bf86e-f297-4e52-b5b6-ea78d1acd65c)

![4](https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/a1cd059a-b043-4a2c-ac0a-324006318960)

![5](https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/5485c6f5-c211-4507-b2e9-0a905cf75085)

General Observations Each heatmap represents spectral data over time, with different frequency components plotted against time intervals. There are four heatmaps per category, representing different channels or data streams labeled LL (Lower Left), RL (Lower Right), RP (Upper Right), and LP (Upper Left). Each heatmap has a color scale indicating the magnitude of the spectral components, with purple indicating lower values and yellow higher values.

Specific Observations

Category: LPD - The LL and RP heatmaps show a nearly uniform distribution without significant variations over time. RL and LP heatmaps also display uniformity, but there seems to be a slight variance with yellow lines indicating some events or noise.
Category: GRDA - These heatmaps are rich in detail, with the frequency components varying significantly over time. The yellow and cyan streaks indicate periods with higher magnitude frequency components, suggesting activity at specific times.
Category: Other - This category shows a very distinct pattern with almost consistent lines across all channels, indicating regular occurrences or periodic events.
Category: LRDA - These heatmaps seem to show sparse activity with some isolated peaks, possibly indicating infrequent or transient events.
Category: GPD - The patterns here suggest sporadic activity with periods of higher frequency components. The LL heatmap appears to have less activity compared to the others.
Category: Seizure The heatmaps show irregular patterns of spectral activity, which may correspond to the unpredictable nature of seizures.
Summary :

Stability: The LPD category shows stable conditions, possibly a baseline or control state.
Activity: GRDA and GPD categories exhibit more active states with distinct spectral events.
Periodicity: The "Other" category has regular and periodic patterns. Transience: LRDA shows transient spectral events.
Irregularity: Seizure category likely represents pathological or irregular brain activity, given the nature of the data and the labeling.
The presence of yellow and other lighter colors near near the lower frequencies on the heatmap suggests more activity or higher magnitudes at these lower frequencies.

Conclusion: We can eliminate the columns that have higher frequencies in order to reduce dimensionality

------------------------------------------------------------------
The below analysis is for a sample seizure EEG and a non-seizure EEG file. 

![image](https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/3ce814fc-4eb5-42b7-80d4-31aea1299bbd)

![image](https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/3c16f22c-7cd8-4c39-b3cc-9660ad601f9e)

Normal State - The color intensity (representing signal power) more power in lower frequencies, less power in higher frequencies have less power.
Seizure State: There is a clear yellow band in the lower frequency range, indicating a higher power in this frequency range during a seizure. There is more power in higher frequencies as well. 



------------------------------------------------------------------

# Statistical Analysis


<img width="953" alt="Screenshot 2024-05-26 at 16 18 46" src="https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/864232f4-37a6-4f2e-be79-4fb68770c437">

<img width="1245" alt="Screenshot 2024-05-26 at 16 19 00" src="https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/26fc9301-57c7-4e85-9e89-b72c00e6a0f7">

<img width="1235" alt="Screenshot 2024-05-26 at 16 19 09" src="https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/30fa2e3d-c09c-4010-9862-21b14e2925d2">

<img width="1237" alt="Screenshot 2024-05-26 at 16 19 21" src="https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/663ce8f7-5af9-411b-9f07-a07cf37c2a94">

<img width="1237" alt="Screenshot 2024-05-26 at 16 19 32" src="https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/36ed4807-8a84-4f08-ba2f-45d6eb055470">

<img width="1237" alt="Screenshot 2024-05-26 at 16 19 32" src="https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/609c161c-cd1a-4d8f-81b0-1eef942c377d">

<img width="1237" alt="Screenshot 2024-05-26 at 16 19 41" src="https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/b16caac5-1af4-48b2-bebe-e30a1391f31e">


------------------------------------------------------------------


## Statistical Analysis between Seizure and Non-Seizure 

The below analysis is for a sample seizure EEG and a non-seizure EEG file. 

1) Mean
Channels that have higher mean values during seizure states might indicate increased electrical activity, while lower mean values could suggest decreased activity or suppression during seizures.
In Fc5, Cz, Fp2, O2, T4, Cp6, F10 we can observe a clear difference between the means. This could mean there is a high chance that a negative value here would indicate a seizure.

   <img width="562" alt="image" src="https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/72d72d59-7e8a-449c-a954-08ffc75d023b">

2) Median
   
   <img width="662" alt="image" src="https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/572b4c2d-d014-4e6a-9839-a194f4a2ed12">

4) Standard Deviation
   
   <img width="582" alt="image" src="https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/b416ee16-a950-4577-9ab5-c15643c20803">
   
   Standard deviation is a measure of the amount of variation or dispersion in a set of values.
Channels with significant differences in standard deviation between states could indicate regions more affected by seizures or more active in general.
Channels show a higher standard deviation in the normal state compared to the seizure state, it could imply that during seizures, the electrical activity becomes more uniform, possibly due to the synchronized firing of neurons that is characteristic of some types of seizures.

5) Variance
   
   <img width="686" alt="image" src="https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/42dad18d-59b2-4b92-9d2a-14b1823fee34">
   
   Variance measures how far a set of numbers is spread out from their average value. In the context of EEG data, a higher variance could indicate more variability in the EEG signal amplitude during that state.

6) Kurtosis
   
   Seizure data has lesser Kurtosis. This would suggest a distribution that has lighter tails and a less extreme range of outlier values.
   
   <img width="688" alt="image" src="https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/c0fabdc4-e20a-4e63-bb94-b5955aacc5fb">

8) Skewness
   
   <img width="654" alt="image" src="https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/1e628414-754b-4047-aa72-9da6d0899092">
   
   Positive Skewness: Indicates a distribution with an asymmetric tail extending towards more positive values. In EEG data, this might suggest more frequent high-amplitude waveforms.
Negative Skewness: Indicates a distribution with an asymmetric tail extending towards more negative values. For EEG, this might suggest more frequent low-amplitude waveforms.
Most of the seizure data seems to have a negative skewness.

------------------------------------------------------------------

# Report of Sample EEGs

1) Function - analyze_eeg(raw)

NORMAL REPORT
Creating RawArray with float64 data, n_channels=19, n_times=10000
    Range : 0 ... 9999 =      0.000 ...    49.995 secs
Ready.
Filtering raw data in 1 contiguous segment
Setting up band-pass filter from 0.5 - 40 Hz

FIR filter parameters
---------------------
Designing a one-pass, zero-phase, non-causal bandpass filter:
- Windowed time-domain design (firwin) method
- Hamming window with 0.0194 passband ripple and 53 dB stopband attenuation
- Lower passband edge: 0.50
- Lower transition bandwidth: 0.50 Hz (-6 dB cutoff frequency: 0.25 Hz)
- Upper passband edge: 40.00 Hz
- Upper transition bandwidth: 10.00 Hz (-6 dB cutoff frequency: 45.00 Hz)
- Filter length: 1321 samples (6.605 s)


Duration: 49.99 seconds
Sampling Frequency: 200.0 Hz
Number of Channels: 19
Channel Names: Fp1, F3, C3, P3, F7, T3, T5, O1, Fz, Cz, Pz, Fp2, F4, C4, P4, F8, T4, T6, O2

Date of Acquisition: Unknown
Noise-to-Signal Ratio (NSR): nan
Lowpass Filter Frequency: 40.0
Highpass Filter Frequency: 0.5

Background Activity:
-------------------
Dominant Rhythm: delta
Dominant Power: 14.08
Symmetry Score: 0.0537


Abnormal Features:
------------------
Detected 2859 spike events.
Detected 2985 sharp wave events.


Relative Power in Frequency Bands:
---------------------------------
Delta band relative power: 2.57
Theta band relative power: 0.44
Alpha band relative power: 0.34
Beta band relative power: 0.42
Power in Frequency Bands:
---------------------------------
Delta band relative power: 14.08
Theta band relative power: 2.14
Alpha band relative power: 1.57
Beta band relative power: 1.92

------------------------------------------------------------------

# Spike Graph

<img width="993" alt="Screenshot 2024-05-26 at 16 43 50" src="https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/e42a9263-aada-43d1-a58f-73d32bb520ea">

As expected, the spike activity for seizure is more than non-seizure.

------------------------------------------------------------------
# Correlation Matrix 

<img width="639" alt="Screenshot 2024-05-26 at 18 54 26" src="https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/6a86990c-157c-4742-9a84-5abd93d8c9f0">

From the correlation matrix above for the vote columns we can observe that other_vote, grda_vote and lrda_vote have significant correlation

------------------------------------------------------------------

# Filtering Methods

Butterworth Lowpass Filter:
Function: butter_lowpass_filter(data, cutoff_freq=20, sampling_rate=200, order=4)
Purpose: This filter is designed to allow signals with a frequency lower than the cutoff frequency (20 Hz) to pass through while attenuating (reducing the amplitude of) signals with frequencies higher than the cutoff.
Process:
The signal is passed through a Butterworth lowpass filter, which is known for having a smooth frequency response.
The cutoff frequency is set to 20 Hz, meaning frequencies above 20 Hz will be significantly attenuated.
This is typically done to remove high-frequency noise and artifacts that are not of interest for EEG analysis.

Median Filter:
Function: median(signal)
Purpose: This filter is used to remove spikes or short-term noise from the signal. It is particularly effective for removing salt-and-pepper noise.
Process:
The signal is processed with a median filter of kernel size 3.
For each point in the signal, the median value of it and its neighbors (within the kernel size) is computed, and the point is replaced by this median value.
This helps in preserving the edges while removing short-term noise.

Wavelet Denoising:
Function: denoise(signal, wavelet="db8")
Purpose: Wavelet denoising is used to remove noise while preserving the signal characteristics. It's particularly useful for non-stationary signals like EEG.
Process:
The signal is decomposed into wavelets using the pywt library with the Daubechies 8 (db8) wavelet.
The detail coefficients (high-frequency components) are thresholded to remove noise.
The signal is then reconstructed from the thresholded coefficients.
This method helps in effectively removing noise without significantly distorting the signal.
The filtered signals are then plotted alongside the original signal for comparison.

The plots are done using the plotly function for each of the channels. You will be able to zoom in and see the values before and after applying the filters mentioned above. 

<img width="1389" alt="Screenshot 2024-05-27 at 14 42 30" src="https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/79ad1714-b912-4d8e-aa9c-8651e655efd8">

<img width="796" alt="Screenshot 2024-05-27 at 14 43 02" src="https://github.com/sreejakr/HarmfulBrainActivity/assets/58878572/5b3e3544-3395-47fc-ac65-a10395eb871f">





