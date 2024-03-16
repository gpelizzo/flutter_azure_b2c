# 1 Install flutter_azure_b2c package
### Official package is available at https://pub.dev/packages/flutter_azure_b2c but an issue does not allow to use it with iOS.
### #alex-dokienko provides a fork supporting TFP for iOS: https://github.com/alex-dokienko/flutter_azure_b2c
### this package is a simple fork of #alex-dokienko one, updating only the intl package version into pubspec.yaml (avoiding  compiling issue with my one app)    

# 2 Azure B2C general settings
## 2.1 Create an Azure Active Directory B2C tenant
https://learn.microsoft.com/en-us/azure/active-directory-b2c/tutorial-create-tenant

## 2.2 Create a new B2C App
### Open Azure AD B2C
<img src="azure_b2c_app_registration_1.png" alt="drawing" width="800"/>

### Go to App Registration (consider <AZURE_B2C_DIRECTORY_NAME>)
<img src="azure_b2c_app_registration_2.png" alt="drawing" width="800"/>

### and click on New registration
<img src="azure_b2c_app_registration_3.png" alt="drawing" width="800"/>

### enter application name and Register
<img src="azure_b2c_app_registration_4.png" alt="drawing" width="800"/>

### and copy Application (client) ID (Consider <AZURE_B2C_APP_CLIENT_ID>>)
<img src="azure_b2c_app_registration_5.png" alt="drawing" width="800"/>

## 2.3 User flow settings
### Go to Certificats & Secrets and + New client secret
<img src="azure_b2c_app_registration_6.png" alt="drawing" width="800"/>

### enter description and set Expires to 730 days (24 months), then click Add
<img src="azure_b2c_app_registration_7.png" alt="drawing" width="800"/>

### copy the client secret value (<CLIENT_SECRET_VALUE>)
<img src="azure_b2c_app_registration_8.png" alt="drawing" width="800"/>

### Go back to Azure AD B2C and click on Identity providers. Select Microsoft Account.
<img src="azure_b2c_app_registration_9.png" alt="drawing" width="800"/>

### Fill a name into Name, copy the <AZURE_B2C_APP_CLIENT_ID> into Client ID and <CLIENT_SECRET_VALUE> into Client Secret. (consider <MICROSOFT_IDP_CALLBACK_URL>), then click Save
<img src="azure_b2c_app_registration_10.png" alt="drawing" width="800"/>

### Click on User flow and + New user flow
<img src="azure_b2c_app_registration_11.png" alt="drawing" width="800"/>

### Select Sign up and Sign IN  as user flow type and the Recommander version, then click Create
<img src="azure_b2c_app_registration_12.png" alt="drawing" width="800"/>

### Fill a name <AZURE_B2C_USER_FLOW_WEB_ANDROID_NAME>, set Local accounts to None, select Microsoft Social identity provider create above, set Type of method to Email, set MFA enforcement to Off, then click Create  
<img src="azure_b2c_app_registration_13.png" alt="drawing" width="800"/>

### Restart and create a new user flow <AZURE_B2C_USER_FLOW_IOS_NAME> with the same parameters
<img src="azure_b2c_app_registration_14.png" alt="drawing" width="800"/>

### let's consider <AZURE_B2C_USER_FLOW_IOS_NAME> (e.g. B2C_1_IOS) and <AZURE_B2C_USER_FLOW_WEB_ANDROID_NAME> (e.g. B2C_1_WEB_ANDROID)

### For both user flows (<AZURE_B2C_USER_FLOW_WEB_ANDROID_NAME> and AZURE_B2C_USER_FLOW_IOS_NAME), open and click on Application claims. Then enable Email Addresses and save
<img src="azure_b2c_app_registration_15.png" alt="drawing" width="800"/>

### For IOs user flow only (<AZURE_B2C_USER_FLOW_IOS_NAME>), click on Properties and go to Token compatibility settings. Select the URL with /tfp/, then save
<img src="azure_b2c_app_registration_16.png" alt="drawing" width="800"/>

## 2.4 API settings

### Go back to App settings and click on Expose an API. Then click on Add a scope
<img src="azure_b2c_app_registration_17.png" alt="drawing" width="800"/>

### Click on Save and continue without modifying the URI
<img src="azure_b2c_app_registration_18.png" alt="drawing" width="800"/>

### Fill 'read' as name, display name and description, then click on Add scope
<img src="azure_b2c_app_registration_19.png" alt="drawing" width="800"/>

