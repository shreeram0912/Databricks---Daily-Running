# Databricks - Job (API to CSV)
This repository contains a daily running job pipeline built on Databricks.
The pipeline fetches data from an API, validates it, and writes the results into a CSV file stored in a Databricks volume. It is designed with retry logic, error handling, and notifications.

### **📅 Schedule**
* The pipeline is scheduled to run every day at 8:00 AM.
* If something goes wrong, an email notification is triggered automatically.

### **🔄 Workflow**

1. Test Notebook (test_daily_pipeline)
   * Runs first to validate the API response and schema.
   * If the test fails, the main pipeline will not run.
   * A notification is sent to alert for repair and rerun.

2. Production Notebook (production_daily_pipeline)
   * Runs only if the test notebook succeeds.
   * Fetches data from the API.
   * Converts JSON (including nested JSON) into a Spark DataFrame.
   * Writes the DataFrame into CSV format in the specified Databricks volume.

3. Retry Logic
   * If the API call or schema conversion fails, the notebook retries up to 3 times with a short wait between attempts.
   * If all retries fail, the pipeline stops and sends a failure notification.

### **📧 Notifications**
* Failure → Email alert with error details.
* Success → Confirmation that data has been ingested and written.
  <img width="1188" height="747" alt="image" src="https://github.com/user-attachments/assets/a694658e-ea28-424f-8b79-42d4f54610c1" />
