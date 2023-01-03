# wixServicenow
This is an experimental development of Wix Velo to Servicenow API

## Create Integration user

### Set up New Integration User

Name the User to identify the type of integration 
for example: wixVeloIntegration

1. Create new User
2. Check "Password needs to Reset"
3. Check "Internal Integration User"
4. Set password and Save
5. Login as new user and set new Password
6. Greate new Group name it "REST API"
7. Add Roles: rest_api_explorer, rest_service

Now we have our integration user set up

https://docs.servicenow.com/bundle/tokyo-application-development/page/integrate/inbound-rest/concept/c_RESTAPI.html

### Service Now REST API Explorer
get you HTTP Method / URI


### Set Basic Encryption
Create Basic Authentication Header Generator
https://www.blitter.se/utils/basic-authentication-header-generator/

view snAPI.jsw for backend functions

