---
title: Enrollment API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='jake@bestow.co'>Sign Up for a Developer Key</a>

# includes:
#   - errors

search: true
---

# Introduction

Welcome to the Bestow Entrollment API. Users of the API can create policies for Bestow life insurance products by creating an application, completing all sections, and submitting payment information.

# Authentication

> To authorize, use this code:

```shell

curl -X POST \
  https://api.hellobestow.com/v2/applications \
  -H 'Authorization: 82zZIHeBqUlBtICMX5li' \
  -H 'Content-Type: application/json' \
```


> Make sure to replace `82zZIHeBqUlBtICMX5li` with your API key.

Bestow uses API keys to allow access to the API. You can request a new Bestow API key by sending an [email to us](mailto:jake@bestow.co). 

Bestow expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: 82zZIHeBqUlBtICMX5li`

<aside class="notice">
You must replace <code>82zZIHeBqUlBtICMX5li</code> with your personal API key.
</aside>










# Enrollment

In order to get a customer enrolled for life insurance at Bestow, first you must create an application.  The application has 7 sections that need to be completed before it can be submitted.

The sections are: 

1. Personal - name, age, gender, SSN, etc
2. Policy - term of the life insurance policy and its face value
3. Medical - questions about medical history (i.e. history with diabetes, stroke, etc)
4. Lifestyle - question about lifestyle (i.e. smoking, travel plans, citizenship, etc)
5. Beneficiaries - information about the applicant's beneficiaries
6. Billing - credit card information
7. Follow-up - after the application is submitted, there may be additional follow-up questions.










## Create Application

`POST https://api.hellobestow.com/v2/applications`

> Endpoint URL

> **POST https://api.hellobestow.com/v2/applications**

> Example Request

```shell

curl -X POST \
  https://api.hellobestow.com/v2/applications \
  -H 'Authorization: 82zZIHeBqUlBtICMX5li' \
  -H 'Content-Type: application/json' \
  -H 'Accept:application/json' \
  -H 'cache-control: no-cache' \
```

> Example Response

```json
{
  "application_id": "4asvn9482haSDf28a",
  "state": "incomplete",
  "completed_sections": {
    "personal": false,
    "policy": false,
    "billing": false,
    "beneficiaries": false,
    "medical": false,
    "lifestyle": false,
    "followup": true
  }
}
```


You will create an application by passing the API a POST request.

### Query Parameters

None

### Return values

The call will receive a JSON object with the `application_id` and the status on the completed sections.  If a completed section is `false`, it will need to be completed before `state` will be go from `imcomplete` to `ready_to_submit`.


### Getting the current application state

You can query the application at any time by calling

`GET https://api.hellobestow.com/v2/applications/:application_id`

which will return the same JSON object.




## Personal Section

> Endpoint URL

> **POST https://api.hellobestow.com/v2/applications/:application_id/personal_section**

> Example Request

```shell

curl -X POST \
  https://api.hellobestow.com/v2/applications/4asvn9482haSDf28a/:application_id/personal_section \
  -H 'Authorization: 82zZIHeBqUlBtICMX5li' \
  -H 'Content-Type: application/json' \
  -H 'Accept:application/json' \
  -H 'cache-control: no-cache' \
  -d '{
    "birth_date": "1980-01-01", 
    "gender": "male", 
    "height_feet": 6, 
    "height_inches": 0, 
    "state": "TX", 
    "weight": 180,
    "address_1": "1000 Main St.",
    "address_2": "Apt 555",
    "city": "Austin",
    "state": "TX",
    "postal_code": 78701
    "ssn": "555-55-5555"
}
'
```

> Example Response

```json
{
  "application_id": "4asvn9482haSDf28a",
  "state": "incomplete",
  "completed_sections": {
    "personal": true,
    "policy": false,
    "billing": false,
    "beneficiaries": false,
    "medical": false,
    "lifestyle": false,
    "followup": true
  }
}
```

`POST https://api.hellobestow.com/v2/applications/:application_id/personal_section`

In this section, you will use the `application_id` and submit personal information for the applicant, including name, birth date, gender, address, SSN, etc.  

NOTE:  It is recommended to collect the SSN (Social Security Number) of the applicant at the end of the application, because it is sensitive information, many people may abandon the application if asked too early in the flow.

### Query Paramters

Parameter | Required | Description
--------- | ------- | -----------
birth_date | true | Birth date in format YYYY-MM-DD
gender | true | "male" or "female"
height_feet | true | Feet part of height
height_inches | true | Inches part of height
weight | true | Weight in lbs
address_1 | true | address line 1
address_2 | false | address line 2
city | true | address city
state | true | The 2-character abbreviation of the US state
postal_code | true | The 5 digit postal code
ssn | true | The 9 digit social security number 

### Return Values

The call will receive a JSON object with the `application_id` and the status on the completed sections.  If a completed section is `false`, it will need to be completed before `state` will be go from `imcomplete` to `ready_to_submit`.








## Policy Section

> Endpoint URL

> **POST https://api.hellobestow.com/v2/applications/:application_id/policy_section**

