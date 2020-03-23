# Bramble API Flow

**1.** The user clicks the Bramble Authorize Button (Front Channel) 
He is directed to the URL by clicking on a Button

```

window.open('http://3.19.60.28:3000/'+btoa('client_id:client_secret_id'), '_self');

```
Then he is directed to this page.
![Authorization Page](img/1.png)

User enters his credentials and he is redirected to your website
![Credentials](img/2.png)

He is redirected on the Callback URL you provided. Suppose for example the callback url is http://flappybird.com/wallet_token. As you have received the wallet token(or the access token) at the callback url, save the wallet token(or access token) in your server for the particular user.

**2.** Authorization request is sent (Back Channel)

URL
http://" + ip_address + "/authorise?response_type=code&client_id=client_id&redirect_uri=callback_url&state=teststate&scope=profile"

POST

Header
("Authorization","Bearer "+ value)

You will receive a code

**6.** 
form.AddField("grant_type","authorization_code");
form.AddField("code","CODE RECEIVED");
form.AddField ("redirect_uri", "CALLBACK_URL");

POST
http://"+ip_address+"/oauth/token

Header
  ("Authorization", "Basic base64value");
  base64 is from base64('client_id:client_secret_id')

You will receive a access token. Store it somewhere.

**7.** Redeem
    authURL = "http://ip_address/redeem/"+$("#earned").html();
    xhr.open('POST',authURL, true);
    xhr.setRequestHeader("Authorization","Bearer "+ Cookies.get('access_token'));

<!-- # BRAMBLE API FLOW DOCUMENTATION

**Open Authorization Page**
----
  Opens Bramble Authorization Page

* **URL**

  /users/:id

* **Method:**

  `GET`
  
*  **URL Params**

   **Required:**
 
   `id=[integer]`

* **Data Params**

  None

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** `{ id : 12, name : "Michael Bloom" }`
 
* **Error Response:**

  * **Code:** 404 NOT FOUND <br />
    **Content:** `{ error : "User doesn't exist" }`

  OR

  * **Code:** 401 UNAUTHORIZED <br />
    **Content:** `{ error : "You are unauthorized to make this request." }`

* **Sample Call:**

  ```javascript
    $.ajax({
      url: "/users/1",
      dataType: "json",
      type : "GET",
      success : function(r) {
        console.log(r);
      }
    });
  ``` -->
