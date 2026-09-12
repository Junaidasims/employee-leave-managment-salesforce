# Employee Leave Management System — Salesforce

A Salesforce-based Employee Leave Management System that automates employee leave requests, manager assignment, approval/rejection, leave balance deduction, and cancellation.

## Project Overview

This project was developed using Salesforce declarative tools to automate the employee leave management process.

The system allows employees to create leave requests and provides managers with a workflow to review and approve or reject requests.

When a leave is approved, the employee's leave balance is automatically deducted. If an approved leave is cancelled, the deducted leave balance is automatically restored.

## Key Features

- Employee management
- Leave request creation
- Automatic manager assignment
- Automatic leave-day calculation
- Leave balance validation
- Manager approval and rejection
- Automatic leave balance deduction
- Approved leave cancellation
- Automatic balance restoration
- Reports and dashboard
- Custom Lightning App

## Salesforce Objects

### Employeee

Stores employee information.

Fields include:

- Employee ID
- Employeee Name
- Leave Balance
- Phone
- Email
- Department
- Manager

### Leave Requestt

Stores employee leave requests.

Fields include:

- Leave Requestt Number
- Employeee
- Start Date
- End Date
- Leave Type
- Other Leave Reason
- Manager
- Request Status
- Requested Days
- Employee Leave Balance

The Employeee and Leave Requestt objects are connected using a Lookup Relationship.

## Automation

The project uses five Salesforce Flows.

### 1. Auto Assign Manager

A Record-Triggered Flow that runs when a new Leave Requestt is created.

It:

1. Gets the related Employeee record.
2. Retrieves the employee's manager.
3. Updates the Leave Requestt Manager field automatically.

### 2. Deduct Leave Balance

A Record-Triggered Flow that runs when a Leave Requestt becomes Approved.

It:

1. Gets the related Employeee record.
2. Calculates the new leave balance.
3. Subtracts Requested Days from the current Leave Balance.
4. Updates the Employeee record.

Formula:

`Current Leave Balance - Requested Days`

### 3. Manager Review Leave Request

A Screen Flow used by the manager to review a leave request.

The manager can:

- View employee details
- View leave dates
- View leave type
- View requested days
- View available leave balance
- Approve or reject the request

The Flow then updates Request Status to Approved or Rejected.

### 4. Restore Leave Balance

A Record-Triggered Flow that runs when a request is cancelled.

It checks the previous status using `$Record__Prior`.

Balance is restored only when:

`Approved → Cancelled`

Formula:

`Current Leave Balance + Requested Days`

This prevents leave from being restored for requests that were never approved.

### 5. Cancel Leave Request

A Screen Flow that allows an approved leave request to be cancelled.

It:

1. Gets the selected Leave Requestt.
2. Checks whether the request is Approved.
3. Displays a cancellation confirmation.
4. Changes Request Status to Cancelled.

The Restore Leave Balance Flow then restores the employee's leave balance.

## Validation Rules

### End Date Validation

Prevents the End Date from being earlier than the Start Date.

### Other Leave Reason Validation

Requires a reason when Leave Type is set to Other.

### Leave Balance Validation

Prevents an employee from requesting more leave days than their available leave balance.

## Formula Fields

### Requested Days

Calculates the number of leave days:

`End Date - Start Date + 1`

### Employee Leave Balance

Displays the current leave balance from the related Employeee record.

## Reports

### Leave Requests by Status

Groups leave requests by:

- Pending
- Approved
- Rejected
- Cancelled

### Employee Leave Balance

Displays employee leave balances and provides a bar chart for visualization.

## Dashboard

The Employee Leave Dashboard contains:

- Leave Requests by Status — Donut Chart
- Employee Leave Balance — Bar Chart

## Lightning App

A custom Lightning App named:

**Employee Leave Management**

Navigation:

- Employeees
- Leave Requestts
- Reports
- Dashboards

## End-to-End Workflow

Employee creates a leave request.

↓

Manager is automatically assigned.

↓

Request starts as Pending.

↓

Manager reviews the request.

↓

Approved → Leave balance is deducted.

↓

Rejected → No deduction occurs.

↓

Approved request can be cancelled.

↓

Cancelled approved request → Leave balance is restored.

## Example

Employee leave balance:

`25 days`

Leave request:

`3 days`

After approval:

`25 - 3 = 22 days`

After cancellation:

`22 + 3 = 25 days`

## Technologies & Salesforce Features

- Salesforce Lightning Platform
- Custom Objects
- Custom Fields
- Lookup Relationships
- Formula Fields
- Validation Rules
- Record-Triggered Flows
- Screen Flows
- Quick Actions
- Reports
- Dashboards
- Lightning App

## Project Type

Salesforce Declarative Automation Project

No Apex was required for the implemented business requirements.

## Project Demonstration

Screenshots of the application, records, dashboard, and automation are included in this repository.

## Author

MD Abdul Junaid Asim
