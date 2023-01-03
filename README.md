# wixServicenow
This is an experimental development of Wix Velo to Servicenow API

## Create Integration user

Set up New Integration User

Name the User to edentify the type of integration 
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


or use Basic Authentication Header Generator
https://www.blitter.se/utils/basic-authentication-header-generator/

Login to your ServiceNow instance go to REST API Explorer



import {fetch} from 'wix-fetch';

// // ... REST Get Function

export async function getIncidents() {
    const settings = {
        method : 'GET',
        headers: {
            Accept: 'application/json',
            Authorization: `Basic d2l4dmVsbzpGcm93ZWFyMjAyMCE=`
        }
    };
    try {
        const fetchResponse = await fetch(`https://instanceName.service-now.com/api/now/table/x_snc_table_name?sysparm_fields=first_name%2Clast_name%2Cemail&sysparm_limit=10`, settings)
        const data = await fetchResponse.json();

        return data;    
    }
    catch(err) {
        return err

    }
}

export async function postMessage(first_name, last_name, email) {
    const fieldsObj = {name, industry_type, website}
    console.log(fieldsObj)
    const settings = {
        method : 'POST',
        body: JSON.stringify(fieldsObj),
        headers: {
            'Content-Type': 'application/json',
            Authorization: `Basic d42ldmVsbzpGcm93ZWFyMjAyMCE=`
        }
    };
    try{
        const fetchResponse = await fetch("https://instanceName.service-now.com/api/now/table/x_snc_table_name",settings)
        const data = await fetchResponse.json();

        console.log(data);
        return data
    }
    catch(err) {
        return err
    }
}





