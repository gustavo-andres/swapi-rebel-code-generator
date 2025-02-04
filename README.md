# Star Wars API Secret Code Generator Challenge

## Overview
Build a serverless application that interacts with the Star Wars API (SWAPI) to generate a secret code using AWS Lambda functions orchestrated by AWS Step Functions and exposed via API Gateway. As you progress, record each intermediate result separately so that by the end you have all the pieces to know what your final Lambda output should be.

## Technical Stack
- AWS Lambda (Python)
- AWS Step Functions
- Amazon API Gateway
- Star Wars API (SWAPI) - [https://swapi.dev/api/](https://swapi.dev/api/)

## AWS Account Requirements
- You must use your personal AWS account for this challenge.
- All services used (Lambda, Step Functions, API Gateway) are covered under the AWS perpetual free tier.
- The expected usage for this challenge should result in zero costs.
- **Important:**  
  - Monitor your AWS Console for any unintended resource usage.
  - Clean up all resources after completion.
- We are not responsible for any AWS costs if resources are not properly monitored or cleaned up.

## Required Lambda Functions

### 1. getFilmData
- **Objective:**  
  - Query the SWAPI `/films/` endpoint.
  - Find and return the film with the earliest release date.
- **Output:**  
  - Return the complete film data for subsequent steps.
- **Documentation:**  
  - Record the full payload from SWAPI and note which film was selected.

### 2. processCharacter
- **Objective:**  
  - Accept the film data from the previous step.
  - For every character URL in that film, query the SWAPI `/people/` endpoint.
  - Calculate the average height (or another numeric aggregate) of all characters.
- **Output:**  
  - Return the original film data along with the calculated average height.
- **Documentation:**  
  - Save the responses from the character queries.
  - Record the individual height values and the computed aggregate.

### 3. selectPlanet
- **Objective:**  
  - Retrieve the complete list of planets from SWAPI by querying the `/planets/` endpoint.
  - Use the average height (from processCharacter) to select one planet:
    - Convert the average height to an integer.
    - Calculate:  
      `index = average_height % number_of_planets`
    - Select the planet at the calculated index.
- **Output:**  
  - Return the selected planet data along with the data from the previous step.
- **Documentation:**  
  - Document the payload from the planets endpoint.
  - Record your logic and the planet chosen.

### 4. generateCode
- **Objective:**  
  Generate a 3-character secret code using the following mapping:
  - **First character:** Map the film's `episode_id` to a letter (e.g., 1 = A, 2 = B, etc.).
  - **Second character:**  
    - Count the total number of residents on the selected planet (using the `residents` array).  
    - Perform modulo 10 on this count (`residents_count % 10`) to get a single digit.
  - **Third character:** Use the first letter of the planet name (uppercase).
- **Output Format:**  
  The final output must be in JSON format:
  ```json
  {
      "secret_code": "XYZ"
  }
