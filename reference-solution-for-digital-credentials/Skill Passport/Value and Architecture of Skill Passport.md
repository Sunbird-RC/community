# What is Skill Passport? Why is it required?

**The Skill Passport contains capabilities of capturing user competencies and 360 degree feedback with an option to integrate with Course Marketplace for skilling and 3rd party assessments for testing progress.**

The skill passport is meant to be a repository of the competencies of a user. It would contain the user’s job roles with requirements, the competency levels achieved till date, along with Self-assessment, End of course(CBP) assessments and Proctored Independent and Authorized Assessment (PIAA) scores and certificates. It would be accessed by employees through a mobile app (developed as part of Compass DPG: https://github.com/compass-core-platform).


The skill passport will take the data related to required competencies, self-assessment, CBP and PIAA scores, from FRAC, PIAA and marketplace modules and will be accessed by the employee on the mobile app. Along with this, WPCAS data will also be displayed which will be taken from the WPCAS module.


The major driver of developing such a module is to enable the users to have a one-stop shop to view and share all their competencies from the same place. This would be useful for users to understand gaps and improve their skills to become more competent professionals. They will also use this feature to share their performance to other necessary stakeholders, like workplaces. The workplaces would be able to verify the authenticity of such documents readily as these documents will have the relevant authority credentials. 

<div>

<img width="314" alt="Screenshot 2024-04-12 at 6 17 55 PM" src="https://github.com/rohitsamagra/community/assets/145356240/59c7d2ac-cd41-499e-a145-a70d61454f4a">

<img width="311" alt="Screenshot 2024-04-12 at 6 18 14 PM" src="https://github.com/rohitsamagra/community/assets/145356240/a44415c4-bc97-4dc2-bbb7-2e43b4fbd3c8">

<img width="313" alt="Screenshot 2024-04-12 at 6 19 04 PM" src="https://github.com/rohitsamagra/community/assets/145356240/749eeded-c033-4aea-bd20-9adc33f338b1">

* *Competency Passbook will be changed to Skill Passport in the above images

</div>

# Sample Use Case: Competency Passbook within Compass DPG

To deliver on the goals of providing public goods and services that improve the lives of India’s 1.4 billion citizens, one of the key dimensions in which the Indian state should improve is the management of its people or human resources. As a direct result of this, Bill & Melinda Gates Foundation (BMGF) wants to track competencies of human resources as a construct for understanding whether an individual has the attitudes, skills, and knowledge required to perform their role. Competencies can help a department answer the question: given our departmental goals, what attitudes, skills and knowledge should our employees possess to enable them to achieve their goals, and in the process, achieve the department’s goals? 

COMPASS is envisioned as a component of a goal-oriented human resource management system (GO-HRM) that will allow departments to link their goals with well-defined targets for teams and individuals, map competencies required to fulfill these targets, and link capacity to performance management.

COMPASS comprises multiple modules meant to deliver specific functionality. Skill Passport acts as passbook with repository of users competencies.

# Architecture Diagram

<div data-full-width="true">

![image](https://github.com/rohitsamagra/community/assets/145356240/4a256b10-b25b-4ec9-af2b-51b7f53868d8)

</div>

Details of other modules can be found here: https://github.com/compass-core-platform

# Installation Guide
Pre-Requisite:

1. git clone https://github.com/COMPASS-DPG/passbook.git
2. create a url for mongodb and change the mongodb url in .env.example (see the example of .env.example)
3. Deploy sunbirdRc - (https://github.com/SamagraX-RCW) and change the endpoint url of crednetial in .env.example

Steps To Install

1. Install docker in your system.

2. then Run the following commands
   ```bash
   docker build -t passbook .
   docker run -p 3000:3000 -d passbook
   ```
3. You can access the passbook on your system at
   ```
   http://localhost:3000/
   ```

# Passbook API Documentation

## Add User Assessment

Add an assessment for a user. This endpoint allows you to record assessment data for a specific user in the Passbook system.

### Endpoint

```
POST {{server}}/api/user/assessment
```

### Request

### Request Body

```
userId (string, required): The unique identifier of the user for whom you want to add an assessment.

competencyId (integer, required): The identifier for the competency being assessed.

competency (string, required): The name or description of the competency being assessed.

levelNumber (integer, required): The level of competency being assessed.

type (string, required):(PIAA/CBP/SELF) The type of assessment (e.g., "CBP" for Continuous Performance).

score (string, required): The score achieved by the user in the assessment.

certificateId (string, required): The identifier of the certificate associated with the assessment.

dateOfIssuance (string, required): The date on which the assessment was issued (in YYYY-MM-DD format).

```

### Example Request Body

json

```json lines
{
  "userId": "1246",
  "competencyId": 1,
  "competency": "NestJs",
  "levelNumber": 1,
  "type": "CBP",
  "score": "80",
  "certificateId": "did:ulp:711777e4-5123-41ea-a1cb-9edeb6c38282",
  "dateOfIssuance": "2021-12-01"
}
```

### Response

Success Response (HTTP 201 Created)

If the assessment is added successfully, the server will respond with an HTTP status code of 201, indicating that the assessment has been created.

### Error Responses

HTTP 400 Bad Request: If the request is invalid, the server will respond with a 400 status code and an error message specifying the issue.

HTTP 404 Not Found: If the user is not found in the system, the server will respond with a 404 status code.

HTTP 500 Internal Server Error: If there is an issue on the server side, it will respond with a 500 status code.

Notes
Make sure you have the necessary permissions and authentication to use this endpoint.
The response will contain the unique identifier of the created assessment for future reference.

# Add User Feedback Score

Add a feedback score for a user. This endpoint allows you to record feedback scores for a specific user in the Passbook system.

## Endpoint

POST {{server}}/api/user/feedback

## Request

### Request Body

```text

dateOfSurveyScore (string, required): The date on which the feedback survey score is recorded (in YYYY-MM-DD format).

certificateId (string, required): The unique identifier of the certificate associated with the feedback.

overallScore (integer, required): The overall feedback score given to the user.

competencies (array, required): An array of objects representing individual competencies and their respective feedback scores.

id (integer, required): The identifier for the competency being assessed.

name (string, required): The name or description of the competency being assessed.

levels (array, required): An array of objects representing feedback scores for different competency levels.

levelNumber (integer, required): The level number of the competency being assessed.

name (string, required): The name of the competency level.

score (string, required): The feedback score achieved for the competency level.
```

Example Request Body

```json5
{
  dateOfSurveyScore: '2023-10-23',
  certificateId: 'cert-001',
  overallScore: 85,
  competencies: [
    {
      id: 1,
      name: 'NestJs',
      levels: [
        {
          levelNumber: 1,
          name: 'Basic',
          score: '90%',
        },
        {
          levelNumber: 2,
          name: 'Intermediate',
          score: '80%',
        },
        {
          levelNumber: 3,
          name: 'Advanced',
          score: '90%',
        },
      ],
    },
    {
      id: 3,
      name: 'DB Modelling',
      levels: [
        {
          levelNumber: 1,
          name: 'Basic',
          score: '90%',
        },
        {
          levelNumber: 2,
          name: 'Intermediate',
          score: '80%',
        },
        {
          levelNumber: 3,
          name: 'Advanced',
          score: '80%',
        },
      ],
    },
    {
      id: 2,
      name: 'Micro Architecture',
      levels: [
        {
          levelNumber: 1,
          name: 'Basic',
          score: '50%',
        },
        {
          levelNumber: 2,
          name: 'Intermediate',
          score: '40%',
        },
      ],
    },
  ],
  userId: '1245',
}
```

## Response

Success Response (HTTP 201 Created)

If the feedback score is added successfully, the server will respond with an HTTP status code of 201, indicating that the feedback score has been created.

### Error Responses

HTTP 400 Bad Request: If the request is invalid, the server will respond with a 400 status code and an error message specifying the issue.

HTTP 404 Not Found: If the user is not found in the system, the server will respond with a 404 status code.

HTTP 500 Internal Server Error: If there is an issue on the server side, it will respond with a 500 status code.

### Notes

Make sure you have the necessary permissions and authentication to use this endpoint.

# Creating Credential In Sunbird Rc

For generating the Credential in Sunbird Rc
These are the following step

1. Create an identity for organisation/author/person
2. Using the above created identity create a Schema mentioning the fields as properties
3. Create a template using the html and giving variable from the schema
4. Finally after following the above steps, we can finally issue a certificate
5. Then given the credential Id, we can verify the credential or print the credential in template

Postman collection for Main flow for all the above step
https://api.postman.com/collections/17248210-f70fc9eb-a81d-47ee-9643-bd846869c30c?access_key=PMAT-01H59MVS7DXPSWSS2VKCCV7FR5
Adding credential information for Passbook

Once the credential has been issued to a person
It can be pushed to passbook with the certificated ID
With calling the POST api

`POST {{server}}/api/user/assessment`

```json5
{
  userId: '1246',
  competencyId: 1,
  competency: 'NestJs',
  levelNumber: 1,
  type: 'CBP',
  score: '80',
  certificateId: 'did:ulp:711777e4-5123-41ea-a1cb-9edeb6c38282',
  dateOfIssuance: '2021-12-01',
}
```

For more info about Sunbird RC, please go through the documention of it
https://docs.sunbirdrc.dev/learn/readme
