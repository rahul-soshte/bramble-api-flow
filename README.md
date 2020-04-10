# Bramble API Flow
**1.**
**User clicks the Bramble Authorize Button**
----
  The user puts in his username and password and then is redirected back to the game. 

* **URL**

    /bramble?response_type=code&client_id=client_id&redirect_uri=callback_url&state=teststate&scope=profile

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
     `HTML Content for the user to put in his username and password`

* **Error Response:**

  * **Code:** 400 <br />
    **Content:** `{
    "statusCode": 400,
    "status": 400,
    "code": 400,
    "message": "Invalid client: redirect_uri does not match client value",
    "name": "invalid_client"
   }`


* **Sample Call:**

  ```javascript
        window.open('http://3.19.60.28:3000/bramble?response_type=code&client_id=mansim&redirect_uri=http://armygrid.com/callback/&state=teststate&scope=profile', '_blank');
  ```

**2.**
**Authorization Grant Request**
----
  Extracting the 'code' in the Success Response of the previous, the final access Token which will help to send / receive data using this request

* **URL**

  /grant

* **Method:**

  `POST`

* **Header Params**
  
  **Required:**

  `Authorization='Basic ' + btoa('client_id:client_secret_id)'`

* **Body Params(raw JSON)**
    `{
     "grant_type":"authorization_code",
     "code":"code_recieved_in_the_previous request",
     "redirect_uri":"armygrid.com/bramble_callback"
    }`

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
      `{
        "access_token": "199146e7e010ffa216301333b4c8cc14b9184958",
        "accessTokenExpiresAt": "2020-03-24T13:34:07.337Z",
        "scope": "profile"
       }`

* **Error Response:**

  * **Code:** 401 <br />
    **Content:** `{
    "statusCode": 401,
    "status": 401,
    "code": 401,
    "message": "Invalid token: access token has expired",
    "name": "invalid_token"
    }`

* **Sample Call:**

  ```javascript
        var xhr = new XMLHttpRequest();
        authURL = "http://3.19.60.28/grant";
        xhr.open('POST',authURL, true);
        xhr.setRequestHeader("Authorization","Basic "+ btoa("mansim:armygrid"));
        xhr.setRequestHeader("Content-Type", "application/json"); 
        var data = JSON.stringify({ "grant_type": "authorization_code", "code": "335c162ae937c0ece349fdea6d57ec0e8160100d", "redirect_uri":"armygrid.com/bramble_callback" });

        xhr.onreadystatechange = function() {

        if (xhr.readyState == XMLHttpRequest.DONE) {
                alert(xhr.responseText);
            }
        }
        xhr.send(data);
  ```
  
**3.** 
**Simple Achievement Data POST Request**
----
Request which will send Achievements data of the user to Bramble API. So if a user has completed a particular the game can send data to Bramble API with this request. 

* **URL**

    /achievement/:achievement_id

* **Method:**

  `POST`
  
*  **URL Params**

   **Required:**

      `achievement_id=[hexadecimal] (example: '5e872a6ac3d6ae53213befcb')`  

* **Header Params**

      `Authorization='Bearer ' + access_token(Received in Authorization Grant Request)`

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** `Added Achievement Successfully`

* **Error Response:**

  * **Code:** 401 <br />
    **Content:** `{
    "statusCode": 401,
    "status": 401,
    "code": 401,
    "message": "Invalid token: access token has expired",
    "name": "invalid_token"
    }`

* **Sample Call:**

  ```javascript
        var xhr = new XMLHttpRequest();
        var achievementURL = "http://3.19.60.28:3000/achievement/5e872a6ac3d6ae53213befcb";
        xhr.open('POST',achievementURL, true);
        xhr.setRequestHeader("Authorization","Bearer "+ "199146e7e010ffa216301333b4c8cc14b9184958");
        xhr.onreadystatechange = function() {

        if (xhr.readyState == XMLHttpRequest.DONE) {
                alert(xhr.responseText);
            }
        }

        xhr.send();
  ```