> Example Request

```shell

curl -X POST \
  https://api.hellobestow.com/v2/applications/4asvn9482haSDf28a/:application_id/policy_section \
  -H 'Authorization: 82zZIHeBqUlBtICMX5li' \
  -H 'Content-Type: application/json' \
  -H 'Accept:application/json' \
  -H 'cache-control: no-cache' \
  -d '{
    "term": 2,
    "face_value": 50000
}
'
```

> Example Response

```json
{
  "application_id": "4asvn9482haSDf28a",
  "state": "incomplete",
  "completed_sections": {
    "personal": true,
    "policy": true,
    "billing": false,
    "beneficiaries": false,
    "medical": false,
    "lifestyle": false,
    "followup": true
  }
}
```

`POST https://api.hellobestow.com/v2/applications/:application_id/policy_section`

In this section, you will use the `application_id` and submit the policy information that the customer is interested in buying.  These include the face value of the policy (the amount paid on disbursement) and the term of the policy in years

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
face_value | true | The dollar amount of the life insurance policy
term | true | The length of the life insurance policy in years

### Return Values

The call will receive a JSON object with the `application_id` and the status on the completed sections.  If a completed section is `false`, it will need to be completed before `state` will be go from `imcomplete` to `ready_to_submit`.



















## Beneficiaries Section

Beneficiaries are the people eligible to receive the distributions of the life insurance policy.

`POST https://api.hellobestow.com/v2/applications/:application_id/beneficiaries_section`

> Endpoint URL

> **POST https://api.hellobestow.com/v2/applications/:application_id/beneficiaries_section**

> Example Request

```shell

curl -X POST \
  https://api.hellobestow.com/v2/applications/4asvn9482haSDf28a/beneficiaries \
  -H 'Authorization: 82zZIHeBqUlBtICMX5li' \
  -H 'Content-Type: application/json' \
  -H 'Accept:application/json' \
  -H 'cache-control: no-cache' \
  -d '[
    {
      "first_name": "Lois",
      "last_name": "Lane",
      "relationship": "spouse",
      "percentage": 50
    },
    {
      "first_name": "Martha",
      "last_name": "Kent",
      "relationship": "parent",
      "percentage": 50
    }
]
'
```

> Example Response

```json
{
  "application_id": "4asvn9482haSDf28a",
  "state": "incomplete",
  "completed_sections": {
    "personal": true,
    "policy": true,
    "billing": false,
    "beneficiaries": true,
    "medical": false,
    "lifestyle": false,
    "followup": true
  }
}
```

### Query Parameters

Pass in a JSON array of Beneficiary objects.  Each object should have the following properties.

Parameter | Required | Description
--------- | ------- | -----------
first_name | true | The first name of the beneficiary
last_name | true | The last name of the beneficiary
relationship | true | one of [spouse, domestic partner, child, parent, fiance, sibling, grandparent]
percentage | true | The percentage of the eligible disbursement


<aside class="notice">
All beneficiary percentages must sum to 100
</aside>

### Return Values

The call will receive a JSON object with the `application_id` and the status on the completed sections.  If a completed section is `false`, it will need to be completed before `state` will be go from `imcomplete` to `ready_to_submit`.










## Billing Section

> Endpoint URL

> **POST https://api.hellobestow.com/v2/applications/:application_id/billing_section**

> Example Request

```shell

curl -X POST \
  https://api.hellobestow.com/v2/applications/4asvn9482haSDf28a/:application_id/billing_section \
  -H 'Authorization: 82zZIHeBqUlBtICMX5li' \
  -H 'Content-Type: application/json' \
  -H 'Accept:application/json' \
  -H 'cache-control: no-cache' \
  -d '{
    "card_number": "424242424242",
    "card_expiration_date": "02/2020"
    "card_cvc": 123
}
'
```

> Example Response

```json
{
  "application_id": "4asvn9482haSDf28a",
  "state": "incomplete",
  "completed_sections": {
    "personal": true,
    "policy": true,
    "billing": true,
    "beneficiaries": true,
    "medical": false,
    "lifestyle": false,
    "followup": true
  }
}
```

`POST https://api.hellobestow.com/v2/applications/:application_id/billing_section`

In this section, you will use the `application_id` and submit the policy information that the customer is interested in buying.  These include the face value of the policy (the amount paid on disbursement) and the term of the policy in years

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
card_number | true | The 16-digit credit card number as a `string`
card_expiration_date | true | The expiration date of the card in format "MM/YYYY"
card_cvc | true | The 3 digit CVC on the card

### Return Values

The call will receive a JSON object with the `application_id` and the status on the completed sections.  If a completed section is `false`, it will need to be completed before `state` will be go from `imcomplete` to `ready_to_submit`.

















## Medical Section

In this section, the applicant will need to answer between 7 and 20 questions.  The required questions may change depending on the answers the applicant gives.  For example, if an applicant has diabetes, there will be more questions asked to clairfy the conditions around their diabetes.  

