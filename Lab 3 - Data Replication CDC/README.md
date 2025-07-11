### IBM InfoSphere Data Replication

In this lab, you will be using IBM InfoSphere Data Replication to replicate data between the JK Life & Wealth’s banking and insurance databases. Also, we will be using InfoSphere Data Replication in connection with InfoSphere DataStage to keep a history of our bank balances to see how our business develops in different areas of the country.

In this lab, you will:
- Understand the concepts of InfoSphere Data Replication.
- Test the continuous mirroring between databases.
- See how to summarize information using InfoSphere IIDR.
- See how to integrate real time data feeds to an InfoSphere DataStage ETL process

---

### Testing the Continuous Synchronization between Operational Databases

I was assigned to setup replication between our banking and insurance database. There are three main scenarios here:
- Provide real-time customer address information for our insurance business out of our banking master data.
- Provide Bank balance information summarized by state code and gender for planning of future marketing activities.
- Provide a chronological view of this aggregated bank balance information to see how our business develops over time

---

1. **Open the “Start Menu” then click on the “IIDR Start” icon.**
 ![image](https://github.com/user-attachments/assets/125e230b-ef38-4f6d-b518-9dd46409aea2)

2. Open the “Start Menu” then click on the “Management Console” icon. 
  Authenticate with our IIDR Administrator ‘iidradm‘ and password ‘inf0server’ and click: “Login”. Leave the values for server name (empty) and port number (10101) unchanged.
 ![image](https://github.com/user-attachments/assets/f12816d4-3424-4e26-8248-8565eee13c49)

3. If not already selected click the “Configuration” button on the top.
 ![image](https://github.com/user-attachments/assets/d0632b36-c5e5-497c-a07c-7244fdc53188)

4. I can see three subscriptions; “IDR2DB”, “IDR2DC”, and “IDR2FILE” which are defined under project “Default Project”. Let’s first take a look at subscription “IDR2DB” which is used for the address and bank balance synchronization part. Click on “IDR2DB” in the left hand side of the window.

5. Please look at the source and target table relationship within the subscription and have a look at the different mapping types of the two source/target table combinations.
   <img width="1051" height="579" alt="image" src="https://github.com/user-attachments/assets/25a8b5d8-59b5-49a5-8754-2cbdb9a740d2" />

   Standard replication stands for straight 1:1 replication between the two tables. Summarization means that the contents of the source table will be aggregated by an individual key in the target table.

6. Let’s have a closer look at the mapping details of one of the table mappings. Double Click on the “JK_BANK1.BANK_ACCOUNTS” table. The mapping details will be opened in the lower part of the window. Here you can see how the columns of the source table are mapped to target table columns.
   <img width="1051" height="579" alt="image" src="https://github.com/user-attachments/assets/5b4ff9d9-27e5-47ed-90b4-643729467eef" />

7. Click on the “Monitoring” button.

   <img width="1051" height="579" alt="image" src="https://github.com/user-attachments/assets/6adbb9ba-d14c-4ea2-9c11-bed2f33a8dd2" />


   Subscription monitoring shows the state and status of the defined subscription. I can see that both subscriptions are in “Inactive” state at this point. Let us take a look at our source and target database tables before we start the replication process.

8. Double click on the “InfoSphere Data Architect” icon. This will open the “InfoSphere Data Architect” client that we will use for connecting to our database “JKLW_DBS”.
   <img width="1051" height="579" alt="image" src="https://github.com/user-attachments/assets/bfc01fc2-c52f-49f4-961b-184a30e92aac" />


9. In the “Data Project Explorer”, navigate to the “IIDR_LAB > SQL Scripts”
    <img width="1051" height="579" alt="image" src="https://github.com/user-attachments/assets/1744215f-5dba-4b50-ae9a-b911613d58af" />

10. Click “01_IDR2DB_SOURCE.sql” to see the script. Connect to **db2inst1** with the password: **inf0server** then run the script
    <img width="1051" height="579" alt="image" src="https://github.com/user-attachments/assets/f50eff1e-275b-447d-b906-d8aa4de58aa6" />

11. In the “SQL Results” pane, expand the operation to see the two SQL statements that were executed. The results show excerpts from our two source tables.
    <img width="1051" height="579" alt="image" src="https://github.com/user-attachments/assets/8ca5d876-c1d6-4698-826b-d6c51c4114c9" />

12. Click on each query and select the results tab to see output from the SQL statement.
  - “JK_BANK1.BANK_ACCOUNTS” which contains our banking customer master data. The “BANK_BALANCE” column is updated once a month during month end processing with the actual balance of the customer
    <img width="1051" height="390" alt="image" src="https://github.com/user-attachments/assets/c95f1bb0-2d80-4f47-ac52-eabc9dd4f112" />

  - “JK_BANK2.BANK_SAVINGS” contains customer address information. One record out of many is displayed here. We’ll change this record later on to see how InfoSphere IIDR replicates changes.
    <img width="1051" height="390" alt="image" src="https://github.com/user-attachments/assets/f4a4f94c-e52d-45ad-8855-87842f64ed0c" />

13. Right-click on the “02_IDR2DB_TARGET.sql” script and select “Run SQL” from the context menu. Repeat step #10 to select the database connection.

14. In the “SQL Results” pane, expand the operation to see the two SQL statements that were executed. The results show excerpts from our two target tables.

15. Click on each query and select the results tab to see output from the SQL statement. Both statements should return no rows back as both are target tables should be empty.
  - “JK_LIFE.ACCOUNTS_BY_STATE” which will later on contain bank balance information aggregated by state and gender.
    <img width="1051" height="390" alt="image" src="https://github.com/user-attachments/assets/f49c1c52-6ef0-4a31-a09d-956e1f7b55d3" />

  - “JK_LIFE.BANK_SAVINGS” which will contain a copy of the address information from our source table “JK_BANK2.BANK_SAVINGS”.
    <img width="1051" height="390" alt="image" src="https://github.com/user-attachments/assets/73768c0c-cb03-4752-9f57-15df3324bda4" />

16. Switch back to the InfoSphere “IIDR Management Console”.

17. On the “Monitoring” tab, right-click the subscription “IDR2DB” and choose “Start Mirroring”. This will start “continuous mirroring” for subscription “IDR2DB” to populate the target tables in the “JK_LIFE” database with data from the source tables according to the replication rules defined in the subscription.
    <img width="1051" height="582" alt="image" src="https://github.com/user-attachments/assets/55f63e7b-d560-4453-8f96-044755bc0047" />


18. Choose “Continuous” as the mirroring method. Please see the message at the bottom of the window that one or more tables will be refreshed when Mirroring is started. Note that since the initial status of the table mappings were set to “Refresh”, when the subscription starts, the data is first pulled from the source and loaded to the target table. Click the “OK” button.

19. Mirroring will be started. Notice that the state of the subscription changes from “Starting” to “Refresh Before Mirror” to “Mirror Continuous”
    <img width="1053" height="582" alt="image" src="https://github.com/user-attachments/assets/0fee1fb3-3366-48d8-9383-7a38cedbad03" />


20. Now switch back to the “InfoSphere Data Architect” window. Right-click on the “03_IDR2DB_UPDATE_SOURCE.sql” script and select “Run SQL” from the context menu. Repeat step #10 to select the database connection.

21. In the “SQL Results” pane, expand the operation to see the SQL statements that were executed.
    <img width="710" height="390" alt="image" src="https://github.com/user-attachments/assets/010b94f6-1cb2-4fa1-b674-b155eb028f3b" />


22. Click on each query and select the results tab to see output from the SQL statement.
  - The first query for the “JK_LIFE.ACCOUNTS_BY_STATE” table has been populated with aggregated data by state, gender from the source table “JK_BANK1.BANK_ACCOUNTS”. Scroll down to the record that shows “male” account holders in the state of “VA” and make note of the value in the “BANK_BALANCE” field.
    <img width="710" height="390" alt="image" src="https://github.com/user-attachments/assets/dafdfc63-d938-48ae-afb4-5c5b403d3e2a" />

  - The second query for the “JK_LIFE.BANK_SAVINGS” table has received a 1:1 copy of the address information from source table “JK_BANK2.BANK_SAVINGS”. Make note of the value in the fourth field.
    <img width="710" height="390" alt="image" src="https://github.com/user-attachments/assets/3aebb6da-f5e4-4fa4-ade1-6f4e6daada37" />

  - The third and fourth queries are performing the changes on our source tables. Ignore the fifth query and the associated warning. This query simply delays the execution of the last two SQL statements by four seconds.
    <img width="710" height="390" alt="image" src="https://github.com/user-attachments/assets/f12d6c25-cb19-4de8-a3c1-6a488711006a" />


    <img width="710" height="390" alt="image" src="https://github.com/user-attachments/assets/38877f58-2ee1-4454-b994-f7a771cc3bb2" />

  - The sixth query is a re-run of the first query for the “JK_LIFE.ACCOUNTS_BY_STATE”. Scroll down to the record that shows “male” account holders in the state of “VA” and make note of the change in the value of the “BANK_BALANCE” field.
    <img width="710" height="390" alt="image" src="https://github.com/user-attachments/assets/3f5be2f7-8683-40df-97f0-8a7257d5b51c" />

  - The last query is a re-run of the second query for the “JK_LIFE.BANK_SAVINGS” table. Make note of the change in the value of the fourth field
    <img width="710" height="390" alt="image" src="https://github.com/user-attachments/assets/9c4a612c-22ce-4abc-858f-346f729f439b" />

23. Switch back to the “IIDR Management Console” and look at our second subscription “IDR2DC”. Switch back to the “Configuration” perspective then click on the subscription “IDR2DC” in the left hand side of the window.
    <img width="1053" height="582" alt="image" src="https://github.com/user-attachments/assets/742f6ec8-8411-4ceb-90b3-fa0b5dee289f" />


  Please notice that the source table is our aggregate “JK_LIFE.ACCOUNTS_BY_STATE” table and the target is not a specific table but InfoSphere DataStage Direct Connect in this case. This means that InfoSphere IIDR directly interacts with an InfoSphere DataStage job by feeding the job with only changed data.

24. Let’s look at the DataStage Properties for this subscription. Right-click on the “IDR2DC” subscription in the left hand side of the window and choose “InfoSphere DataStage Properties”
    <img width="1053" height="582" alt="image" src="https://github.com/user-attachments/assets/63cbc052-6eb7-4b6a-b052-39b6de2d960d" />


25. In the InfoSphere DataStage Properties window is defined how InfoSphere IIDR interacts with the InfoSphere DataStage job. Notice that the subscription is set to automatically start the DataStage job.
    <img width="1053" height="582" alt="image" src="https://github.com/user-attachments/assets/a9411418-fc39-4793-bd53-0f93f4d53a50" />


26. On the “Monitoring” tab, right-click the subscription “IDR2DC” and choose “Start Mirroring”.
    <img width="1053" height="582" alt="image" src="https://github.com/user-attachments/assets/17f30441-1867-4044-87b5-639f9669f1cd" />


27. On the “Start Mirroring” dialog box, click the “OK” button.
    <img width="1053" height="582" alt="image" src="https://github.com/user-attachments/assets/9418203b-4dd7-4a24-8e39-e9af67bd6fd3" />


28. Let’s start the InfoSphere DataStage designer to have a look at the related DataStage job. Logon as user “tracy”, password ‘inf0server’. Ensure that the project is set to ‘dstage1’ before clicking ‘OK’.
    <img width="710" height="390" alt="image" src="https://github.com/user-attachments/assets/81d9353c-27a8-424b-9a03-413e3a7bca02" />


29. From the repository browser open the job “IDR2DC” in the “JKLW > 08_DATA_REPLICATION” folder. Right-click on the “IDR2DC” job and select “Edit”.
    <img width="1053" height="582" alt="image" src="https://github.com/user-attachments/assets/3a3f2c24-a098-442c-9e7b-f2472c91bbff" />

  - Let’s have a look at the job design of the “IDR2DC” job.
    <img width="1053" height="582" alt="image" src="https://github.com/user-attachments/assets/722974b8-8a73-4231-b7f6-5086d2a60951" />

 The high level job flow is as follows:
  - Whenever changes occur on the source table InfoSphere IIDR immediately pushes the delta information through the IIDR Transaction Stage to the DataStage job.

  - Data is then processed through a so called Slowly Changing Dimension stage which writes the data to a DB2 table.
      - Whenever a new record is processed, the row is created in the target table and the VALID_FROM column of the table is populated with the current timestamp. The VALID_TO column is empty at that time, which means the record is currently valid.
      - Whenever an existing record is processed the existing record in the target table gets updated. The VALID_TO column reflects then the current timestamp and invalidates the records. In addition a new record will be created containing the updated values forming the new record which is currently valid.

31. While still in InfoSphere DataStage designer start the Run Directory utility
    <img width="1053" height="582" alt="image" src="https://github.com/user-attachments/assets/24e368e8-8cae-4982-9f65-808549f82f55" />


33. In Run Director you can later see that the InfoSphere DataStage job is started.
    <img width="710" height="390" alt="image" src="https://github.com/user-attachments/assets/512439aa-1c14-46e2-b854-fbe6c62ebd13" />

    Double click the job IDR2DC then click run
    <img width="710" height="390" alt="image" src="https://github.com/user-attachments/assets/1fb72516-1f85-490a-9fdd-dbd6ad9ecd48" />

    
    <img width="710" height="390" alt="image" src="https://github.com/user-attachments/assets/8b85018a-9340-4ef0-aee3-5feb6d5f7b91" />

34. Now switch back to the “InfoSphere Data Architect” window. Right-click on the “04_IDR2DC_SOURCE.sql” script and select “Run SQL” from the context menu. Repeat step #10 to select the database connection.
  - Remember that our current source table “JK_LIFE.ACCOUNTS_BY_STATE” is also being mirrored from the table “JK_BANK1.BANK_ACCOUNTS” using the subscription IIDR2DB from the beginning of this lab.
    <img width="710" height="390" alt="image" src="https://github.com/user-attachments/assets/9beab0b5-d0dd-4ac6-b4c3-90ecbee6ec56" />


35. Right-click on the “05_IDR2DC_TARGET.sql” script and select “Run SQL” from the context menu. Repeat step #10 to select the database connection.

36. In the “SQL Results" pane, expand the operation to see the two SQL statements that were executed. The results show excerpts from our two target tables.

37. Click on each query and select the results tab to see output from the SQL statement. Both statements should return no rows back as both are target tables should be empty.
  - “JK_LIFE.ACCOUNTS_BY_STATE” contains bank balance information aggregated by state and gender from earlier in the lab. Scroll down and make note of the “BANK_BALANCE” fields for account holders in the state of “VA”.
    <img width="710" height="390" alt="image" src="https://github.com/user-attachments/assets/d2efba6f-d84b-4850-b158-93caf3b3852f" />

  - “JK_LIFE.ACCOUNTS_SCD” has now been populated with data reflecting the source table “JK_LIFE.ACCOUNTS_BY_STATE”. Please also note that the “VALID_FROM” column reflects the current timestamp and the “VALID_TO” column is empty for all rows.
    <img width="710" height="390" alt="image" src="https://github.com/user-attachments/assets/3e213479-d618-4a24-98a6-84b82aab2541" />


38. Right-click on the “06_IDR2DC_UPDATE_SOURCE.sql” script and select “Run SQL” from the context menu. Repeat step #10 to select the database connection. This query will take at least 90 seconds to complete.

39. In the “SQL Results” pane, expand the operation to see the SQL statements that were executed.

40. Click on each query and select the results tab to see output from the SQL statement.
  - The first query is for the “JK_LIFE.ACCOUNTS_BY_STATE”. Scroll down to the records that show account holders in the state of “VA” and make note of the change in the value of the “BANK_BALANCE” fields.
    <img width="710" height="390" alt="image" src="https://github.com/user-attachments/assets/f96030ea-3d7f-42f8-b11a-46243044fe86" />

  - The second query is for the “JK_LIFE.ACCOUNTS_SCD” table. Scroll down and make note of the “BANK_BALANCE” fields for the state of “VA”.
    <img width="710" height="390" alt="image" src="https://github.com/user-attachments/assets/6b98c804-d24e-4279-91fc-ae206ff36b35" />

  - The third and fourth queries update the “BANK_BALANCE” field for the two records shown above. Ignore the fifth query and the associated warning. This query simply delays the execution of the last two SQL statements by ninety seconds.
    <img width="710" height="390" alt="image" src="https://github.com/user-attachments/assets/35a02a60-1fa6-42ad-b8f6-c0b09e254002" />

    <img width="710" height="390" alt="image" src="https://github.com/user-attachments/assets/5cc69506-edd8-41ec-ab9d-f08bbfb2152c" />

  - The sixth query is for the “JK_LIFE.ACCOUNTS_BY_STATE”. Scroll down to the records that show account holders in the state of “VA” and make note of the change in the value of the “BANK_BALANCE” fields.
    <img width="710" height="390" alt="image" src="https://github.com/user-attachments/assets/bfdd7f8f-9ad5-4766-83de-b0682fb719c8" />

  - The last query is a for the “JK_LIFE.ACCOUNTS_SCD” table you will notice that the two updated records from the source table have lead to properly time-delimited data in the target.
    <img width="710" height="390" alt="image" src="https://github.com/user-attachments/assets/ca907208-d342-46b8-9f7d-b21049f7bd91" />

  
40. This concludes the first part of this lab. Close the “IIDR Management Console”, the “DataStage Run Director”, the “DataStage Designer Client”, and “InfoSphere Data Architect” client.

---
### Summary
IBM InfoSphere Data Replication is a very powerful log based replication product which has little impact on a source database. In addition to replication, InfoSphere Data Replication can be used for database auditing and real time data transformation. It supports all the major relational database systems such as IBM DB2, Oracle, Sybase, IBM Informix®, IBM solidDB™, SQL Server for dual replication (source and target. Captured data can be provided as JMS messages or could be passed to third party software by using the ‘User Exit’ feature of the product.

IBM InfoSphere DataStage is a powerful, scalable ETL solution allowing comprehensive data transformations. IBM InfoSphere DataStage Designer enables you to build these comprehensive data transformations graphically, therefore letting the user focus on the data flow instead of worrying how to implement a certain function.

By using IBM InfoSphere Data Replication with IBM InfoSphere DataStage in your data integration architecture, you can effectively eliminate the need for resource intensive data extraction processes; you are not affecting the availability of your source systems; you are capturing changes in real-time; and you can harness the power of a comprehensive ETL solution.
