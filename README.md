
# Educational Organisation Management System using ServiceNow

##  Project Overview

The **Educational Management System (EMS)** is a streamlined and customized solution developed on the **ServiceNow platform** to enhance administrative operations within educational institutions. This system enables:

- Centralized management of student, teacher, and admission data
- Automated workflows for accurate and consistent processes
- Easy tracking of academic progress
- A user-friendly and secure interface for data interaction

By leveraging the low-code/no-code features of ServiceNow, EMS provides institutions with an efficient digital solution to replace traditional, manual processes.

---

##  Setting Up the ServiceNow Instance

1. **Sign Up for Developer Access**
   - Visit: [https://developer.servicenow.com](https://developer.servicenow.com)
   - Create a free developer account

2. **Request Personal Developer Instance**
   - Navigate to **Manage > Instance**
   - Click **Request Instance** and select the latest release
   - Access the instance using credentials sent via email

3. **Login to Your Instance**
   - Use the provided URL and credentials to log in and begin development

---

##  Creating an Update Set

- Navigate to **All → Local Update Sets**
- Click **New**, name it `Educational Organisation`
- Click **Submit** and then **Make Current**

>  All configuration changes like table creation, form layout, and scripts are tracked in this update set.

---

##  Table Design

### 1. **Salesforce Table** (Base Student Table)

- Fields:  
  - Admin Number (Dynamic Default: Get Next Padded Number)  
  - Grade (Choice: Primary, Secondary, etc.)  
  - Student Name, Admission Date, Contact Numbers, etc.  
- Mark as **Extensible**

---

### 2. **Admission Table** (Extends Salesforce)

- Tracks admission-specific details
- Fields include:
  - Admission Number
  - School, School Area, Pincode
  - Admin Status (Choice)
  - Purpose of Join, Parent Information

---

### 3. **Student Progress Table**

- Tracks academic performance
- Fields:
  - Admission Number (Reference)
  - Marks (Telugu, Hindi, English, Maths, Science, Social)
  - Total Marks, Percentage, Result

---

##  Configuring Forms and Layout

- Navigate to: **System Definition → Tables → [Your Table] → Configure → Form Design**
- Arrange fields for logical data entry
- Enabled module visibility for quick access in Application Navigator

---

##  Number Maintenance (Admin Numbers)

- Navigate to: **All → Number Maintenance**
- Create a number format like: `ADM0001`
- Auto-increment ensures unique Admin Numbers

---

##  Process Flow for Admissions

- Navigate to: **Process Flow → New**
- Created stages:
  - `New → InProgress → Joined → Rejected → Rejoined → Closed → Cancelled`

>  Helps visualize and control the admission lifecycle

---

##  Client Scripts for Automation

### 1. **Auto-Populate Admission Fields**

Auto-fills fields like Grade and Student Name on selecting Admission Number.

```javascript
function onChange(control, oldValue, newValue, isLoading, isTemplate) {
  if (isLoading || newValue === '') return;
  var admission = g_form.getReference('u_admission_number');
  g_form.setValue('u_grade', admission.u_grade);
  g_form.setValue('u_student_name', admission.u_student_name);
}