When starting the medical section, the API user will send a request for the first question, Bestow will reply with the question and possible answers, and then the API user will then ask the applicant, submit the answer and receive the next question.  Because of the dynamic nature of the questions, Bestow will give the API user one question at a time until we are done with the section.

The API user will use the following 2 routes:

`GET https://api.hellobestow.com/v2/applications/:application_id/medical_section/next_question`

`POST https://api.hellobestow.com/v2/applications/:application_id/medical_section/answer_question`


### Requesting the first question

> Endpoint URL

> **GET https://api.hellobestow.com/v2/applications/:application_id/medical_section/next_question**

> Example Request

```shell

curl -X GET \
  https://api.hellobestow.com/v2/applications/4asvn9482haSDf28a/medical_section/next_question \
  -H 'Authorization: 82zZIHeBqUlBtICMX5li' \
  -H 'Content-Type: application/json' \
  -H 'Accept:application/json' \
  -H 'cache-control: no-cache' 
'
```

> Example Response

```json
{
  "application_id": "4asvn9482haSDf28a",
  "next_question": {
    "question_id": "zxvzkjaIUHdg2ga42",
    "question_text": "In the past 3 months, have you unintentionally lost more than 20 pounds?",
    "answer_type": "single choice",
    "choices": ["Yes", "No"]
  }
}
```


Making an API call will retrieve the first question.

`GET https://api.hellobestow.com/v2/applications/:application_id/medical_section/next_question`


#### Query Parameters

None

#### Return Values

The call will receive a JSON object with `application_id`, as well as a `next_question` object will will have the `question_id` you will use in the next call, and the parameters of the question to ask the applicant.

Parameter  | Description
--------- | ------- | -----------
question_text | The text of the question you must ask the applicant
choices | An array of eligible answers that the applicant can select
answer_type | The type of answer expected.  Will either be ["single choice", "multiple choice", "text"]




-------------




### Answering A Question

The `application_id` and `question_id` will be passed into the API request, along with the applicant's answer, in order to answer a question.

`POST https://api.hellobestow.com/v2/applications/:application_id/medical_section/answer_question`

> ---------------------------

> Endpoint URL

> **POST https://api.hellobestow.com/v2/applications/:application_id/medical_section/answer_question**

> Example Request

```shell

curl -X POST \
  https://api.hellobestow.com/v2/applications/4asvn9482haSDf28a/medical_section/answer_question \
  -H 'Authorization: 82zZIHeBqUlBtICMX5li' \
  -H 'Content-Type: application/json' \
  -H 'Accept:application/json' \
  -H 'cache-control: no-cache' \
  -d '{
    "question_id": "zxvzkjaIUHdg2ga42",
    "answer": "No"
  }
'
```

> Example Response

```json
{
  "application_id": "4asvn9482haSDf28a",
  "answer_id": "nkasdu489ax082d",
  "next_question": {
    "question_id": "48398asdfjhkajshdfquegkj",
    "question_text": "In the past 12 months, have you used any tobacco or nicotine products (Excluding less than 24 cigars)",
    "answer_type": "single choice",
    "choices": ["Yes", "No"]
  }
}
```

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
question_id | true | The ID of the question given to the API user in `/next_question`
answer | true | The applicant's answer.  May either be a `string` or an `array` in the case that it is multiple choice

### Return values

The call will receive a JSON object with `application_id`, as well as a `next_question` object will will have the `question_id` you will use in the next call, and the parameters of the question to ask the applicant.


-------------

### When all questions are answered

When there are no futher questions, a call to `/answer_question` will return with `next_question` being null.

`
{
  "application_id": "4asvn9482haSDf28a",
  "answer_id": "f48sh084h2oiaddo8",
  "next_question": null
}
`

The `medical_section` will then be complete.




## Lifestyle Section

***This is identical to the medical section with a different set of questions***



## Submitting The Application

> Endpoint URL

> **POST https://api.hellobestow.com/v2/applications/:application_id/submit**

> Example Request

```shell

curl -X POST \
  https://api.hellobestow.com/v2/applications/:application_id/submit \
  -H 'Authorization: 82zZIHeBqUlBtICMX5li' \
  -H 'Content-Type: application/json' \
  -H 'Accept:application/json' \
  -H 'cache-control: no-cache' 
```

> Example Response

```json
{
  "application_id": "4asvn9482haSDf28a",
  "state": "submitted",
  "completed_sections": {
    "personal": true,
    "policy": true,
    "billing": true,
    "beneficiaries": true,
    "medical": true,
    "lifestyle": true,
    "followup": true
  }
}
```

When the application has all sections complete and `"state": "ready_to_submit"`, submit the application by calling the endpoint

`POST https://api.hellobestow.com/v2/applications/:application_id/submit` 


## Follow-up Section

TODO: After submitting the application, Bestow will run internal checks in order to determine eligibility for approval.  After this process, it may be necessary to ask follow-up questions to the applicant.


## Outstanding Questions (for internal discussion)

Will the API user need to submit a field for 

1. Accept Terms of Service
2. Agree to Bestow running 3rd party data checks against applicant
2. HIPAA authorization of applicant


