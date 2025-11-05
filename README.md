# Healthcare-Data-Analytics-SQL
An advanced SQL portfolio on Google BigQuery analyzing a healthcare database. Demonstrates Window Functions, CTEs, & Subqueries to solve business problems like physician revenue & aged receivables. Also features CASE statements, multi-table JOINs, & COALESCE for data cleaning & business logic.

# Healthcare Operations Analysis in SQL (BigQuery)

## Project Objective
The goal of this project was to act as a Data Analyst for a hospital. I analyzed a multi-table relational database to identify key drivers of revenue, track billing efficiency, and understand patient-doctor interactions.

## The Dataset
The raw data for this project was sourced from the "Hospital Management Dataset" on Kaggle.

*   **Source:** https://www.kaggle.com/datasets/kanakbaghel/hospital-management-dataset
*   **Process:** I personally uploaded the CSV files to a private project in Google BigQuery, established the table relationships, and used BigQuery's SQL engine for all analysis.

## Database Schema
The database contained 5 tables:

*   `patients` (patient_id, first_name, ...)
*   `doctors` (doctor_id, specialization, ...)
*   `appointments` (appointment_id, patient_id, doctor_id, ...)
*   `treatments` (treatment_id, appointment_id, ...)
*   `billing` (bill_id, treatment_id, amount, ...)

## Key Analyses & Insights

### Analysis 1: Doctor Revenue Report

*   **Business Question:** Which doctors are generating the most revenue for the hospital?
*   **My SQL Query:**
    ```sql
    -- This query joins all 4 tables to connect doctors to billing
    SELECT
        d.first_name,
        d.last_name,
        d.specialization,
        SUM(b.amount) AS Total_Billed_Revenue
    FROM
        `bio_analytics.doctors` AS d
    JOIN
        `bio_analytics.appointments` AS a ON d.doctor_id = a.doctor_id
    JOIN
        `bio_analytics.treatments` AS t ON a.appointment_id = t.appointment_id
    JOIN
        `bio_analytics.billing` AS b ON t.treatment_id = b.treatment_id
    GROUP BY
        d.first_name, d.last_name, d.specialization
    ORDER BY
        Total_Billed_Revenue DESC
    LIMIT 5;
    ```
*   **Results & Insight:** The query successfully identified the top 5 earners. This insight would allow management to explore what high-performing doctors (e.g., in Dermatology) are doing differently.

    ![Screenshot of BigQuery Results for Doctor Revenue](<img width="639" height="167" alt="Image" src="https://github.com/user-attachments/assets/0b66387b-0ab4-4e64-8f5e-6cdbfc94099b" />)

