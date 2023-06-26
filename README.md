# Case Study 2: Scoring Leads for an Educational Institution

----------------------------------------------------------------
----------------------------------------------------------------

For documentation of this case study there were two things that I worked on - 
1) Summary
2) Report

------------------------------------------------------------------

### Summary of the Case Study:

The objective of the Case Study was to predict the number of converted leads for X Education a company that sells online courses. We wanted the lead conversion rate of our model to be around 80%.

We were given a dataset with 9240 rows and 37 columns. Upon performing Data Cleaning, we had to remove some unrequired columns, and some columns with null values. For some columns, we even had to remove some rows due to null values in them, after which we were left with 9074 rows and 31 columns. By further visualizing the data, we found out that most of the leads are Sourced from google and most customers contacted are have either opened their email recently or they have received/sent a message. Also, most of the leads originate from Landing Page Submission. Apart from this, customers have visited very few web pages of the website, spending very less time on it and frequency of these visits is also very low. Next, when outlier analysis was performed, many outliers were found, thus we decided to created bins for such columns.

While data-preparation, we had to reduce the number of levels for categorical variables. Also, there were some repeated levels in some columns, which we fixed by merging them. Our next step was to convert Binary-Categorical-Columns from Yes/No type to integer 1/0 type. Following which we created dummy variables for Non-Binary-Categorical-variables. This step was followed by 70-30 Train-Test split which was again followed by Standard-Scaling the continuous variables. We then checked for leadconversion rate which came out to be 38% which is good. 

Since we had a lot of variables present in the data, we minimized their count using RFE and selected 15 best columns with it. 

We then made use of the Logistic regression Algorithm to build the model. We ran multiple models until we got a VIF-score of less than 5 and a P-value of 0.05. After running each model, we dropped a feature based on VIF-score and P-value. On our sixth try we got a suitable model. When we ran the model on the training set with a conversion_probability cut-off of 0.5, we got accuracy of 80%, sensitivity/recall of 67%, Specificity of 88% and Precision of 77%. We then proceeded with plotting the ROC curve which gave us an AUC as 86% which is considered very good. Even though we achieved our business objective of around 80% lead conversion rate, still we calculated the optimized cut-offs using Accuracy-Sensitivity-Specificity trade-off which came out to be 0.35. With this new cut-off, the Accuracy became 78%, sensitivity/recall 80%, specificity 77% and Precision 68% which is still good.

On making predictions on the test set with cut-off of 0.35, we got an accuracy of 78%, Sensitivity/Recall 77%, Specificity 78% and Precision 67%. Also, we plotted the ROC curve for the same and AUC came out to be 0.85, which is good. Following this we calculated the Lead Score by multiplying Conversion_probability*100. 

Thus, we successful implemented the Logistic Regression model with a lead conversion rate of around 80%.


-----------------------------------------------------------
`The report that follows is my explanation of what was performed in the Case Study.`
-----------------------------------------------------------


## Business Understanding:
------------------------
An education company named X Education sells online courses to industry professionals. On any given day, many professionals who are interested in the courses land on their website and browse for courses.

The company markets its courses on several websites and search engines like Google. Once these people land on the website, they might browse the courses or fill up a form for the course or watch some videos. When these people fill up a form providing their email address or phone number, they are classified to be a lead. Moreover, the company also gets leads through past referrals. Once these leads are acquired, employees from the sales team start making calls, writing emails, etc. Through this process, some of the leads get converted while most do not. The typical lead conversion rate at X education is around 30%.

Now, although X Education gets a lot of leads, its lead conversion rate is very poor. For example, if, say, they acquire 100 leads in a day, only about 30 of them are converted. To make this process more efficient, the company wishes to identify the most potential leads, also known as ‘Hot Leads’. If they successfully identify this set of leads, the lead conversion rate should go up as the sales team will now be focusing more on communicating with the potential leads rather than making calls to everyone. A typical lead conversion process can be represented using the following funnel:

