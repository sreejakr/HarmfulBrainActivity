# HarmfulBrainActivity
Harmful Brain Activity Exploratory Data Analysis

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

For now I have done some first stage analysis on only the EEG data. 

This recording is non-invasive (taken over the scalp) and follows the 10-20 electrode placement system. The electrodes given in the dataset are placed in the following way. 
I have also mentioned the names of the functions associated with the output.

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




