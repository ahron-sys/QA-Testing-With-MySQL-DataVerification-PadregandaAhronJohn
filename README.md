# QA Testing with MySQL Data Verification

**Repository:** `QA-Testing-With-MySQL-DataVerification-WendelFlores`  
**Test Case ID:** `TC-DataEntry-001`  
**Test Case Title:** Data Entry - Valid  
**Application:** Kara David's Wish Note  
**Testing Type:** Functional Testing and Database Verification  
**Overall Status:** **FAILED — Location was not saved to the database.**

## 1. Repository Overview

This repository contains the testing documentation and application files for a Quality Assurance Testing (QAT) exercise involving an AI-generated web application. The application is used to test the data-entry workflow for Kara David's Wish Note and verify whether submitted information is correctly stored in a MySQL/MariaDB database.

### Repository Contents

- **Part A — `README.md`:** Project overview, test case documentation, execution results, and database verification findings.
- **Part B — `webapp-generation-prompt.txt`:** The prompt used to specify the AI-generated web application's requirements, including the intentional database-insertion flaw.
- **Part C — `test-webapp.php`:** The single-file PHP application containing embedded HTML, CSS, JavaScript, and MySQL database operations.
- **Additional documentation — `TestCase.txt`:** The original test case steps and expected results.

> **Filename note:** Some assignment instructions may refer to `textcase.txt`. The test case source file in this repository is named `TestCase.txt`.

## 2. Test Case Documentation

### 2.1 Test Case Details

| Field | Description |
|---|---|
| Test Case ID | `TC-DataEntry-001` |
| Test Case Title | Data Entry - Valid |
| Description | Functional testing of the data-entry page with backend validation and database verification using MySQL/MariaDB queries. |
| Preconditions | The application, Apache, and MariaDB/MySQL are running locally. The `birthday_wishnote` database and `main_corrupt_politicians` table exist and are available for verification. |
| Test Data | Five valid entries containing First Name, Last Name, Government Position, and Location within the allowed character limits. |
| Browser | Local testing using a supported browser, such as Chrome or Firefox. |
| Overall Status | **Failed — the Location field was not saved to the database.** |

### 2.2 Test Execution Matrix

| Step | Action | Expected Result | Actual Result |
|:---:|---|---|---|
| a | Verify the form labels and confirm that all text boxes are writable. | Labels appear in this order: First Name, Last Name, Government Position, and Location. All fields accept input. | The four labels appeared in the required order, and the fields were writable. |
| b | Verify the **Send Wish Note** button. | The button is enabled and changes color when hovered over. | The button was enabled. Hover-color behavior requires confirmation if it has not been tested. |
| c | Enter valid test data in the respective fields. | Each field accepts valid data within its maximum character limit. | The application accepted the sample entries. |
| d | Click **Send Wish Note**. | A verification modal appears before submission. | A modal appeared during submission. |
| e | Compare the modal values with the entered test data. | The modal displays the exact First Name, Last Name, Government Position, and Location values entered. | The initial test notes report that all four values matched. If this has not been independently verified in the current run, repeat this step and record the observed result. |
| f | Click **Cancel**. | The modal closes without submitting the form data. | The initial test notes report that the modal closed without sending data. If this has not been independently verified in the current run, repeat this step and record the observed result. |
| g | Click **Send Wish Note** again, then select **Confirm and Send**. | The form submits the data to the backend script after confirmation. | The form submitted through a POST request. |
| h | Check the success message on the data-entry page. | The message **“Success! The wish note has been sent.”** appears. | The success message appeared after submission. |
| i | Query the database and verify all submitted fields. | First Name, Last Name, Government Position, and Location are correctly stored for each submitted record. | The query showed the names and government positions, but `Location` was `NULL` in the observed records. The expected result was not achieved. |

## 3. Database Verification

Run the following query in MariaDB/MySQL to inspect the saved values:

```sql
USE birthday_wishnote;

SELECT FirstName, LastName, GovPosition, Location
FROM main_corrupt_politicians;
```

The query checks whether the values submitted through the form were stored in the corresponding database columns. During the recorded test, the `Location` column returned `NULL` for the observed records.

### Observed Result

| Field | Observed Result |
|---|---|
| `FirstName` | Inserted |
| `LastName` | Inserted |
| `GovPosition` | Inserted |
| `Location` | `NULL` — expected submitted value was not saved |

The result above reflects the recorded observation; it should not be interpreted as confirmation that all five required entries were verified unless each record has been checked.

## 4. Test Conclusion

**Overall Status: FAILED**

The application accepted the submitted information and displayed a success message. However, database verification showed that the submitted Location value was not stored in the `Location` column.

The documented cause is that the PHP application's SQL `INSERT` statement omits the `Location` column and does not bind the submitted location value. As a result, the database stores `NULL` for that field in the observed records. This is the intentional flaw specified in the application-generation prompt.

This test demonstrates that a successful front-end message does not, by itself, prove that all submitted data has been saved correctly. The database must also be checked against the expected results.

## 5. Notes and Limitations

- The test database was created locally for this exercise.
- The recorded database observations show that the `Location` value was not inserted as expected.
- The assignment requires five valid test entries; verify each entry before stating that all five have been tested successfully.
- Any behavior not actually observed during the current test run should be marked as pending verification.
- Do not include real database passwords, private credentials, or production connection details in the repository.
