## Configure AI Applications to Optimize Search Results: Challenge Lab

- **Task 6.11: You will create a Schema of type YAML. Paste the following into the Schema text box**
```
openapi: 3.0.0
info:
  title: Employee Feedback API
  version: 1.0.0
servers:
  - url: 'YOUR_CLOUD_RUN_FUNCRION_URL'
paths:
  /:
    post:
      summary: Record employee feedback
      operationId: recordEmployeeFeedback
      requestBody:
        description: Employee feedback and store number to record.
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/EmployeeFeedback'
      responses:
        '200':
          description: Success
          content:
            application/json:
              schema:
                type: object
                properties:
                  message:
                    type: string
                    example: New row has been added          
components:
  schemas:
    EmployeeFeedback:
      type: object
      required:
        - feedback
      properties:
        feedback:
          type: string
        store_number:
          type: integer
```