### Copy the scope URI(<AZURE_B2C_EXPOSED_API_URI>)
<img src="azure_b2c_app_registration_20.png" alt="drawing" width="800"/>

### Click on API permission and + Add a permission
<img src="azure_b2c_app_registration_21.png" alt="drawing" width="800"/>

### Select Microsoft APIs and click on Microsoft Graph
<img src="azure_b2c_app_registration_22.png" alt="drawing" width="800"/>

### Click on Application permissions
<img src="azure_b2c_app_registration_23.png" alt="drawing" width="800"/>

### Enable Application.Read.All into Application scope, then Add permissions
<img src="azure_b2c_app_registration_24.png" alt="drawing" width="800"/>

### Add a new permission and click on APIs my organization uses. Then click ok the App name
<img src="azure_b2c_app_registration_25.png" alt="drawing" width="800"/>

### Into Delegated permissions, enable read permission, then click on Add permissions
<img src="azure_b2c_app_registration_26.png" alt="drawing" width="800"/>

### click on Grant admin consent
<img src="azure_b2c_app_registration_27.png" alt="drawing" width="800"/>
<img src="azure_b2c_app_registration_28.png" alt="drawing" width="800"/>

# 3 Azure B2C Web authentifications settings

### Click on Authentication and + Add a platform
<img src="azure_b2c_app_registration_29.png" alt="drawing" width="800"/>

### Click on WEB
<img src="azure_b2c_app_registration_30.png" alt="drawing" width="800"/>

### Fill Redirect URIs with <MICROSFT_IDP_CALLBACK_URL> obtained before. Enable Acces token and ID tokens, then click on Configure. 
<img src="azure_b2c_app_registration_31.png" alt="drawing" width="800"/>

### Add a new platform and select Single-page application
<img src="azure_b2c_app_registration_32.png" alt="drawing" width="800"/>

### Fill Redirects URIs with application URI (localhost host is supported for testing). Then click on Configure
<img src="azure_b2c_app_registration_33.png" alt="drawing" width="800"/>

# 4 Azure B2C Android authentifications settings
## 4.1 Obtain signature hash base64
### on FLutter, move to \android subfolder and type ./gradlew signinReport
<img src="azure_b2c_app_registration_34.png" alt="drawing" width="800"/>

### Copy SHA1 hex signature
<img src="azure_b2c_app_registration_35.png" alt="drawing" width="800"/>

### Go to https://base64.guru/converter/encode/hex to convert signature du base64 and consider <SIGNATURE_HASH_BASE64>
<img src="azure_b2c_app_registration_36.png" alt="drawing" width="800"/>

## 4.2 Create Android authentication
### Go back to Authentication menu, Click on + Add a platform
<img src="azure_b2c_app_registration_37.png" alt="drawing" width="800"/>

### Fill Package name with the Android App package Name <ANDROID_APP_PACAKE_NAME> (available into android/app/build.gradle, cf namespace) and copy <SIGNATURE_HASH_BASE64> create above into Signature hash. Then click on Configure and Done.
<img src="azure_b2c_app_registration_38.png" alt="drawing" width="800"/>

### Copy <AZURE_B2C_ANDROID_REDIRECT_URI>
<img src="azure_b2c_app_registration_39.png" alt="drawing" width="800"/>

# 5 Azure B2C iOS authentifications settings
## 5.1 Create iOS authentication
### Go back to Authentication menu, Click on + Add a platform
<img src="azure_b2c_app_registration_40.png" alt="drawing" width="800"/>

### Fill Bundle ID with <IOS_BUNDLE_ID> (cf ......), then click on Configure and Done
<img src="azure_b2c_app_registration_41.png" alt="drawing" width="800"/>

### Copy <AZURE_B2C_IOS_REDIRECT_URI>
<img src="azure_b2c_app_registration_42.png" alt="drawing" width="800"/>

# 6 Flutter Azure B2C settings
## 6.1 iOS settings
### 6.1.1 Create azure_auth_config.json config file and save it into ios/Resources (create directory if necessary)

``` 
{
    "client_id" : "<AZURE_B2C_APP_CLIENT_ID>",
    "redirect_uri" : "<AZURE_B2C_IOS_REDIRECT_URI>",  /*something like msauth.<IOS_BUNDLE_ID>://auth"*/
    "account_mode" : "MULTIPLE",
    "broker_redirect_uri_registered": false,
    "authorities": [
      {
          "type": "B2C",
          "authority_url": "https://<AZURE_B2C_DIRECTORY_NAME>.b2clogin.com/tfp/<AZURE_B2C_DIRECTORY_NAME>.onmicrosoft.com/<AZURE_B2C_USER_FLOW_IOS_NAME>/",
          "default": true
      }
    ],
    "default_scopes": [
        "<AZURE_B2C_EXPOSED_API_URI>"
    ]
}
``` 

