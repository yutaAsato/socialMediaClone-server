# REST API socialMediaApp-server
REST API server for the socialMediaApp-client.

**EXAMPLE - getUser**
----
  Returns json data about a single user.

* **URL**

  /user

* **Method:**

  `GET`
  
*  **URL Params**

   **Required:**
 

* **Data Params**

  None

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** `{ notifications:[,..], relationships: [,..], user:{username: sagat, location: thailand, bio: ...} }`
 
* **Error Response:**

  * **Code:** 404 NOT FOUND <br />
    **Content:** `{ error : "User doesn't exist" }`

  OR

  * **Code:** 401 UNAUTHORIZED <br />
    **Content:** `{ error : "Wrong Credentials" }`

* **Sample Call:**

  ```javascript
   async function client(endpoint,{ data, token, headers: customHeaders, ...customConfig } = {}) {
   const config = {
    method: data ? "POST" : "GET",
    body: data ? JSON.stringify(data) : undefined,
    headers: {
      Authorization: token ? token : undefined,
      "Content-Type": data ? "application/json" : undefined,
      ...customHeaders,
    },
    ...customConfig,
  };
  return window
    .fetch(`${apiURL}/${endpoint}`, config)
    .then(async (response) => {
      if (response.status === 401) {
        queryCache.clear();
        window.location.assign(window.location);
        return Promise.reject({ message: "Please re-authenticate." });
      }
      const data = await response.json();
      if (response.ok) {
        return data;
      } else {
        return Promise.reject(data);
      }
    });
