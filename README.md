# wifi-qoe
QoE score prediction based on WiFi KPIs

## 1. Introduction
Wifi has been the go to connectivity network for the users, smart devices within a home environment. Devices in homes are increasingly connected, generating higher network traffic and requiring exponentially higher bandwidth, lower latency and reliability. End users are demanding more of these applications as applications like AR/VR, cloud gaming, streaming, video conferncing are being adopted at a faster pace. End users are more and more interested in over-all user experience than factors like internet speed etc.

Internet Operators are reaching out to end customers with more emphasis on user experience because of the shift in end user apetite. Hence there is an increased focus within operator community about end user's Quality Of Experience, besides the network QoS and KPIs.

Our focus with this experimentation is about how end user quality of experience vary with respect to WiFi metrics. And our goals is to evaluate multiple ML/AI models in predicting end user Quality of Experience based the real-time WiFi and application metrics data.

## 2. Experimentation overview
The goal of the setup is to simulate a home environment, where a device can be at different distances from the AP and multiple devices connect to AP and contend for the air interface. Setup simulates the varying distance from the AP by having 3 separate chambers each hosting a virtual AP and these three chambers are stationed at near, medium and far distance from the device under test chamber.

In each chamber we instantiate x number of virtual STAs. We start with 1 virtual STA and increase the number of active virtual STAs one by one until 8. Each virtual STA send 30Mbps traffic upstream, down stream and both direction traffic. Each time we run a cloud gaming application using automated testing system that can simulate different application bandwidths 10, 30, 50 Mbps. This automated platform also can capture the application QoE metrics.

## 3. Data overview
At each run of the test, candella LanForge system captures two separate files: port_metrics.csv & vap_metrics.csv. Port Metrics file consists of the stats collected at virtual AP in the Lanforge system and the metrics are the system metrics. VAP metrics file contains the metrics for each virtual STA as well as device under test STA.

Application management system collects the application metrics and these metrics are captured in Qoe.xslx file.

Here are the list of metrics in each file.

<img src="https://github.com/user-attachments/assets/1784baf9-0644-4cd4-b058-52fb36fea266" width="500" height="500">

## 4. Understanding the data
A simple pre-processing step that can be done before data preparation is to understand the data and find the correlation between different features and explore the relation between the input data.

### 4.1 MOS Score distribution
When choosing random data for trainining, validation and testing, we need to make sure that selected data follows the same distribution pattern as that how MOS scores are distributed. Hence here we find the MOS score distribution pattern and plot it as histogram as well as a pie chart. MOS score distribution indicates that MOS scores aren't distributed unifromly.
<img width="891" alt="image" src="https://github.com/user-attachments/assets/7c9ee643-0cb2-4912-947c-003e01b45081">


### 4.2 Corrlation Map
Next step in data analysis to understand the correlation between the features. Correlation can be used to reduce dimensionality if the features are correlated strongly. In our data, it's clear that WiFi metrics like "rx/tx bps and rx/tx bps ll" are equivalent. Based on that we drop the following features and drop the feature dimensionality.

* 'bps rx ll'
* 'bps tx ll'
* 'pps rx'
* 'pps tx'
* 'sta_signal'
* 'rx pkts'
'* tx pkts'

Also the following features have more correlation to our target feature i.e. MOS score.
<img width="400" alt="image" src="https://github.com/user-attachments/assets/7c2be99f-0cd1-42c1-a9b5-4a2336e92101">

So, a simple line plot of these features vs MOS score shows the following correlation. It seems that no single feature has a disproportinate impact on MOS score.

<img width="1204" alt="image" src="https://github.com/user-attachments/assets/55561937-237a-4011-a4fc-d21dcdba21d9">

## 5. Data Analysis

Data analysis step includes identify multiple regression models and evaluating the performance and accuracy of these models. For each model that is evaluated, the following KPIs are captured and stored in 'model_kpi_df' data frame.

| Key          | Description                         |
| -------------| ------------------------------------|
| model        | Model Name                          |
| mean_error   | Mean Error                          |
| mean_perror  | Mean Percentage Error               |
| std_error    | Error Stanadard Deviation           |
| std_perror   | Percentage Error Standard Deviation |
| duration     | Duration                            |

### 5.1 Linear Regression with PCA components
The first model we explore for data analysis is Linear regression. Instead of using Wifi metrics as is, we explore if data transformed to PCA components can be used to predict the MOS score and how effective this model to predict the MOS scores. We use PCA transform method to persist the input data transformations within the model, so that new data can be used as it comes in for MOS score prediction.

<img width="572" alt="image" src="https://github.com/user-attachments/assets/06a26d7c-6804-415b-b7d3-b7d671a9c270">

### 5.2 Linear Regression
The next model we explore for data analysis is Linear regression. We use Wifi metrics data as is, for training the model. Just as in previous model evaluation, we evalute accuracy and performance of MOS score prediction.

<img width="577" alt="image" src="https://github.com/user-attachments/assets/eb7bf29f-e810-4245-8568-ac827fb6f424">

### 5.3 Ridge Regression
The next model we explore for data analysis is Ridge regression. We use Wifi metrics data as is, for training the model. Just as in previous model evaluation, we evalute accuracy and performance of MOS score prediction.

<img width="769" alt="image" src="https://github.com/user-attachments/assets/c1a30a83-500b-4570-9a98-7026ea34dbda">

### 5.4 Lasso Regression
The next model we explore for data analysis is Lasso regression. We use Wifi metrics data as is, for training the model. Just as in previous model evaluation, we evalute accuracy and performance of MOS score prediction.

<img width="769" alt="image" src="https://github.com/user-attachments/assets/5d57ab76-1778-440e-9bf1-c2656f61e1c1">

### 5.5 K-nearest neighbor Regression
The next model we explore for data analysis is K-nearest neighbors regression. We use Wifi metrics data as is, for training the model. Just as in previous model evaluation, we evalute accuracy and performance of MOS score prediction.

<img width="803" alt="image" src="https://github.com/user-attachments/assets/90078f6e-7b12-463c-82a0-1961fe54bc43">

### 5.6 Decision Tree Regression
The next model we explore for data analysis is Decision Tree regression. We use Wifi metrics data as is, for training the model. Just as in previous model evaluation, we evalute accuracy and performance of MOS score prediction.

<img width="1177" alt="image" src="https://github.com/user-attachments/assets/443d761e-9d29-4497-bbce-8af1480e73bd">

### 5.7 SVM Regression
The next model we explore for data analysis is SVM regression. We use Wifi metrics data as is, for training the model. Just as in previous model evaluation, we evalute accuracy and performance of MOS score prediction. SVM kernel regression consumes considerable compute, hence we use sampled data, insted of whole data for regression analysis.

<img width="965" alt="image" src="https://github.com/user-attachments/assets/508ffcba-2157-4e21-abb6-764aac3fdb47">

## 6. Conclusions
This section summarizes the performance and accuracy scores of all the models that were evaluated. It depicts Mean error, Mean percentage error, Error standard deviation as well as Percentage error standard deviation as bar chart.

<img width="705" alt="image" src="https://github.com/user-attachments/assets/05927bf8-636f-4d7d-916b-a2658529a2b2"> <img width="694" alt="image" src="https://github.com/user-attachments/assets/3ee5cf71-1d7b-4cfe-8bea-4ec0e1eb927c">

I conclude that Linear regression on PCA components provide the best predictiona accuracy. This model also requires the lowest compute power for prediction. Since PCA transformations are embedded in the model, this model can be applied to future data collected and predict the corresponding MOS Score.





