
## Beaconstac REST API documentation


### Steps to add Single-Sign-On(SSO) support for Beaconstac partners
- Make a POST request on https://beaconstac.mobstac.com/api/2.0/users/partner-sso/. Ensure you make this request after the user has finished the process of buying the beacons.
    - Add 'Authorization' header with value 'Token YOUR_TOKEN'. You can find the token in the 'Account' page on the Beaconstac dashboard.
    - JSON POST body with fields. All the fields are compulsory.
        - email
        - password (plaintext)
        - first_name
        - last_name
- Pass the response into the Beaconstac Partner SSO html iframe.
```html
    <iframe id="beaconstacDashboard" src="https://dashboard.beaconstac.com/partner-sso.html"></iframe>
```
```javascript
    var receiver = document.getElementById('beaconstacDashboard').contentWindow;
    receiver.postMessage(JSON.parse(userData), 'https://dashboard.beaconstac.com/partner-sso.html');
```
- The user is now logged in. Open https://dashboard.beaconstac.com to see the user specific dashboard.


Please check the repository for a sample implementation(partner_sso.html) of the whole SSO flow. Don't forget to change the TOKEN and USER_DATA values in the file before using.