![download](https://user-images.githubusercontent.com/30548563/147401086-8d00570f-4788-4167-8af5-a01e15eaba51.png)

As you can see, there are a lot of leads generated in the initial stage (top) but only a few of them come out as paying customers from the bottom. In the middle stage, you need to nurture the potential leads well (i.e. educating the leads about the product, constantly communicating etc.) in order to get a higher lead conversion.

X Education has appointed you to help them select the most promising leads, i.e. the leads that are most likely to convert into paying customers. The company requires you to build a model wherein you need to assign a lead score to each of the leads such that the customers with higher lead score have a higher conversion chance and the customers with lower lead score have a lower conversion chance. The CEO, in particular, has given a ballpark of the target lead conversion rate to be around 80%.

------------------------------------------------------------------

## Data Understanding:
---------------------
Leads dataset from the past with around 9000 data points is provided. 
This dataset consists of various attributes such as Lead Source, Total Time Spent on Website, Total Visits, Last Activity, etc. which may or may not be useful in ultimately deciding whether a lead will be converted or not. The target variable, in this case, is the column ‘Converted’ which tells whether a past lead was converted or not wherein 1 means it was converted and 0 means it wasn’t converted. You can learn more about the dataset from the data dictionary provided in the zip folder at the end of the page. Another thing that you also need to check out for are the levels present in the categorical variables.

------------------------------------------------------------------

## Data Dictionary:
----------------------
| Variables |	Description |
| :- | :------------- |
| Prospect ID | A unique ID with which the customer is identified.|
| Lead Number | A lead number assigned to each lead procured.|
|Lead Origin | The origin identifier with which the customer was identified to be a lead. Includes API, Landing Page Submission, etc.|
|Lead Source | The source of the lead. Includes Google, Organic Search, Olark Chat, etc. |
|Do Not Email | An indicator variable selected by the customer wherein they select whether of not they want to be emailed about the course or not.|
| Do Not Call |	An indicator variable selected by the customer wherein they select whether of not they want to be called about the course or not.|
| Converted |	The target variable. Indicates whether a lead has been successfully converted or not.|
| TotalVisits | The total number of visits made by the customer on the website.|
| Total Time Spent on Website |	The total time spent by the customer on the website.|
| Page Views Per Visit | Average number of pages on the website viewed during the visits.|
| Last Activity | Last activity performed by the customer. Includes Email Opened, Olark Chat Conversation, etc.|
| Country | The country of the customer.|
| Specialization | The industry domain in which the customer worked before. Includes the level 'Select Specialization' which means the customer had not selected this option while filling the form. |
| How did you hear about X Education | The source from which the customer heard about X Education.|
| What is your current occupation | Indicates whether the customer is a student, umemployed or employed.|
| What matters most to you in choosing this course | An option selected by the customer indicating what is their main motto behind doing this course.|
| Search | Indicating whether the customer had seen the ad in any of the listed items.|
| Magazine |Indicating whether the customer had seen the ad in any of the listed items.|
| Newspaper Article	| Indicating whether the customer had seen the ad in any of the listed items.|
|X Education Forums	| Indicating whether the customer had seen the ad in any of the listed items.|
| Newspaper	| Indicating whether the customer had seen the ad in any of the listed items.|
| Digital Advertisement	|Indicating whether the customer had seen the ad in any of the listed items.|
| Through Recommendations | Indicates whether the customer came in through recommendations.|
| Receive More Updates About Our Courses | Indicates whether the customer chose to receive more updates about the courses.|
| Tags | Tags assigned to customers indicating the current status of the lead.|
| Lead Quality	| Indicates the quality of lead based on the data and intuition the the employee who has been assigned to the lead. |
|Update me on Supply Chain Content | Indicates whether the customer wants updates on the Supply Chain Content.|
| Get updates on DM Content | Indicates whether the customer wants updates on the DM Content.|
|Lead Profile | A lead level assigned to each customer based on their profile.|
|City | The city of the customer.|
|Asymmetrique Activity Index|An index and score assigned to each customer based on their activity and their profile|
|Asymmetrique Profile Index	|An index and score assigned to each customer based on their activity and their profile|
|Asymmetrique Activity Score|An index and score assigned to each customer based on their activity and their profile|
|Asymmetrique Profile Score|An index and score assigned to each customer based on their activity and their profile|
|I agree to pay the amount through cheque | Indicates whether the customer has agreed to pay the amount through cheque or not.|
| a free copy of Mastering The Interview | Indicates whether the customer wants a free copy of 'Mastering the Interview' or not.|
|Last Notable Activity | The last notable acitivity performed by the student.|

------------------------------------------------------------------

## Objectives of the Case Study:
-------------------------------
**Task 1** - <br>
Build a Logistic Regression Model to assign a lead score between 0 and 100 to each of the leads which can be used by the company to target potential leads. A higher score would mean that the lead is hot, i.e. is most likely to convert whereas a lower score would mean that the lead is cold and will mostly not get converted.

**Task 2** - <br>
There are some more problems presented by the company which your model should be able to adjust to if the company's requirement changes in the future so you will need to handle these as well. Our task will be to use the Logistic regression model to solve the problems provided by the company. This is attached as a separate word document.

------------------------------------------------------------------------------

## Methodology:
------------------------------------------------------------------------------
- 1): Reading and Understanding the Data
- 2): Exploratory Data Analysis
	- Data Cleaning
		- Dealing with Null Values
		- Dropping Unnecessary Columns
		- 
	- Visualizing the Data
	- Outlier Analysis
