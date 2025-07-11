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

10. Double click on the “InfoSphere Data Architect” icon. This will open the “InfoSphere Data Architect” client that we will use for connecting to our database “JKLW_DBS”.

11. In the “Data Project Explorer”, navigate to the “IIDR_LAB > SQL Scripts”,

12. Right-click on the “01_IDR2DB_SOURCE.sql” script and select “Run SQL” from the context menu.

13. In the “Select Connection Profile” dialog box, select the “JKLW_DBS” connection then click the “Finish” button.

14. In the “SQL Results” pane, expand the operation to see the two SQL statements that were executed. The results show excerpts from our two source tables.

15. Click on each query and select the results tab to see output from the SQL statement.
  - “JK_BANK1.BANK_ACCOUNTS” which contains our banking customer master data. The “BANK_BALANCE” column is updated once a month during month end processing with the actual balance of the customer
  - “JK_BANK2.BANK_SAVINGS” contains customer address information. One record out of many is displayed here. We’ll change this record later on to see how InfoSphere IIDR replicates changes.

14. Right-click on the “02_IDR2DB_TARGET.sql” script and select “Run SQL” from the context menu. Repeat step #12 to select the database connection.

15. In the “SQL Results” pane, expand the operation to see the two SQL statements that were executed. The results show excerpts from our two target tables.

16. Click on each query and select the results tab to see output from the SQL statement. Both statements should return no rows back as both are target tables should be empty.
  - “JK_LIFE.ACCOUNTS_BY_STATE” which will later on contain bank balance information aggregated by state and gender.
  - “JK_LIFE.BANK_SAVINGS” which will contain a copy of the address information from our source table “JK_BANK2.BANK_SAVINGS”.

17. Switch back to the InfoSphere “IIDR Management Console”.

18. On the “Monitoring” tab, right-click the subscription “IDR2DB” and choose “Start Mirroring”. This will start “continuous mirroring” for subscription “IDR2DB” to populate the target tables in the “JK_LIFE” database with data from the source tables according to the replication rules defined in the subscription.

19. Choose “Continuous” as the mirroring method. Please see the message at the bottom of the window that one or more tables will be refreshed when Mirroring is started. Note that since the initial status of the table mappings were set to “Refresh”, when the subscription starts, the data is first pulled from the source and loaded to the target table. Click the “OK” button.

20. Mirroring will be started. Notice that the state of the subscription changes from “Starting” to “Refresh Before Mirror” to “Mirror Continuous”

21. Now switch back to the “InfoSphere Data Architect” window. Right-click on the “03_IDR2DB_UPDATE_SOURCE.sql” script and select “Run SQL” from the context menu. Repeat step #12 to select the database connection.

22. In the “SQL Results” pane, expand the operation to see the SQL statements that were executed.

23. Click on each query and select the results tab to see output from the SQL statement.
  - The first query for the “JK_LIFE.ACCOUNTS_BY_STATE” table has been populated with aggregated data by state, gender from the source table “JK_BANK1.BANK_ACCOUNTS”. Scroll down to the record that shows “male” account holders in the state of “VA” and make note of the value in the “BANK_BALANCE” field.
  - The second query for the “JK_LIFE.BANK_SAVINGS” table has received a 1:1 copy of the address information from source table “JK_BANK2.BANK_SAVINGS”. Make note of the value in the fourth field.
  - The third and fourth queries are performing the changes on our source tables. Ignore the fifth query and the associated warning. This query simply delays the execution of the last two SQL statements by four seconds.
  - The sixth query is a re-run of the first query for the “JK_LIFE.ACCOUNTS_BY_STATE”. Scroll down to the record that shows “male” account holders in the state of “VA” and make note of the change in the value of the “BANK_BALANCE” field.
  - The last query is a re-run of the second query for the “JK_LIFE.BANK_SAVINGS” table. Make note of the change in the value of the fourth field

24. Switch back to the “IIDR Management Console” and look at our second subscription “IDR2DC”. Switch back to the “Configuration” perspective then click on the subscription “IDR2DC” in the left hand side of the window.

  Please notice that the source table is our aggregate “JK_LIFE.ACCOUNTS_BY_STATE” table and the target is not a specific table but InfoSphere DataStage Direct Connect in this case. This means that InfoSphere IIDR directly interacts with an InfoSphere DataStage job by feeding the job with only changed data.

