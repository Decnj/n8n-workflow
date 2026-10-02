# Webhook Lead Formatter 

## Workflow Name 

Webhook Lead Formatter 

--- 

## Workflow Objective 

This workflow receives lead information through an HTTP POST webhook endpoint, validates required fields, verifies email format using a regular expression, checks for duplicate leads using Google Sheets, stores valid leads, and returns an appropriate response to the sender. 

The workflow demonstrates core n8n concepts including:  

- Webhooks 
- HTTP POST requests 
- Data transformation 
- Field mapping 
- Conditional logic 
- Email validation 
- Google Sheets integration 
- Duplicate detection 
- Error handling 
- API responses 

--- 

## Workflow Logic 

1. A Webhook node receives incoming lead submissions. 
2. An Edit Fields node standardises incoming data into a consistent structure. 
3. An IF node validates that required fields are present: 
   - first_name 
   - email 
   - company 
4. A second IF node validates the email format using a regex expression. 
5. A Google Sheets node searches for an existing lead using the submitted email address. 
6. A third IF node checks whether the email already exists: 
   - If found, the workflow returns a duplicate lead response. 
   - If not found, the lead is saved to Google Sheets. 
7. A response is returned to the requesting application. 

--- 

## Workflow Design 

```text 

Webhook (POST) 
      ↓ 
Edit Fields 
      ↓ 
IF 
(Required Field Validation) 
      ↓ 
IF 
(Email Regex Validation) 
      ↓ 
Get Row(s) In Google Sheet 
      ↓ 
IF 
(Email Exists?) 

  ├─ TRUE 
  │     ↓ 
  │ Lead Already Exists 
  │     ↓ 
  │ Respond to Webhook 
  │ 
  └─ FALSE 
        ↓ 
   Append Row 
        ↓ 
   Lead Saved 

``` 
--- 

## Workflow Components 

### 1. Webhook (POST) 

The workflow begins with a Webhook node configured to receive HTTP POST requests. 

Configuration: 

- Method: POST 
- Path: `/lead` 
 
This acts as the entry point for incoming lead submissions. 

Example payload: 

```json 
{ 
  "first_name": "Sarah", 
  "surname": "Doe", 
  "email": "sarah@example.com", 
  "phone": "+41-333-333-333", 
  "company": "ABC Logistics" 
} 

``` 
--- 

### 2. Edit Fields (Manual Mapping) 

The Edit Fields node standardises incoming webhook data into a predictable structure. 

| Field | Source | 
|---------|---------| 
| first_name | `$json.body.first_name` | 
| surname | `$json.body.surname` | 
| email | `$json.body.email` | 
| phone | `$json.body.phone` | 
| company | `$json.body.company` | 

Example output: 

```json 
{ 
  "first_name": "John", 
  "surname": "Doe", 
  "email": "john.doe@example.com", 
  "phone": "+41333333333", 
  "company": "ABC Ltd" 
} 

``` 
--- 

### 3. Required Field Validation 

The first IF node checks that the following fields are present: 

- first_name 
- email 
- company 

All validation checks must pass before processing continues. 
Validation logic implemented using AND conditions.

--- 

### 4. Email Format Validation 

The second IF node validates the submitted email address using a regular expression.  

Accepted examples: 

```text 
john@example.com 
john.doe@example.com 
user123@company.org 
``` 

Rejected examples: 

```text 
john@ 
example.com 
john..doe@example.com 

```  

If validation fails, the workflow returns: 

```json 
{ 
  "status": "error", 
  "message": "Invalid email format" 
} 

``` 
--- 

### 5. Duplicate Lead Detection 

The Google Sheets node performs a lookup using the submitted email address. 

Lookup rule: 

```text 
email = submitted email 
``` 

The workflow searches the Google Sheet for an existing record matching the incoming email. 

If a matching record exists: 

```json 
{ 
  "status": "Failed", 
  "message": "Lead already exists" 
} 
``` 
The lead is not saved again. 

--- 
 
### 6. Google Sheets Storage 

If no matching email is found:  

- first_name 
- surname 
- email 
- phone 
- company 

are appended to the Google Sheet. 
This creates a persistent lead database and prevents duplicate entries. 

--- 

## Response Behaviour 

### Valid Lead 

When all validation checks pass and no duplicate email exists: 

```json 
{ 
  "status": "success", 
  "message": "Lead accepted" 
} 

``` 
--- 

### Duplicate Lead 

Returned when a matching email already exists in the sheet: 

```json 
{ 
  "status": "Failed", 
  "message": "Lead already exists" 
} 

``` 
--- 

### Invalid Email 

Returned when regex validation fails: 

```json 
{ 
  "status": "error", 
  "message": "Invalid email format" 
} 

``` 
--- 

### Missing Required Fields 

Returned when one or more mandatory fields are missing: 

```text 
Validation Failed 
 
Lead submission could not be processed because one or more required fields are missing. 

Required fields: 
- first_name 
- email 
- company 

Please verify and resubmit the request with all mandatory fields included. 

``` 
--- 

## Google Sheet Structure 

The workflow stores lead information using the following columns:  

| Column | 
|----------| 
| first_name | 
| surname | 
| email | 
| phone | 
| company | 

---  

## Sample Test Payload 

```json 
{ 
  "first_name": "John", 
  "surname": "Doe", 
  "email": "john.doe@example.com", 
  "phone": "+41333333333", 
  "company": "ABC Ltd" 
} 

``` 
--- 

## Testing Performed 

| Test Case | Expected Result | Status | 
|------------|----------------|---------| 
| Valid lead submission | Lead saved successfully | ✅ Pass | 
| Missing email | Validation error returned | ✅ Pass | 
| Missing first_name | Validation error returned | ✅ Pass | 
| Missing company | Validation error returned | ✅ Pass | 
| Invalid email format | Lead rejected | ✅ Pass | 
| Duplicate email | Duplicate response returned | ✅ Pass | 
| New unique email | Lead saved to sheet | ✅ Pass | 
| Optional phone omitted | Lead accepted | ✅ Pass | 
| Optional surname omitted | Lead accepted | ✅ Pass | 

--- 

## Testing Method 

The workflow was tested using: 

- n8n Test Webhook URL 
- HTTP POST requests 
- Sample JSON payloads 

Example: 

```bash 
curl -X POST http://localhost:5678/webhook-test/lead \ 
-H "Content-Type: application/json" \ 
-d '{ 
  "first_name":"John", 
  "surname":"Doe", 
  "email":"john.doe@example.com", 
  "phone":"+41-333-333-333", 
  "company":"ABC Ltd" 
}' 

``` 
--- 
 
## Outcome 

The workflow successfully: 

- Receives lead data through a webhook. 
- Standardises incoming data. 
- Validates required fields. 
- Performs email format validation. 
- Prevents duplicate lead creation. 
- Stores valid leads in Google Sheets. 
- Returns structured API responses. 

This project demonstrates a practical lead capture and validation workflow that can serve as a foundation for CRM and sales automation solutions. 