- 3): Preparing the Data for Modelling
	- Decreasing the Number of Labels from Non-Binary Categorical Variables
	- Converting the column into the required data types
	- Creating Dummy Variables
	- Train-Test Split (70% - 30%)
	- Re-Scaling (Standard Scalar)
	- Checking for Lead Conversion Rate and Class Imbalance
	- Looking for Correlations
- 4): Recursive Feature Elimination & Model Building
	- Running Recursive Feature Elimination with 15 variables as Output.
	- Building the Logistic Regression Model
	- Using the VIF Score to remove the unnecessary variables from the train data.
- 5): Making the Prediction with the Model
- 6): Model Evaluation
	- Calculation confusion matrix.
	- Calculating various performance metrics.
- 7): Plotting the ROC Curve
- 8): Finding the optimal probability cut-offs
- 9): Making Predictions on the Test Set
- 10): Calculating lead scores for the entire dataset
- 11): Choosing the Best Features that impacts our predictions



-----------------------------------------------------------------------------

## Observations and Inferences from the Data Plotting and Visualization:
------------------------------------------------------------------------------

- We observe that there most if origin of the leads are from 'Landing Page Submission', while the least is from 'Lead Import'.
- We also observe that most of the lead sources are from 'Google' followed by 'Direct Traffic', whereas 'Referral Sites' have the least.
- We also see that people who opened their email are highly seen by the company as a possible lead.
<br><br>
- We observe that the average number of total visits by a customer on the website is on the lower end.
- We also observe that average total time spent on the website by customes is also less.
- We also observe that the average number of page view for the website on one visit is very less.
<br><br>

----------------------------------------------------------------------------

## Top Features impacting the Lead conversion:
------------------------------------------------------
The conversion probability of a lead increases with increase in values of the following features in descending order:
> - Lead Source_Welingak Website
> - Lead Source_Reference
> - Last Activity_SMS Sent
> - Last Activity_Other Activities
> - Lead Origin_Lead Import
> - Lead Source_Olark Chat
> - Total Time Spent on Website
> - Last Activity_Email Opened
<br><br>

The conversion probability of a lead increases with decrease in values of the following features in descending order:
> - Do Not Email
> - Last Activity_Olark Chat Conversion

--------------------------------------------------------------------------------

## Conclusion:
-----------------------------------------------------------------------------------
The model we made using the logistic regression can be considered a good model. It has the following characteristics -

- `All the features/variables have a P-value of less than 0.05.`

- `The VIF scores for all the variables are very low and are less than 5, thus there is hardly any multi-collinearity between the variables.`

- `The overall accuracy of the model is around 78%, with a threshold probaility of 0.5.`

- `Thus the accuracy is very acceptable.`

- `Also the specificity of the model is around 78.5% which is also acceptable.`

- `The sensitivity/recall of the model is around 76% which is also acceptable.`

- `The precision of the model is about 68% which could be considered decent.`

- `Also, when we plotted the ROC curve, the area under the curve we got was aroung 86%, which could be considered good.`

<br><br>
**Overall this model meets our business requirement, where we can say we got a lead conversion rate of nearly 80%.**
<br><br>
Apart from the model there were some variable which greatly influenced our model.
The top 3 variables which influenced our model in a positive way are -
- 1 - Lead Source - Welingak Website
- 2 - Lead Source - Reference
- 3 - Last Activity - SMS Sent
<br><br>
The top 2 variables which influenced our model in a negative way are -
1 - Do Not Email
2 - Last Activity - Olark Chat Conversion


--------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------


