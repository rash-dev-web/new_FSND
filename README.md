Base URL: https://render-deployment-example-y6pb.onrender.com/

Motivation for the project: This project marks the end journey for Udacity FSND program where I can show the amazing learning experience gained throughout this course.

Installing Dependencies: pip -r install requirements.txt

1. Before Executing Postman file
    a. Update the respective token(all three tokens - 
    Casting Assistant - token_cast_asst, 
    Casting Director - token_cast_direct, 
    Executive Producer - token_exec_pro) by hitting below url:
    https://fsnd721.us.auth0.com/authorize?audience=capstone&response_type=token&client_id=gR6MqPCGbbwwOGHnyxnHfsIj2wE2LO86&redirect_uri=http://localhost:8080/login-results
    b. Update the existing movie and actor id before hitting patch and delete calls.

1. All three roles can perform below task successfully:

a. Casting Assistant
I. GET /actors
Sample Request: 
curl --location 'https://render-deployment-example-y6pb.onrender.com/actors' \
--header 'Authorization: <token_cast_asst>'

Sample Response: 
{
    "actors": [
        {
            "age": 55,
            "gender": "Female",
            "id": 1,
            "name": "Richard"
        }
    ],
    "success": true
}

II. GET /movies
Sample Request: 
curl --location 'https://render-deployment-example-y6pb.onrender.com/movies' \
--header 'Authorization: <token_cast_asst>'

2. Casting Director

a. The same role as Casting Assistant for GET /actors, GET /movies and including below

I. POST /actors

Sample Request:
curl --location 'https://render-deployment-example-y6pb.onrender.com/actors' \
--header 'Authorization: Bearer <token_cast_direct>' \
--data '{
    
    "name": "Richard",
    "age": 55,
    "gender": "Male"
}'

Sample Response: 
{
    "actors": [
        {
            "age": 55,
            "gender": "Male",
            "id": 6,
            "name": "Richard"
        }
    ],
    "success": true
}

II. DELETE /actors

Sample Request: 
curl --location --request DELETE 'https://render-deployment-example-y6pb.onrender.com/actors/4' \
--header 'Authorization: Bearer <token_cast_direct>'

Sample Response:
{
    "id": 4,
    "success": true
}

3. Executive Producer
a. The same role as Casting Director for GET /actors, GET /movies and including below

I. POST /movies

Sample Request: 
curl --location 'https://render-deployment-example-y6pb.onrender.com/movies' \
--header 'Authorization: Bearer <token_exec_pro>' \
--header 'Content-Type: application/json' \
--data '{
    "title": "Sample",
    "release_date": "2025-01-01 10:10:15"
}'

Sample Response: 
{
    "movie": [
        {
            "id": 5,
            "release_date": "Wed, 01 Jan 2025 10:10:15 GMT",
            "title": "Sample"
        }
    ],
    "success": true
}

II. DELETE /movies

Sample Request: 
curl --location --request DELETE 'https://render-deployment-example-y6pb.onrender.com/movies/5' \
--header 'Authorization: Bearer <token_exec_pro>'

Sample Response:
{
    "id": 5,
    "success": true
}


4. RBAC controls

*** Casting Assistant ***
Casting Assistance cannot perform any other tasks other than GET calls, else below response should be displayed:
{
    "error": 403,
    "message": {
        "code": "unauthorized",
        "description": "Permission not found."
    },
    "success": false
}


*** Casting Director ***
Casting Director cannot add or delete a movie, else below error response should be displayed:
{
    "error": 403,
    "message": {
        "code": "unauthorized",
        "description": "Permission not found."
    },
    "success": false
}

*** Expired token ***
In case of expired token, below response will be displayed to all users:
{
    "error": {
        "code": "token_expired",
        "description": "Token expired."
    },
    "status_code": 401
}