# Ingest-real-time-data-with-Eventstream-in-Microsoft-Fabric
# 🚴‍♂️ Ingest Real-Time Bicycle Data with Eventstream in Microsoft Fabric

This project demonstrates how to ingest, transform, and query real-time data using **Microsoft Fabric Eventstream** and **KQL databases**. The use case simulates a bike-share system where events are emitted for bicycle collection points across a city.

## 🧪 Lab Overview

In this lab, you'll:
- Set up a workspace and an eventhouse
- Ingest streaming bicycle data using sample data
- Transform the data using real-time groupings
- Query both raw and transformed data using Kusto Query Language (KQL)
- Clean up resources after use

> ⏱ Estimated time: 30 minutes  
> 🧾 Prerequisite: Microsoft Fabric tenant (Trial, Premium, or Fabric license)

---

## 📁 Steps

### 1. Create a Workspace
- Go to [Microsoft Fabric](https://app.fabric.microsoft.com/home?experience=fabric)
- Sign in and create a new **workspace** with Fabric capacity enabled

### 2. Create an Eventhouse
- Inside your new workspace, create a new **Eventhouse**
- This will automatically include a **KQL database**

### 3. Create an Eventstream
- Open your KQL database and click **Get data**
- Select **Eventstream > New eventstream**, and name it `Bicycle-data`

### 4. Add a Source
- Use the **sample data** option and select the `Bicycles` source
- Name the source `Bicycles`

### 5. Add a Destination
- Add a destination of type **Eventhouse**
- Set it up with the following parameters:
  - Destination name: `bikes-table`
  - Table name: `bikes`
  - Input format: `JSON`
  - Ingestion mode: `Event processing before ingestion`
- Save and **Publish**

### 6. Query Captured Data
- Navigate to your KQL database
- Run the following query:
  ```kql
  bikes
  | where ingestion_time() between (now(-1d) .. now())
    This shows records from the last 24 hours

7. Transform Event Data

    Edit the Bicycle-data Eventstream

    Add a Group By transformation:

        Operation: Sum

        Field: No_Bikes

        Group by: Street

        Time window: Tumbling, duration: 5 seconds

    Add a destination:

        Destination name: bikes-by-street-table

        Table name: bikes-by-street

        Format: JSON

    Save and Publish

8. Query Transformed Data

    Query the grouped data using:

    ['bikes-by-street']
    | summarize TotalBikes = sum(tolong(SUM_No_Bikes)) by Window_End_Time, Street
    | sort by Window_End_Time desc, Street asc

🧹 Clean Up

After you are done, delete the workspace:

    Select the workspace

    Open Workspace Settings

    Choose Remove this workspace

📌 Notes

    Brackets ['bikes-by-street'] are used in KQL because the table name contains a hyphen -

    Use Refresh in the database view to update the table list after publishing