### 6.1.2 Add new keychain Group into xCode project
#### Open xCode project, go to Signing & Capabilities, add capability and select Keychain Groups. Then add com.microsoft.adalcache on iOS (and com.microsoft.identity.universalstorage on MacOS).

<img src="azure_b2c_xcode_1.png" alt="drawing" width="800"/>

### 6.1.3 Automatic copy azure_auth_config.json before building
#### On xCode, go to Build Phases and under Copy Bundle Resources, add azure_auth_config.json file

<img src="azure_b2c_xcode_2.png" alt="drawing" width="800"/>

### 6.1.4 Update Info.plist
#### Open Info.plist and insert following line inside <key>CFBundleURLTypes</key> array (replace <IOS_BUNDLE_ID>)

```  
<key>CFBundleURLTypes</key>
    <array>
        ....
        <dict>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>msauth.<IOS_BUNDLE_ID>></string>
            </array>
        </dict>
</array>
```

#### add also following lines somewhere else
```
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>msauthv2</string>
    <string>msauthv3</string>
</array>
```

### 6.1.5 Update AppDelegate.swift file
#### Add import MASL and add function below without deleting the previous one

```
import MSAL

....

override func application(
    _ app: UIApplication, 
    open url: URL, 
    options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        guard let sourceApplication = options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String else {
            return false
        }  
   
    return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApplication)
  }
```

## 6.2 Android settings
### 6.2.1 Configure app to use the INTERNET and ACCESS_NETWORK_STATE
#### Open android/app/src/main/AndroidManifest.xml and copy following line into manifest

```
    <manifest xmlns:android="http://schemas.android.com/apk/res/android">
        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        .....
```

#### and into <application>, add following activity and filter

```
    <activity android:name="com.microsoft.identity.client.BrowserTabActivity">
        <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="msauth" android:host = "<ANDROID_APP_PACKAGE_NAME>" android:path = "/<SIGNATURE_HASH_BASE64>" />
        </intent-filter>
    </activity>
```

### 6.2.2 Create azure_auth_config.json config file and save it into android/app/src/main/res/raw (create directory if necessary)

```
{
    "client_id" : "<AZURE_B2C_APP_CLIENT_ID>",
    "redirect_uri" : "<AZURE_B2C_ANDROID_REDIRECT_URI>", /*something like "msauth://<ANDROID_APP_PACKAGE_NAME>/<SIGNATURE_HASH_BASE64_URL_ENCODES>"*/
    "account_mode" : "MULTIPLE",
    "broker_redirect_uri_registered": false,
    "authorities": [
      {
          "type": "B2C",
          "authority_url": "https://<AZURE_B2C_DIRECTORY_NAME>.b2clogin.com/<AZURE_B2C_DIRECTORY_NAME>.onmicrosoft.com/<AZURE_B2C_USER_FLOW_WEB_ANDROID_NAME>/",
          "default": true
      }
    ],
    "default_scopes": [
        "<AZURE_B2C_EXPOSED_API_URI>"
    ]
}
```

## 6.3 Web settings
#### 6.3.1 Open web/index.html and add following line

```
<script type="text/javascript" src="https://alcdn.msauth.net/browser/2.14.0/js/msal-browser.min.js"></script>
```

#### 6.3.2 Create azure_auth_config.json config file and save it into assets

```
{
    "client_id" : "<AZURE_B2C_APP_CLIENT_ID>,
    "redirect_uri" : <WEB APP URL>, /*something like "http://localhost:53537" or any other URL "https://mywebsite"*/
    "cache_location": "sessionStorage",
    "interaction_mode": "popup",
    "authorities": [
      {
          "type": "B2C",
          "authority_url": "https://<AZURE_B2C_DIRECTORY_NAME>.b2clogin.com/<AZURE_B2C_DIRECTORY_NAME>.onmicrosoft.com/<AZURE_B2C_USER_FLOW_WEB_ANDROID_NAME>/,
          "default": true
      }
    ],
    "default_scopes": [
        "<AZURE_B2C_EXPOSED_API_URI>"
    ]
  }


```