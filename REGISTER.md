**1.**
**Bramble Registration Page with Redirect**
----
  Opens up the Bramble Register Page. The user registers in his username and password and then is redirected back to the game. 

* **URL**

    /register?response_type=code&client_id=client_id&redirect_uri=callback_url&state=teststate&scope=profile

* **Method:**

  `GET`
  
*  **URL Query Params**

   **Required:**
 
   `response_type='code' (example: 'code')`
  
   `client_id=[String] (example: 'mansim')`
    
   `redirect_uri=[String] (example: 'http://armygrid.com/callback/')`
    
   `state=[String] (example: 'teststate')`
    
   `scope=[String] (example: 'profile')`

* **Success Response:**

  * **Code:** 200 <br />
    **Content:**
     `Opens the Bramble Register Page for the user`

* **Error Response:**

  * **Code:** 400 <br />
    **Content:** <br />
    `{
    "statusCode": 400,
    "status": 400,
    "code": 400,
    "message": "Invalid client: redirect_uri does not match client value",
    "name": "invalid_client"
   }`

  * **Code:** 400 <br />
    **Content:** <br />
    `{
    "statusCode": 400,
    "status": 400,
    "code": 400,
    "message": "Invalid client: missing client grants",
    "name": "invalid_client"
    }`

  * **Code:** 400 <br />
    **Content:** <br />
    `{
    "statusCode": 400,
    "status": 400,
    "code": 400,
    "message": "Unsupported response type: response_type is not supported",
    "name": "unsupported_response_type"
    }`


* **Sample Call:**

  ```javascript
  window.open('https://brambleapi.herokuapp.com/register?response_type=code&client_id=mansim&redirect_uri=http://armygrid.com/callback/&state=teststate&scope=profile', '_self');
  ```
* **Notes:**

  The user will be redirected after the registration step to the callback url of the game. The authorization code will be attached to the callback url ( Example: http://armygrid.com/callback/authorization_code ). Extract the authorization_code value and use it in the next request.
