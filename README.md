# Lead Scoring - Using ML Modelling

## Business Understanding
An education company - online course provider for industry professionals, faces a challenge with its low lead conversion rate. Despite generating numerous leads through marketing efforts on platforms like Google and referrals, only about 30% of leads convert into paying customers. The company wants to identify "Hot Leads"—those most likely to convert—to improve efficiency and increase the conversion rate by allowing the sales team to focus on the most promising leads.

![download](https://user-images.githubusercontent.com/30548563/147401086-8d00570f-4788-4167-8af5-a01e15eaba51.png)

## Business Problem to Be Solved
The company needs a predictive model that assigns a lead score to each potential customer. This score should help prioritize leads based on their likelihood of conversion, aiming to achieve a target lead conversion rate of around 80%.

## Objective/Goal
1. Build a Logistic Regression model to assign a lead score (0-100) to each lead, with higher scores indicating higher conversion probability.
2. Ensure the model is flexible enough to adapt to future changes in business requirements.
3. Achieve a lead conversion rate close to 80%, improving from the current rate of 30%.


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



## Data Instruction
- **Dataset**: Historical data with ~9,000 rows and 37 columns.
- **Target Variable**: `Converted` (1 for converted leads, 0 for non-converted).
- **Features**: Includes variables like Lead Source, Total Time Spent on Website, Last Activity, etc. Some features required cleaning, binning, and transformation (e.g., creating dummy variables for categorical data).

## Process in Brief
### Data Analysis
1. **Data Cleaning**:
   - Removed unnecessary columns and rows with excessive null values.
   - Addressed outliers by binning certain columns.
   - Reduced levels in categorical variables and merged repeated levels.
2. **Exploratory Data Analysis**:
   - Visualized lead origins, sources, and activities.
   - Identified key patterns such as most leads originating from Google or Landing Page Submissions.

### ML Modeling
1. **Data Preparation**:
   - Converted binary categorical variables (Yes/No) into integers (1/0).
   - Created dummy variables for non-binary categorical features.
   - Split data into training (70%) and testing (30%) sets.
   - Standard-scaled continuous variables.
2. **Feature Selection**:
   - Used Recursive Feature Elimination (RFE) to select the top 15 features.
3. **Model Building**:
   - Developed a Logistic Regression model iteratively by removing features with high Variance Inflation Factor (VIF > 5) or insignificant p-values (> 0.05).
4. **Evaluation**:
   - Evaluated the model using metrics like accuracy, sensitivity/recall, specificity, precision, and Area Under the Curve (AUC).
   - Optimized the probability cutoff based on accuracy-sensitivity-specificity trade-offs.

### Results
- **Training Set Performance** (cutoff = 0.35):
  - Accuracy: 78%
  - Sensitivity/Recall: 80%
  - Specificity: 77%
  - Precision: 68%
  - AUC: 86%
- **Test Set Performance**:
  - Accuracy: 78%
  - Sensitivity/Recall: 77%
  - Specificity: 78%
  - Precision: 67%
- Lead scores were calculated as $$ \text{Conversion Probability} \times 100 $$.

## Results
The Logistic Regression model successfully achieved a lead conversion rate close to the target of 80%. The model's performance metrics indicate it is robust and meets business requirements. Key insights include:
- Positive impact of features like Lead Source (e.g., Welingak Website) and Last Activity (e.g., SMS Sent).
- Negative impact of features like "Do Not Email" and "Olark Chat Conversion."

This solution enables X Education's sales team to focus on high-priority leads, improving efficiency and potentially increasing revenue.