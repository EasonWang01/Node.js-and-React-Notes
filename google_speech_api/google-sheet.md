# Google Sheet

Google試算表 API

以下為官方Node.js範例

[https://developers.google.com/sheets/api/quickstart/nodejs](https://developers.google.com/sheets/api/quickstart/nodejs)

1.點選Enable The Sheet API然後下載憑證

2.創建新的google sheet 並更改 code 的 spreadsheetId![](/assets/螢幕快照 2019-05-30 上午10.48.31132234.png)3.npm install googleapis

程式範例:

```js
const fs = require('fs');
const readline = require('readline');
const { google } = require('googleapis');

// If modifying these scopes, delete token.json.
var SCOPES = ['https://www.googleapis.com/auth/spreadsheets'];
// The file token.json stores the user's access and refresh tokens, and is
// created automatically when the authorization flow completes for the first
// time.
const TOKEN_PATH = 'token.json';

// Load client secrets from a local file.
fs.readFile('credentials.json', (err, content) => {
    if (err) return console.log('Error loading client secret file:', err);
    // Authorize a client with credentials, then call the Google Sheets API.
    authorize(JSON.parse(content), listMajors);
});

/**
 * Create an OAuth2 client with the given credentials, and then execute the
 * given callback function.
 * @param {Object} credentials The authorization client credentials.
 * @param {function} callback The callback to call with the authorized client.
 */
function authorize(credentials, callback) {
    const { client_secret, client_id, redirect_uris } = credentials.installed;
    const oAuth2Client = new google.auth.OAuth2(
        client_id, client_secret, redirect_uris[0]);

    // Check if we have previously stored a token.
    fs.readFile(TOKEN_PATH, (err, token) => {
        if (err) return getNewToken(oAuth2Client, callback);
        oAuth2Client.setCredentials(JSON.parse(token));
        callback(oAuth2Client);
    });
}

/**
 * Get and store new token after prompting for user authorization, and then
 * execute the given callback with the authorized OAuth2 client.
 * @param {google.auth.OAuth2} oAuth2Client The OAuth2 client to get token for.
 * @param {getEventsCallback} callback The callback for the authorized client.
 */
function getNewToken(oAuth2Client, callback) {
    const authUrl = oAuth2Client.generateAuthUrl({
        access_type: 'offline',
        scope: SCOPES,
    });
    console.log('Authorize this app by visiting this url:', authUrl);
    const rl = readline.createInterface({
        input: process.stdin,
        output: process.stdout,
    });
    rl.question('Enter the code from that page here: ', (code) => {
        rl.close();
        oAuth2Client.getToken(code, (err, token) => {
            if (err) return console.error('Error while trying to retrieve access token', err);
            oAuth2Client.setCredentials(token);
            // Store the token to disk for later program executions
            fs.writeFile(TOKEN_PATH, JSON.stringify(token), (err) => {
                if (err) return console.error(err);
                console.log('Token stored to', TOKEN_PATH);
            });
            callback(oAuth2Client);
        });
    });
}

/**
 * Prints the names and majors of students in a sample spreadsheet:
 * @see https://docs.google.com/spreadsheets/d/1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgvE2upms/edit
 * @param {google.auth.OAuth2} auth The authenticated Google OAuth client.
 */
function listMajors(auth) {
    const sheets = google.sheets({ version: 'v4', auth });
    sheets.spreadsheets.values.get({
        spreadsheetId: '1aCA49m9XIKbX3NZk4FA5WDvSfOQEAbAfFQ_-MSQGdAo',
        range: ['Sheet1!A1:B2'],
    }, (err, res) => {
        if (err) return console.log('The API returned an error: ' + err);
        const rows = res.data.values;
        if (rows.length) {
            rows.map((row) => {
                console.log(row);
            });
        } else {
            console.log('No data found.');
        }
    });
}
```

> 第一次執行後在console會產生網址 之後到那個網址會有hash 把它貼到console  之後本地電腦會產生token，供下次使用

Range可參考

```
Sheet1!A1:B2 refers to the first two cells in the top two rows of Sheet1.
Sheet1!A:A refers to all the cells in the first column of Sheet1.
Sheet1!1:2 refers to the all the cells in the first two rows of Sheet1.
Sheet1!A5:A refers to all the cells of the first column of Sheet 1, from row 5 onward.
A1:B2 refers to the first two cells in the top two rows of the first visible sheet.
Sheet1 refers to all the cells in Sheet1.
```

SCOPE\( read/write \)

如出現

```
   [ { message: 'Request had insufficient authentication scopes.',
       domain: 'global',
       reason: 'forbidden' } ] }
```

更改SCOPE記得把電腦內的./credential 憑證刪除剛產生的

[https://github.com/google/google-auth-library-nodejs/issues/168](https://github.com/google/google-auth-library-nodejs/issues/168)

```
value | describe
---- | ---
https://www.googleapis.com/auth/spreadsheets.readonly | Allows read-only access to the user's sheets and their properties.
https://www.googleapis.com/auth/spreadsheets |    Allows read/write access to the user's sheets and their properties.
https://www.googleapis.com/auth/drive.readonly |    Allows read-only access to the user's file metadata and file content.
https://www.googleapis.com/auth/drive.file |    Per-file access to files created or opened by the app.
https://www.googleapis.com/auth/drive |    Full, permissive scope to access all of a user's files. Request this scope only when it is strictly necessary.
```

#### 寫入範例\(記得改scope\)

```js
function listMajors(auth) {
    var sheets = google.sheets('v4');
    var values = [
        [
            12123
        ],
        // Additional rows ...
    ];
    var body = {
        values: values
    };
    sheets.spreadsheets.values.update({
        auth: auth,
        spreadsheetId: '1aCA49m9XIKbX3NZk4FA5WDvSfOQEAbAfFQ_-MSQGdAo',
        range: ['sheet1'],
        valueInputOption: 'RAW',
        resource: body
    }, function (err, result) {
        if (err) {
            // Handle error
            console.log(err);
        } else {
            console.log('%d cells updated.', result.updatedCells);
        }
    });
}
```

# 讀取A sheet並寫入 B sheet範例

```js
const fs = require('fs');
const readline = require('readline');
const { google } = require('googleapis');

const SCOPES = ['https://www.googleapis.com/auth/spreadsheets'];
const spreadsheetId = '1aCA49m9XIKbX3NZk4FA5WDvSfOQEAbAfFQ_-MSQGdAo';
const TOKEN_PATH = 'token.json';

// Load client secrets from a local file.
fs.readFile('credentials.json', (err, content) => {
    if (err) return console.log('Error loading client secret file:', err);
    // Authorize a client with credentials, then call the Google Sheets API.
    authorize(JSON.parse(content), doAction);
});

/**
* Create an OAuth2 client with the given credentials, and then execute the
* given callback function.
* @param {Object} credentials The authorization client credentials.
* @param {function} callback The callback to call with the authorized client.
*/
function authorize(credentials, callback) {
    const { client_secret, client_id, redirect_uris } = credentials.installed;
    const oAuth2Client = new google.auth.OAuth2(
        client_id, client_secret, redirect_uris[0]);

    // Check if we have previously stored a token.
    fs.readFile(TOKEN_PATH, (err, token) => {
        if (err) return getNewToken(oAuth2Client, callback);
        oAuth2Client.setCredentials(JSON.parse(token));
        callback(oAuth2Client);
    });
}

/**
* Get and store new token after prompting for user authorization, and then
* execute the given callback with the authorized OAuth2 client.
* @param {google.auth.OAuth2} oAuth2Client The OAuth2 client to get token for.
* @param {getEventsCallback} callback The callback for the authorized client.
*/
function getNewToken(oAuth2Client, callback) {
    const authUrl = oAuth2Client.generateAuthUrl({
        access_type: 'offline',
        scope: SCOPES,
    });
    console.log('Authorize this app by visiting this url:', authUrl);
    const rl = readline.createInterface({
        input: process.stdin,
        output: process.stdout,
    });
    rl.question('Enter the code from that page here: ', (code) => {
        rl.close();
        oAuth2Client.getToken(code, (err, token) => {
            if (err) return console.error('Error while trying to retrieve access token', err);
            oAuth2Client.setCredentials(token);
            // Store the token to disk for later program executions
            fs.writeFile(TOKEN_PATH, JSON.stringify(token), (err) => {
                if (err) return console.error(err);
                console.log('Token stored to', TOKEN_PATH);
            });
            callback(oAuth2Client);
        });
    });
}



function readSheet2(auth) {
    return new Promise((resolve, reject) => {
        const sheets = google.sheets({ version: 'v4', auth });
        sheets.spreadsheets.values.get({
            spreadsheetId,
            range: ['Sheet2'],
        }, (err, res) => {
            if (err) return console.log('The API returned an error: ' + err);
            const rows = res.data.values;
            if (rows.length) {
                rows.shift(); // 移除欄位名稱
                resolve(rows);
            } else {
                reject('No data found.');
            }
        });

    })
}


function updateSheet1(auth, values) {
    var sheets = google.sheets('v4');
    var body = {
        values
    };
    sheets.spreadsheets.values.append({
        auth,
        spreadsheetId,
        range: ['sheet1'],
        valueInputOption: 'RAW',
        resource: body
    }, function (err, result) {
        if (err) {
            // Handle error
            console.log(err);
        } else {
            console.log('%d cells updated.', result.updatedCells);
        }
    });
}

async function doAction(auth) {
  const sheet2Value = await readSheet2(auth);
  updateSheet1(auth, sheet2Value);
}
```



