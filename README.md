# Bramble API Flow

**1.**
**User clicks the Bramble Authorize Button**
---
He is directed to the URL by clicking on a Button

```

window.open('http://3.19.60.28:3000/'+btoa('client_id:client_secret_id'), '_self');

```
Then he is directed to this page.

![Authorization Page](img/1.png) 

User enters his credentials and he is redirected to your website

![Credentials](img/2.png) 

He is redirected on the Callback URL of the game. Suppose for example the callback url returned is 

``` 

https://flappybird.com/wallet_token

```
As you have received the **wallet token** at the callback url, save the **wallet token** in your server for the particular user.

**2.**
**Authorization Request**
----
  The Game Server receieves a Authorization code from this request.

* **URL**

    /authorise?response_type=code&client_id=client_id&redirect_uri=callback_url&state=teststate&scope=profile

* **Method:**

  `POST`
  
*  **URL Query Params**

   **Required:**
 
   `response_type='code' (example: 'code')`
  
   `client_id=[String] (example: 'mansim')`
    
   `redirect_uri=[String] (example: 'http://armygrid.com/callback/')`
    
   `state=[String] (example: 'teststate')`
    
   `scope=[String] (example: 'profile')`

* **Header Params**
  **Required:**

  `Authorization='Bearer ' + wallet_token`

* **Success Response:**

  * **Code:** 200 <br />
    **Content:**
     `{
      "authorizationCode": "8b8c5ee88cf3f61043e7a5e372deae5374cfe91b",
      "expiresAt": "2020-03-23T14:14:52.796Z",
      "redirectUri": "http://localhost:3000/",
      "scope": "profile",
      "code": "8b8c5ee88cf3f61043e7a5e372deae5374cfe91b"
    }`

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
        var xhr = new XMLHttpRequest();
        authURL = "http://3.19.60.28:3000/authorise?response_type=code&client_id=mansim&redirect_uri=http://armygrid.com/callback/&state=teststate&scope=profile";
        xhr.open('POST',authURL, true);
        xhr.setRequestHeader("Authorization","Bearer "+ wallet_token );
        xhr.onreadystatechange = function() {

        if (xhr.readyState == XMLHttpRequest.DONE) {
                alert(xhr.responseText);
            }
        }
        xhr.send();
  ```

**3.**
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

**4.** 
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