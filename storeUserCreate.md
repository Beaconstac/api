## Partner API: Create a user account in the Beaconstac store

1. Make a POST request on https://storefrontapi.beaconstac.com/v1/users/create/. It is mandatory for you to make this request before you navigate the user to buy beacons on our store.
    - Add 'Authorization' header with value 'BASIC YOUR_TOKEN'. Please use the token emailed by us.
    - JSON POST body with fields. All the fields are compulsory.
        - email
        - password (plaintext)
        - firstName
        - lastName
2. Insert the script below in your webpage.
```html
    <script src="https://static.beaconstac.com/assets/js/partner-sso.js"></script>
```
3. Once the script is added. Call the following method to login the user into Beaconstac store.
  The method expects one parameter i.e. the response object returned by the POST request in step 1.
```javascript
    signInUserToBeaconstacStore(USER_JSON_OBJECT);
```
4. The user is now logged in to store and the plan (as specified in the partner agreement) will automatically be added to the cart. Open https://www.beaconstac.com/buy-beacons/ to take user to buy beacons from Beaconstac store.


Please [partner_sso.html](https://github.com/Beaconstac/api/blob/master/partner_sso.html) for a sample implementation . Don't forget to change the TOKEN and USER_DATA values in the file.
