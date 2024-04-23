![image](https://github.com/jVilla206/patient_satisfaction/assets/113951737/6aab4200-f9cc-4236-8a8b-55077063377e)# Overview
Huge shoutout to Joshua Matlock (Datawizardry) for helping guide this project

* Data set origins: **Centers for Medicare and Medicaid Services (CMS)**
* Data set: **Hospital Consumer Assessment of Healthcare Providers and Systems (HCAHPS)**
**Preface:**
  * The U.S. government requires hospitals to collect this data in the form of surveys given to their patients.
  * Hospitals are incentivized in a program called value based purchasing.
   
This is a breakdown of how I looked at patient survey data from Hospital Consumer Assessment of Healthcare Providers and Systems (HCAHPS) to see how satisifed patients were with their hospital stay.

# Data Cleaning
**Problem 1:**

**Problem 2: Sometimes the hospitals will have multiple dates listed to account for the different times at which they reported out the number of beds that their hospital has**
**Solution: Will specify 1 row per hospital. We will write a query that will pick the most recent reporting of hospital beds. Then join to the HACHPS dataset so we won't get a one to many type join. We want to kick out all the older existing data and keep the most up-to-date information that we have in the dataset.**

**Problem 3: Dates are not formatted in a very good way**
* SQL (Postgres) doesn't like dates in the MM-DD-YYYY format and prefers them to be in the YYYY-MM-DD format.
* I will be using the to_date() function to convert the data to the preferred format



* Each hospital has a unique Provider CMS Certification Number (CCN) to identify each specific hospital. These values consist of 6 numbers in total. However, looking at the data I saw that excel cut off the leading 0 making it seem like some of the Proivder CCN's only had 5 integers.
   * lpad()
   * cast()

* to_date

* Created a common table expression (CTE)
 
* row_number() [Windows functions]

* To categorize the size of hospitals, I decided to count the number of  in-patient hospital beds. We would later use the number of beds to categorize our hospitals into 'small', 'medium', and 'large' scale hospitals.

* After I prepped and cleaned the data from PostgresSQL, I wanted to export only the relevant and now clean data for use in Tableau. Using the create table statement I was able to export the file back to .csv
  * create table "postgres"."Hospital_Data".Tableau_File as
  ...


# Data Visualization
