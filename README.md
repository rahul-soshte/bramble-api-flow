# Bramble API Flow

**1.**
**User clicks the Bramble Authorize Button**
---
He is directed to the URL by clicking on a Button

```

window.open('http://3.19.60.28:3000/'+btoa('CLIENT_ID:CLIENT_SECRET_ID'), '_self');

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

    /authorise?response_type=code&client_id=CLIENT_ID&redirect_uri=CALLBACK_URL&state=TESTSTATE&scope=PROFILE

* **Method:**

  `POST`
  
*  **URL Params**

   **Required:**
 
   `response_type='code'`
  
   `client_id=[String]`
    
   `redirect_uri=[String]`
    
   `state=[String]`
    
   `scope=[String]`

* **Header Params**
  **Required:**

  `Authorization='Bearer ' + wallet_token
  `

* **Data Params**

  None

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

* **Sample Call:**

  ```javascript
        var xhr = new XMLHttpRequest();
        authURL = "http://" + bramble.ip_address + "/authorise?response_type=code&client_id=CLIENT_ID&redirect_uri=CALLBACK_URL&state=teststate&scope=profile";
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

  /oauth/token

* **Method:**

  `POST`
  
*  **URL Params**

   **Required:**
 
   None

* **Header Params**
  
  **Required:**

  `Authorization='Basic ' + base64encode('CLIENT_ID:CLIENT_SECRET_ID)')
  `

* **Data Params**

     `grant_type='authorization_code'`

     `code=CODE_RECEIVED_IN_THE_AUTHORIZATION_REQUEST`
     
     `redirect_uri=CALLBACK_URL`

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** 
      `{
        "access_token": "199146e7e010ffa216301333b4c8cc14b9184958",
        "accessTokenExpiresAt": "2020-03-24T13:34:07.337Z",
        "scope": "profile"
       }`

* **Sample Call:**

  ```javascript
        var xhr = new XMLHttpRequest();
        authURL = "http://3.19.60.28/oauth/token";
        xhr.open('POST',authURL, true);
        xhr.setRequestHeader("Authorization","Basic "+ btoa("CLIENT_ID:CLIENT_SECRET_ID"));
        let formData = new FormData();
        formData.append("grant_type", "authorization_code");
        formData.append(code,"8b8c5ee88cf3f61043e7a5e372deae5374cfe91b");
        formData.append("redirect_uri", "http://localhost:3000/");
        xhr.onreadystatechange = function() {

        if (xhr.readyState == XMLHttpRequest.DONE) {
                alert(xhr.responseText);
            }
        }
        xhr.send(formData);
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

      `achievement_id=[hexadecimal]` 

* **Header Params**
  
  **Required:**

      `
      Authorization='Bearer ' + access_token(Received in Authorization Grant Request)
      `

* **Data Params**

  None

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** `Added Achievement Successfully`

* **Sample Call:**

  ```javascript
        var xhr = new XMLHttpRequest();
        var achievementURL = "http://3.19.60.28:3000/achievement/5e871be30b3d43640a15e01b";
        xhr.open('POST',achievementURL, true);
        xhr.setRequestHeader("Authorization","Bearer "+ "199146e7e010ffa216301333b4c8cc14b9184958");        
        xhr.send();

        xhr.onreadystatechange = function() {
        
        if (xhr.readyState == XMLHttpRequest.DONE) {
            // alert(xhr.responseText);
            var win = window.open(xhr.responseText, '_blank');
            if (win) {
            //Browser has allowed it to be opened
            win.focus();
            } else {
                //Browser has blocked it
                alert('Please allow popups for this website');
                }
            }
        }
  ```