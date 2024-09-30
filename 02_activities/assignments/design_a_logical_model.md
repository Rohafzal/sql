# Assignment 1: Design a Logical Model

## Question 1
Create a logical model for a small bookstore. 📚

At the minimum it should have employee, order, sales, customer, and book entities (tables). Determine sensible column and table design based on what you know about these concepts. Keep it simple, but work out sensible relationships to keep tables reasonably sized. Include a date table. There are several tools online you can use, I'd recommend [_Draw.io_](https://www.drawio.com/) or [_LucidChart_](https://www.lucidchart.com/pages/).

## Question 2
We want to create employee shifts, splitting up the day into morning and evening. Add this to the ERD.

## Question 3
The store wants to keep customer addresses. Propose two architectures for the CUSTOMER_ADDRESS table, one that will retain changes, and another that will overwrite. Which is type 1, which is type 2?

_Hint, search type 1 vs type 2 slowly changing dimensions._

Bonus: Are there privacy implications to this, why or why not?
```
Your answer...
```
When handling customer address changes, there are two approaches:

Type 1 SCD (Overwrite the old value by new values): The new address replaces the old one, and no history is kept.

Pros:
Simpler structure and less storage.
Cons:
Previous addresses are lost, with no history available.

Type 2 SCD (Create a new additional record by row versioning): A new record is created for each address change, preserving historical data.

Pros:
Tracks address changes for historical, legal, or analytical purposes.
Cons:
More complex structure and requires more storage.

Example Table Structures:
Type 1:
Customer_ID	   Address_Line1	   City	         Postal_Code	    Country
1001	       123 Main St	       Toronto	     M5H 2N2	        Canada
1002	       456 King St	       Vancouver	 V5K 1B1	        Canada

Type 2:
Customer_ID	   Address_ID	Address_Line1	City	     Postal_Code	Start_Date	   End_Date	     Is_Current
1001	        1	        123 Main St	    Toronto	     M5H 2N2	    2022-01-01	   2023-06-30	 No
1001	        2	        789 Queen St	Toronto	     M6J 1K1	    2023-07-01	   NULL	         Yes

Privacy Implications:

Type 1 (Overwrite):
This method  limits the amount of personally identifiable information (PII) stored over time, reducing the risk of exposure.
However, if the address is updated incorrectly, there's no historical record to revert or audit.

Type 2 (Retain History):
Privacy risks increase in this architecture because historical addresses are retained indefinitely, allowing the sensitive data such as home addresses to bekept even when no longer needed. This data could be exposed if the database is compromised.
Keeping historical data could conflict with privacy regulations, such as the General Data Protection Regulation (GDPR), where customers may request to have their data erased ("Right to be Forgotten"). This method would require special handling to remove old records if a customer makes such a request.

Recommendations:
Implement a data retention policy for Type 2 to purge old addresses when no longer necessary.
Use encryption and access controls to protect sensitive and personal data.


## Question 4
Review the AdventureWorks Schema [here](https://i.stack.imgur.com/LMu4W.gif)

Highlight at least two differences between it and your ERD. Would you change anything in yours?
```
Your answer...
```
Here are some differences between my bookstore ERD and Adventureworks ERD:
1. Granularity of Relationships
Bookstore ERD: The relationships between entities are simpler. For instance, Customer has a direct relationship with Order, and the Order table links to Book, but it also includes an Order_Details table to break down individual line items in an order. The schema focuses on essential business operations.
AdventureWorks ERD: It has more complex relationships and includes additional entities to manage various levels of business operations such as SalesOrderHeader and SalesOrderDetail, which separate header information from order lines. There are also additional tables for handling things like currencies, territories, and shipping methods.
2. Employee and Shift Management
Bookstore ERD: The Shift and Employee_Shift tables handle employee shift schedules, creating a direct link between shifts and employees, indicating the hours they work.
AdventureWorks ERD: This schema lacks an explicit structure for shift management. Employees are handled through HumanResources.Employee, but no shift-related scheduling information appears to be present.

I could make the following changes for better data organization, scalibility, better employee management and easier tracking:
Adding Payment or Shipment Include a Payment table to track payment methods, statuses, and details, or a Shipment table to handle deliveries, if the bookstore offers those services.
Additional Employee Information: Add further details to the Employee table to include information like hire date, department, or job title, similar to the more detailed approach in the AdventureWorks schema.

# Criteria

[Assignment Rubric](./assignment_rubric.md)

# Submission Information

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

### Submission Parameters:
* Submission Due Date: `September 28, 2024`
* The branch name for your repo should be: `model-design`
* What to submit for this assignment:
    * This markdown (design_a_logical_model.md) should be populated.
    * Two Entity-Relationship Diagrams (preferably in a pdf, jpeg, png format).
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/sql/pull/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `model-design`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via our Slack at `#cohort-4-help`. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.
