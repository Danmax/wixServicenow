# wixServicenow
This is an experimental development of Wix Velo to Servicenow API


Here we will take API package and transformit for Servicenow Connection API

# Create Secrete API connection 

User ID - Create new Integration user in SN
Password 
Token ?

Step[operation 2 ]

Project File

import { queryRecords, createRecords } from '@velo/salesforce-integration-backend';

export async function getPhoneBasedOnName(accountName) {
    const query = `SELECT phone FROM Account WHERE name='${accountName}' ORDER BY CreatedDate DESC`;
    try {
        validateName(accountName);
        const result = await queryRecords(query);
        return result.records[0].Phone;
    } catch (err) {
        return Promise.reject('Query records failed. Info: ' + err);
    }
}

export async function createRecordsWrapper(record) {
    const { name, phone } = record;
    try {
        validateName(name);
        validatePhoneNumber(phone);
        try {
            const res = await createRecords('Account', record);
            return res.success;
        } catch (err) {
            return Promise.reject(err);
        }
    } catch (validationError) {
        return Promise.reject(validationError.toString());
    }
}

function validateName(name) {
    const regex = new RegExp('^[A-Za-z\\s\\d-]+$');
    if (!regex.test(name)) {
        throw new Error(`Provided name must only contains English letters (got: ${name})`);
    }
    return true;
}

function validatePhoneNumber(phone) {
    const regex = new RegExp('^[0-9]+$');
    if (!regex.test(phone)) {
        throw new Error(`Provided phone must only contains digits (got: ${phone})`);
    }
    return true;
}



////////////////
salesforce.js

import jsforce from 'jsforce';
import { getSecret } from 'wix-secrets-backend';

const loginUrl = 'https://login.salesforce.com';
const sfConn = new jsforce.Connection({ loginUrl });

async function authenticateAccount() {
    try {
        const sfAccountSecretStr = await getSecret('velo-salesforce-credentials');
        const sfAccountSecret = JSON.parse(sfAccountSecretStr);
        await sfConn.login(sfAccountSecret.username, sfAccountSecret.password + sfAccountSecret.token);
    } catch (err) {
        return Promise.reject(err);
    }
}

// Query in SOQL (https://jsforce.github.io/document/#using-soql)
// Example: SELECT Id, Name FROM Contact
export async function queryRecords(queryStr) {
    try {
        await authenticateAccount();
        return await sfConn.query(queryStr);
    } catch (err) {
        return Promise.reject(err);
    }
}

// Retrieve from a Salesforce Object the records with the specified IDs
export async function retrieveRecords(sfObjName, ids) {
    try {
        await authenticateAccount();
        return await sfConn.sobject(sfObjName).retrieve(ids);
    } catch (err) {
        return Promise.reject(err);
    }
}

// Creates new given records in a Salesforce Object 
export async function createRecords(sfObjName, records) {
    try {
        await authenticateAccount();
        return await sfConn.sobject(sfObjName).create(records);
    } catch (err) {
        return Promise.reject(err);
    }
}

// Updates given records in a Salesforce Object 
export async function updateRecords(sfObjName, records) {
    console.log(typeof records)
    try {
        await authenticateAccount();
        return await sfConn.sobject(sfObjName).update(records);
    } catch (err) {
        return Promise.reject(err);
    }
}

// Deletes given records in Salesforce Object 
export async function deleteRecords(sfObjName, ids) {
    try {
        await authenticateAccount();
        return await sfConn.sobject(sfObjName).del(ids);
    } catch (err) {
        return Promise.reject(err);
    }
}
