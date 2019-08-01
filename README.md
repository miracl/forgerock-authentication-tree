# MIRACL Trust Node for Authentication Trees

[MIRACL Trust® ID](https://miracl.com) is a 100% software solution providing true two-factor authentication using the latest [Zero Knowledge Proof (ZKP)](https://www.miracl.com/zero-knowledge) technology - no personal data is stored or transmitted. This eliminates the need for outdated security practices such as passwords, SMS Texts, push notifications and key-cards.

Usability is paramount, no additional user enrolment steps are required and users authenticate in one step. Without the requirement for hardware, mobiles or fiddly second steps, your users will love the simplicity and you will appreciate the reduction in support and maintenance overheads.

Provisioned as a PAYG service at a fraction of the cost of hardware solutions or SMS texts, MIRACL Trust® ID can be integrated into any OIDC compliant platform such as ForgeRock. Capable of scaling to millions of users overnight, a combination of usability, security and cost make it possible to deploy strong MFA to large B2C networks where it was previously impossible or impractical.
MIRACL Trust® ID makes a perfect addition to any large-scale ForgeRock installation.

[![Play Video](images/video.png)](https://www.youtube.com/watch?v=h-ccPbfWI7A)

Please visit [https://miracl.com](https://miracl.com) or email forgerock@miracl.com for details.

Create a MIRACL Trust® account and get 1,000 uses/month free forever - [Get Started](https://trust.miracl.cloud/get-started)


## Integration manual

The steps of reference integration constructing are listed below.

* Deploy Access Manager 6 as described in the [ForgeRock manual](https://backstage.forgerock.com/docs/am/6/quick-start-guide).
* Create OIDC interface at [MIRACL Trust Management Portal](https://trust.miracl.cloud) as described in the [MIRACL Trust manual](https://devdocs.trust.miracl.cloud/register-create-new-app/). Remember its application client ID and secret.
* Go to AM realm dashboard. Open "Authentication - Trees".
* Click "(+) Create Tree".

* Fill the form.
    + Tree Name: `"MIRACL"`

* Click "Create".

The GUI tree builder will appear. Initially it contains two connected nodes - `"Start"` and `"Failure"`.

* In the "Components" section at the left, find "OpenID Connect" one and drag it to the working area. Do the same for "Provision Dynamic Account" and "Success" components. You should have the following components at the working area:
    + "Start"
    + "OpenID Connect"
    + "Provision Dynamic Account"
    + "Success"


* Click the "OpenID Connect" component. Properties list will appear at the right.

* Fill the form.
    + Node Name: `"MIRACL"`
    + Client Id: `The OIDC application client ID from Management Portal`
    + Client Secret: `The OIDC application client secret from Management Portal`
    + Authentication Endpoint URL: `"https://api.mpin.io/authorize"`
    + Access Token Endpoint URL: `"https://api.mpin.io/oidc/token"`
    + User Profile Service URL: `"https://api.mpin.io/oidc/userinfo"`
    + OAuth Scope: `"openid"`
    + Redirect URL: `The URL where AM is deployed`
    + Social Provider: `"MIRACL"`
    + Auth ID Key: `"id"`
    + Use Basic Auth: `Off`
    + Account Provider: `"org.forgerock.openam.authentication.modules.common.mapping.DefaultAccountProvider"`
    + Account Mapper: `"org.forgerock.openam.authentication.modules.common.mapping.JsonAttributeMapper"`
    + Attribute Mapper: `"org.forgerock.openam.authentication.modules.common.mapping.JsonAttributeMapper"`
    + Add the following entry to Account Mapper Configuration. You have to click "Add" button first for subform to popup and click "+" button after finished:
        + `"sub"`: `"uid"`
    + Add the following entries to Attribute Mapper Configuration. You have to click "Add" button first for subform to popup and click "+" button after finished:
        + `"sub"`: `"uid"`
        + `"email"`: `"mail"`
    + Save Attributes in the Session: `Off`
    + OpenID Connect Mix-Up Mitigation Enabled: `Off`
    + Token Issuer: `https://api.mpin.io`
    + OpenID Connect Validation Type: `JWK URL`
    + OpenID Connect Validation Value: `https://api.mpin.io/oidc/certs`

* Now nodes should be connected in the given way. Node input is located at the left, and output(s) at the right. Connection is performed by dragging some output to a required input.
    + Connect "Start" node output to "MIRACL" node input.
    + Connect "MIRACL" node output saying "No account exists" to "Provision Dynamic Account" node input.
    + Connect "Provision Dynamic Account" node output to "Success" node input.

    ![ScreenShot](images/forgerock_authentication_tree.png)

* Click "Save".


## Test the integration

* Go to AM realm dashboard. Open "Authentication - Settings".
* Go to "Core" tab.
    + Select Organization Authentication Configuration: `"MIRACL"`
* Click "Save Changes".

* Once you change that, the default log in to AM will be `"MIRACL"` tree. If you want to go back to the administrator window to make changes to the configuration go to AM URL by appending `"/console"` (for example, `"http://openam.partner.com:8080/openam/console"`). This will use the administrator service to log in to AM (which should be the `"ldapService"`).

* Logout from Access Manager and try to login again.
* AM should automatically redirect you to the MIRACL identity provider.


# Disclaimer

The sample integration setup described herein is provided on an "as is" basis, without warranty of any kind, to the fullest extent permitted by law. MIRACL does not warrant or guarantee the individual success developers may have when following the integration manual on their development platforms or in production configurations.
MIRACL does not warrant, guarantee or make any representations regarding the use, results of use, accuracy, timeliness or completeness of any data or information relating to the integration manual. MIRACL disclaims all warranties, expressed or implied, and in particular, disclaims all warranties of merchantability, and warranties related to the integration manual, or any service or software related thereto.
MIRACL shall not be liable for any direct, indirect or consequential damages or costs of any type arising out of any action taken by you or others related to the integration manual.
