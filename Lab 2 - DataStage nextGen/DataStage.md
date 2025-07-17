#  IBM DataStage Core Lab

### 🔑 Environment Access

Use the cluster link provided by your instructor.

1.  **Access the shared cluster 1:**
    *   URL: **https://ibm.biz/BdnLHM**
    *   Username: Key in your student id (`student1` – `student20`)

    **Access the shared cluster 2:**
    *   URL: **https://ibm.biz/BdntC9**
    *   Username: Key in your student id (`student21` – `student40`)

2.  Enter `IDtechxchange` for the password.

---

## ⚙️ Lab Setup (Building the flow)

To begin, download the **DataStage Lab Project ZIP file** from Box in the link provided below.

* https://ibm.ent.box.com/s/th0lz0x2eqa34duiqa9zc66g83mek988

To get started, head to the shared SW cluster homepage. Navigate to **projects -> all projects**. Create a new project from file.

![0.All Project](https://github.com/Client-Engineering-Indonesia/workshop-cdc-datastage/blob/main/Lab%202%20-%20DataStage%20nextGen/Assets/0.All%20Project%201.png)

## 💡 Explainer - DataStage Projects

 A **"Project"** is a collaborative workspace in Cloud Pak for Data where you work with data and other assets to accomplish a particular goal. In this case, we are using projects to transform and integrate data with DataStage.

1. Inside the **Create a Project**.

2. select **Local file** and upload the downloaded ZIP file in the folder of this repository.

![1.Upload ZIP file]()

3. After the DataStage Core Lab Project ZIP file is uploaded, provide a unique name for your project (e.g., `DataStage Core Lab_YourInitials`).

![2.Naming the project with initials.]()

4. Click the **Create** button to create the project.


> **Tip:** Once inside the Assets view, sorting the assets by **Name** in ascending order will assist you in navigating the contents inside the Project.

---

## 📂 Create DataStage Flows

1. Click into the **NEW Flow** to get started building your DataStage job!

![3.Click new flow button]()

>**Rename these DataStage flows to include your initials.** This will help in identifying your assets.

## 💡 DataStage Flows Explanation

**DataStage flows** are the design-time assets that contain data integration logic. The basic building blocks of a flow are:
*   Data sources that read data
*   Stages that transform the data
*   Data targets that write data
*   Links that connect the sources, stages, and targets

This Core Lab section is divided of two DataStage flows in the project.

1. **NEW Flow**
*   The **NEW Flow** is the starting point for building the core capabilities of DataStage in the Core Lab.

2. **COMPLETED Flow**.

*   The **COMPLETED  Flow** is the final output of the Core Lab, allowing you to have a reference example of the completed flow.

---

## 👉 Task 1: Run an existing DataStage Flow

Let's start with a basic DataStage flow that joins the mortgage applicants and mortgage applications data sets.

The outputs that result to a CSV file in the project. Follow these steps to run the DataStage flow:

1.  Click your **DataStage Core Lab_Your Initials** project.
    
    *   If you don't have the project open, follow these steps:
        a. From the Navigation menu, choose **Projects > View all projects**.

        ![0.All Project]()
    
        b. Open your data integration project.
        
2.  Click the **Assets** tab to see all the assets in the project.

    ![4.Assets page]()

3.  Click **Flows > DataStage flows**.

    ![5.Data Stage FLows]()

4.  Click the **Core Demo NEW Flow_your initials** flow in the list to open it. 

    * This flow joins the `Mortgage Applicants` and `Mortgage Applications` tables that are stored in Db2 Warehouse, filters the data to those records from the State of California, and creates a sequential file in CSV format as the output.

    

5.  Click the **zoom in** and **zoom out** icons on the toolbar to set your preferred view of the canvas.

![6.Zoom in and Zoom out Button]()

6.  Double-click the `MORTGAGE_APPLICATIONS_1` node to view the settings.

    a. Expand the **Properties** section.

    b. Scroll down to the bottom, then click **Preview data**. This data set includes information that is captured on a mortgage application.

    c. Click **Close**.

7.  Double-click the `MORTGAGE_APPLICANTS_1` node to view the settings.

    a. Expand the **Properties** section.

    ![7.Mortgage applicants properties expand]()

    b. Scroll down, and click **Preview data**. This data set includes information about mortgage applicants who applied for a loan.

    c. Optional: Visualize the data.

    * Click the **Chart** panel.

     ![8.Chart Panel]()

    * In the **Columns to visualize** list, select **STATE**.

    ![9.Column to visualize]()

    * Click **Visualize data** to see a pie chart showing the distribution of the data by state.

    * Click the **Treemap** icon to see the same data in a treemap chart.

    d. Click **Close**.

8.  Double-click the `Join_on_ID` node to view the settings.
    a. Expand the **Properties** section.

    b. Note that the join key is the `ID` column.

    c. Click **Cancel** to close the settings.

    

9.  Click the **Logs** icon on the toolbar so you can watch the flow's progress.

![10.Logs Button]()

10. Click **Compile**, and then click **Run**. The run can take about one minute to complete.

![11.Compile Button]()


11. View the logs. You can use the total rows and rows/sec for each step in the flow to visually verify that the filter is working as expected.

12. When the run completes successfully, click **DataStage Core Lab_<Your Initials>** in the navigation trail to return to the project.

![11.SS the number 12]()

13. On the **Assets** tab, click **Data > Data assets**.

14. Open the `MORTGAGE_DATA.CSV` file. You can see that this file contains the columns from both the mortgage applicants and mortgage applications data sets.

___

### ✅ Check your progress

The following image shows the resulting CSV file. The next task is to edit the DataStage flow.

![11.SS Result of the processed csv]()

---

## 🗺️ Overview: Edit the DataStage Flow

Now that you joined the mortgage applicant and application data, It is possible to you to things below:

1. Specify a key column for the Join stage.

2. Add credit score data from a PostgreSQL database.

3. Add a Join stage to join the credit score data with the applicant and application data.

4. Add a Transformer stage to calculate total debt.

5. Add interest rate data from a MongoDB database.

6. Add a Lookup stage to look up interest rates for applicants based on their credit scores.

all this possibilitiies have the hands on within this document below.

> #### 💡 Explainer - DataStage Stages
> A DataStage flow consists of **stages** that are linked together, which describe the flow of data from a data source to a data target. A stage describes a data source, a processing step, or a target system.

---

# 👉 1. Specify the key column for the Join stage

The `Join_on_ID` node joins the data sets using the `ID` column. The next phase is to join the resulting data set with the credit score data using the `EMAIL_ADDRESS` column. In this task, you will specify `EMAIL_ADDRESS` as a key column. 

**Below are steps to do the explanation above:**

1.  Return to your project by clicking the project name in the navigation trail.'

![12. where to press the navigation Trail]()

2.  On the **Assets** tab, click **Flows > DataStage flows**.

![13. where to find the assests]()

3.  Open the **Core Demo NEW Flow_your initials** flow.

4.  Double-click the `Join_on_ID` node to edit the settings.

5.  Click the **Output** tab, and expand the **Columns** section.

6.  Click **Edit**.

7.  For the `EMAIL_ADDRESS` column name, check the **Key** box.

![14. Click Email address]()

8.  Click **Apply and return**.

9.  Click **Save** to save the `Join_on_ID` node settings.

### ✅ Check your progress

---

The following image shows the DataStage flow is ready for the next step. Now you can add the PostgreSQL data containing the applicants' credit scores.

![15. Layout SS Like original document]()


> #### 💡 Explainer - Asset Browser for DataStage
> The **Asset browser** is used to search for connections and assets and add them to your DataStage flows. You can browse connectors, DataStage subflows, and data assets (.csv, .txt, .xls, etc.). The data can then be previewed before being onboarded to the DataStage flow.


---

# 👉 2.Add credit score data from a PostgreSQL database

1.  In the node palette on the left, expand the **Connectors** section.

2.  Drag the **Asset browser** connector to the canvas beside the `MORTGAGE_APPLICANTS_1` node.

3.  Locate the asset by selecting **Connection > Trial Connection - Databases for PostgreSQL > BANKING > CREDIT_SCORE**.
    > **Note:** Click the connection or schema name itself (not the checkbox) to expand the tree.

4.  Click the **Preview** icon (👁️) to preview the credit score data.

5.  Click **Add**.

### ✅ Check your progress

The following image shows the DataStage flow with the credit score asset added. Now you need to join the applicant, application, and credit score data.



---

# 👉 3. Add a Join stage to join the credit score data

1.  In the node palette, expand the **Stages** section.
2.  Drag the **Join** stage onto the canvas, and drop the node on the link line between the `Filter_State_Code` and `Sequential_file_1` nodes.
3.  Hover over the `CREDIT_SCORE_1` connector to see the arrow. Connect the arrow to the new **Join** stage.
4.  Double-click the `CREDIT_SCORE_1` node to edit the settings.
    a. Click the **Output** tab and expand the **Columns** section.
    b. Click **Edit**.
    c. For the `EMAIL_ADDRESS` and `CREDIT_SCORE` column names, select **Key**.
    d. Click **Apply and return**.
    e. Click **Save**.
5.  Double-click the new **Join\_1** node to edit the settings.
    a. Expand the **Properties** section.
    b. Click **Add key**.
    c. Click **Add key** again and select `EMAIL_ADDRESS` from the list.
    d. Click **Apply**.
    ![15. same phot as in the original document in task 4]()
    e. Change the `Join_1` node name to `Join_on_email`.
    f. Click **Save**.

### ✅ Check your progress

The following image shows the DataStage flow with a second Join stage added. Now you need to add a Transformer stage to calculate each applicant's total debt.

![16. same photo as in real document]()

---

# 👉 4. Add a Transformer stage to calculate total debt

1.  In the **Stages** section, drag the **Transformer** stage onto the link line between the `Join_on_email` and `Sequential_file_1` nodes.
2.  Double-click the **Transformer** node to edit its settings.
3.  Click the **Output** tab.

    * Click **Add column**.

    * Scroll down and name the new column `TOTAL_DEBT`.
    * Click the **Edit** icon (✎) in the row's **Derivation** column.
    * Click the **Calculator** icon (🧮) to open the expression builder.
    * Search for `LOAN_AMOUNT`, and double-click the column name to add it.
    * Type a plus sign `+`.
    * Search for `CREDITCARD_DEBT`, and double-click the column name to add it.
    * Verify the final expression is `Link_7.LOAN_AMOUNT + Link_7.CREDITCARD_DEBT` (your link number may be different).
    * Click **Apply and return**. If you see an error, change the column **Data type** to **nvarchar**.
    * For the `CREDIT_SCORE` column name, scroll to the right and select **Key**.

4.  Click the **Stage** tab.
    * Select the **Advanced** page.
    * Change the **Execution mode** to **Sequential**.

5.  Click **Save and return** to return to the canvas.

---

# 👉 5. Add interest rate data from a MongoDB database

1.  In the node palette, expand the **Connectors** section.
2.  Drag the **Asset browser** connector to the canvas beside the `CREDIT_SCORE_1` node.
3.  Locate the asset by selecting **Connection > Trial Connection - Mongo DB > DOCUMENT > DS_INTEREST_RATES**.
4.  Click the **Preview** icon (👁️) to preview the interest rates for each credit score range.
5.  Click **Add**.

![17. same photo as in documents]()
---

# 👉 6. Add a Lookup stage to look up interest rates

1.  In the **Stages** section, drag the **Lookup** stage onto the link line between the `Transformer_1` and `Sequential_file_1` nodes.
2.  Connect the `DS_INTEREST_RATES_1` connector to the **Lookup\_1** stage.
3.  Double-click the `DS_INTEREST_RATES_1` node to edit the settings.
4.  Click the **Output** tab.
    a. Expand the **Columns** section and click **Edit**.
    b. Select the `_ID` column and click the **Delete** icon (🗑️).
    c. Click **Apply and return**, then **Save**.
5.  Double-click the `Lookup_1` node to edit its settings.

6.  Expand the **Properties** section.

    * For the **Apply range to columns** field, select `CREDIT_SCORE`.

    * For the **Reference Links**, select the link coming from the interest rates table (e.g., `Link_9`).

    * For the first **Operator**, select **<=**, and for the **Range column**, select `ENDING_LIMIT`.

    * For the second **Operator**, select **>=**, and for the **Range column**, select `STARTING_LIMIT`.

    ![17. same photo as in the document showing tge]()

7.  Click the **Output** tab.
    a. Expand the **Columns** section and click **Edit**.
    b. Select the `STARTING_LIMIT` and `ENDING_LIMIT` columns.
    c. Click the **Delete** icon (🗑️) to delete these unnecessary columns.
    d. Click **Apply and return**.
8.  Click **Save**.

> #### 💡 Explainer - Column metadata change propagation
> When you add/remove columns, **Column metadata change propagation** automatically propagates these changes downstream. For example, when the `STARTING_LIMIT` and `ENDING_LIMIT` columns were deleted, these changes are propagated to the output Sequential File automatically.

---

# 👉 7 Edit the Sequential file node and run the DataStage flow

1.  Double-click the `Sequential_file_1` node to edit its settings.
2.  Click the **Input** tab.
3.  Expand the **Properties** section.
4.  For the **Target File**, enter `MORTGAGE_APPLICANTS_INTEREST_RATES.CSV`.
5.  Select **Create data asset**.
6.  For the **First line is column names** field, select **True**.
7.  Click **Save**.
8.  Click **Run** to compile and run the DataStage flow. The job takes about 1 minute.
9.  Click **Logs** on the toolbar to watch the flow's progress. It is normal to see warnings.

### ✅ Check your progress

The following image shows that the DataStage flow ran successfully!

![18.Same photo as in the document that shows the logs of the datas.]()

10. Hover over the sequential file node, click the ellipses (...) and select **Preview data**. The CSV file now includes all applicants and their personalized interest rates.

![19. same photo as in the document that show how to preview the datas.]()
---

## 🏁 Conclusion and Next Steps

This section covered the core functionalities of DataStage on Cloud Pak for Data, including how to create projects and modify an existing flow to provide a personalized interest rate for each mortgage applicant. Key concepts and features of DataStage's modernized developer experience were covered – including the Asset Browser and Column Metadata Change Propagation.

## 🎉 Thank you for the participation, Have a pleasent day!.
