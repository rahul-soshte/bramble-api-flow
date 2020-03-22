 # Bramble API Flow
**1.** First the user click the Authorize Button 
He is directed to the URL by the following code in Javascripts

```
var win = window.open('http://ip_address/'+btoa('client_id:client_secret_id'), '_blank');

```

**2.** Then he is directed to this page.
![Authorization Page](img/1.png)

**3.** Enter his credentials and he is redirected to your website
![Credentials](img/2.png)

**4.** He is redirected on the Callback URL you provided. Suppose for example the redirect uri is http://armygrid.com/wallet_token . You can receive the wallet token at the callback url. After you have saved wallet_token from the callback url.

**5.** After that a authorization request is sent.
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
