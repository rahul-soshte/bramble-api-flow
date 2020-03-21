<!-- # Bramble API Flow
**1.** First the user click the Authorize Button 
He is directed to the URL by the following code in Javascripts

```
var win = window.open('http://3.19.60.28:3000/'+btoa('mansim:armygrid'), '_blank');

```

**2.** Then he is directed to this page.
![Authorization Page](img/1.png)

**3.** Enter his credentials and he is redirected to your website
![Credentials](img/2.png)

**4.** He is redirected on the Callback URL you provided. Suppose for example the redirect uri is http://armygrid.com/wallet_token . You can receive the wallet token the callback url. After you have saved wallet_token from the callback url. You need to send more information about 

-->

# BRAMBLE API FLOW DOCUMENTATION

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
  ```
