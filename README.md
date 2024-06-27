# Overview
Huge shoutout to Joshua Matlock (Datawizardry) for helping guide this project.

* Data set origins: **Centers for Medicare and Medicaid Services (CMS)**
* Data set: **Hospital Consumer Assessment of Healthcare Providers and Systems (HCAHPS)**
**Preface:**
  * The U.S. government requires hospitals to collect this data in the form of surveys given to their patients.
  * Hospitals are incentivized in a program called value-based purchasing. The hospital value-based purchasing program (HVBP) means that hospital payments are adjusted based on the quality of care they provide each fiscal year.
    * Hospital care is evaluated against measures in four domains. We will look specifically at the last domain (person & community engagement) which has 8 measures. We want to look at how satisfied are patients with the hospital and the patients who gave their hospital a "top box" answer aka a 9 or 10 on their HCAHPS survey.
   
This is a breakdown of how I looked at patient survey data from the Hospital Consumer Assessment of Healthcare Providers and Systems (HCAHPS) to see how satisfied patients were with their hospital stay. Also, I wanted to see how well hospitals were doing in comparison to one another.

The tools I will be using for this project include:
* **SQL (PostgreSQL)**
* **Tableau**

# Data Cleaning
**Problem 1:**
* Each hospital has a unique CMS Identification Number (Provider CCN) to identify each specific hospital. These values consist of 6 digits in total. However, looking at the data I saw that excel cut off the leading 0 making it seem like some of the Provider CCN's only had 5 integers. We want to keep the leading the 0 in there.

**Solution: We will pad numbers to the left until it gets to six digits in length.****
   * lpad()
   * cast()
The lpad() function will not work on integer-based columns, so we need to convert the data type from integers to text string. We can achieve this using the cast() function.
````sql
lpad(cast(facility_id as text), 6, '0') as provider_ccn
````

**Problem 2: Sometimes the hospitals will have multiple dates listed to account for the different times at which they reported out the number of beds that their hospital has.**

**Solution: Will specify 1 row per hospital. We will write a query that will pick the most recent reporting of hospital beds. Then join to the HCAHPS dataset so we won't get a one to many type join. We want to kick out all the older existing data and keep the most up-to-date information that we have in the dataset.**

**Method: Combination of a common table expression and a partitioning statement.****

 Created a common table expression (CTE)

 partitioning statement:
````sql
row_number() over (partition by provider_ccn order by to_date(fiscal_year_end_date, 'MM/DD/YYYY') desc) as nth_row
````

* To categorize the size of hospitals, I decided to count the number of in-patient hospital beds to label them as either 'small', 'medium', or 'large' scale hospitals. Categorizing hospitals into different sizes will allow us to put peer comparison groups where we can evaluate things such as medium hospitals vs. other medium hospitals in a specific state.


**Problem 3: Dates are not formatted in a very good way**
* SQL (Postgres) doesn't like dates in the MM-DD-YYYY format and prefers them to be in the YYYY-MM-DD format.
 
* I will be using the to_date() function to convert the data to the preferred format

````sql
to_date(start_date, 'MM/DD/YYYY') as start_date_converted
to_date(end_date, 'MM/DD/YYYY') as end_date_converted
````

**Export:**
* After I prepped and cleaned the data from PostgresSQL, I wanted to export only the relevant and now clean data for use in Tableau. Using the create table statement I was able to export the file back to .csv
  * ````Create table "postgres"."Hospital_Data".Tableau_File````


# Data Visualization
Uploaded exported .csv file to Tableau.
Created visuals with information regarding:
* % of patients rating the hospital 9-10 out of 10 (Top Box answer)
* Survey response rate
* # of completed surveys
* Question delta (change) from mean cohort % (depending on hospital size and state)
* Cohort hospital delta spread for each question from the HCAHPS survey to indicate relative performance
* Also added a custom views to stratify by "State" and "Hospital Size".

To see the final data visualization in Tableau <https://public.tableau.com/app/profile/jvilla/viz/PatientSatisfactioninHospitals/HCAHPSDashboard>