25. Let’s look at the DataStage Properties for this subscription. Right-click on the “IDR2DC” subscription in the left hand side of the window and choose “InfoSphere DataStage Properties”

26. In the InfoSphere DataStage Properties window is defined how InfoSphere IIDR interacts with the InfoSphere DataStage job. Notice that the subscription is set to automatically start the DataStage job.

27. On the “Monitoring” tab, right-click the subscription “IDR2DC” and choose “Start Mirroring”.

28. On the “Start Mirroring” dialog box, click the “OK” button.

29. Let’s start the InfoSphere DataStage designer to have a look at the related DataStage job. Logon as user “tracy”, password ‘inf0server’. Ensure that the project is set to ‘dstage1’ before clicking ‘OK’.

30. From the repository browser open the job “IDR2DC” in the “JKLW > 08_DATA_REPLICATION” folder. Right-click on the “IDR2DC” job and select “Edit”.
  - Let’s have a look at the job design of the “IDR2DC” job.
  - Whenever changes occur on the source table InfoSphere IIDR immediately pushes the delta information through the IIDR Transaction Stage to the DataStage job.
  - Data is then processed through a so called Slowly Changing Dimension stage which writes the data to a DB2 table.
      - Whenever a new record is processed, the row is created in the target table and the VALID_FROM column of the table is populated with the current timestamp. The VALID_TO column is empty at that time, which means the record is currently valid.
      - Whenever an existing record is processed the existing record in the target table gets updated. The VALID_TO column reflects then the current timestamp and invalidates the records. In addition a new record will be created containing the updated values forming the new record which is currently valid.

31. While still in InfoSphere DataStage designer start the Run Directory utility

32. In Run Director you can later see that the InfoSphere DataStage job is started.

33. Now switch back to the “InfoSphere Data Architect” window. Right-click on the “04_IDR2DC_SOURCE.sql” script and select “Run SQL” from the context menu. Repeat step #12 to select the database connection.
  - Remember that our current source table “JK_LIFE.ACCOUNTS_BY_STATE” is also being mirrored from the table “JK_BANK1.BANK_ACCOUNTS” using the subscription IIDR2DB from the beginning of this lab.

34. Right-click on the “05_IDR2DC_TARGET.sql” script and select “Run SQL” from the context menu. Repeat step #12 to select the database connection.

35. In the “SQL Results" pane, expand the operation to see the two SQL statements that were executed. The results show excerpts from our two target tables.

36. Click on each query and select the results tab to see output from the SQL statement. Both statements should return no rows back as both are target tables should be empty.
  - “JK_LIFE.ACCOUNTS_BY_STATE” contains bank balance information aggregated by state and gender from earlier in the lab. Scroll down and make note of the “BANK_BALANCE” fields for account holders in the state of “VA”.
  - “JK_LIFE.ACCOUNTS_SCD” has now been populated with data reflecting the source table “JK_LIFE.ACCOUNTS_BY_STATE”. Please also note that the “VALID_FROM” column reflects the current timestamp and the “VALID_TO” column is empty for all rows.

37. Right-click on the “06_IDR2DC_UPDATE_SOURCE.sql” script and select “Run SQL” from the context menu. Repeat step #12 to select the database connection. This query will take at least 90 seconds to complete.

38. In the “SQL Results” pane, expand the operation to see the SQL statements that were executed.

39. Click on each query and select the results tab to see output from the SQL statement.
  - The first query is for the “JK_LIFE.ACCOUNTS_BY_STATE”. Scroll down to the records that show account holders in the state of “VA” and make note of the change in the value of the “BANK_BALANCE” fields.
  - The second query is for the “JK_LIFE.ACCOUNTS_SCD” table. Scroll down and make note of the “BANK_BALANCE” fields for the state of “VA”.
  
40. This concludes the first part of this lab. Close the “IIDR Management Console”, the “DataStage Run Director”, the “DataStage Designer Client”, and “InfoSphere Data Architect” client.

41. Open the “Start Menu” then click on the “IIDR Stop” icon.

---
