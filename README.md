# Overview
This is a breakdown of how I looked at data from the Centers for Medicare & Medicaid Services (CMS) and took patient survey data from the Hospital Consumer Assessment of Healthcare Providers and Systems (HCAHPS) to see how satisifed patients were with their hospital stay.

# Data Cleaning
* Each hospital has a unique Provider CMS Certification Number (CCN) to identify each specific hospital. These values consist of 6 numbers in total. However, looking at the data I saw that excel cut off the leading 0 making it seem like some of the Proivder CCN's only had 5 integers.
   * lpad()
   * cast()

* to_date

* Created a common table expression (CTE)
 
* row_number() [Windows functions]

* To categorize the size of hospitals, I decided to count the number of hospital beds. We would later use the number of beds to categorize our hospitals into 'small', 'medium', and 'large' scale hospitals.

* After I prepped and cleaned the data from PostgresSQL, I wanted to export only the relevant and now clean data for use in Tableau. Using the create table statement I was able to export the file back to .csv
  * create table "postgres"."Hospital_Data".Tableau_File as
  ...


# Data Visualization
