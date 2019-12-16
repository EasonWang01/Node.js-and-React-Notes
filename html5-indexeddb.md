# HTML5 IndexedDB

indexedDB 為 大量資料的前端儲存方案，因為 localstorage 與 cookie 只能存少量資料。

## 文件：

```
https://developer.mozilla.org/zh-TW/docs/Web/API/IndexedDB_API/Using_IndexedDB
https://developers.google.com/web/ilt/pwa/working-with-indexeddb
```

## 使用：

新增如下檔案：

indexDB.js

```js
export default class indexDB {
  constructor(dbName) {
    this.dbName = dbName;
  }
  init(version, upgrade, done) {
    const indexedDB = window.indexedDB || window.webkitIndexedDB || window.mozIndexedDB;
    console.log('init');
    var openReq = indexedDB.open(this.dbName, version);
    openReq.onsuccess = function (e) {
      var db = e.target.result;
      // Chrome 23 still has setVersion so don't upgrade
      // unless the version is still old.
      if ('setVersion' in db && db.version < version) {
        var setVerReq = db.setVersion(version);
        setVerReq.onsuccess = function (e) {
          console.log('upgrading');
          upgrade(e.target.result.db);
          done();
        };
      } else {
        done();
      }
    };
    openReq.onupgradeneeded = function (e) {
      // Never gets raised before Chrome 23.
      console.log('upgrading');
      upgrade(e.target.result);
    };
    openReq.onerror = function (e) {
      console.log('init error');
    };
    openReq.onblocked = function (e) {
      console.log('init blocked');
    };
  }
  read(stores, fn, done) {
    return this.transaction('readonly', stores, fn, done);
  }
  readWrite(stores, fn, done) {
    return this.transaction('readwrite', stores, fn, done);
  }
  transaction(mode, stores, fn, done) {
    var openReq = indexedDB.open(this.dbName);
    openReq.onsuccess = function (e) {
      var db = e.target.result;
      var tx = db.transaction(stores, mode);
      tx.oncomplete = function (e) {
        if (done) {
          done();
        }
      };
      tx.onabort = function (e) {
        console.log('tx abort');
      };
      tx.onerror = function (e) {
        console.log('tx error');
      };
      fn(tx);
    };
    openReq.onerror = function (e) {
      console.log('open tx error');
    };
  }
}
```

app.js

```js
import indexDB from './indexDB';
const databaseName = 'projectDB';
    const projectsStoreName = 'projects';
    const dbVersion = 1;
    const indexdb = new indexDB(databaseName);
    indexdb.init(dbVersion, function (db) {
      db.createObjectStore(projectsStoreName, {
        autoIncrement: true
      });
    }, function () {
      console.log('ready');
      // 寫入資料
      indexdb.readWrite([projectsStoreName], function (tx) {
        var contact = {
          name: 123,
          email: 123
        };
        tx.objectStore(projectsStoreName).put(contact);
      }, function () {
        console.log('added');
      });
    });
```

## 執行後可看到如下畫面：

> ![](/assets/螢幕快照 2019-12-16 下午4.25.52.png)key 為 1是因為使用了 auto-increment

## HTML 範例：

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>IndexedDB Example</title>
</head>
<body>
    <h1>IndexedDB Example</h1>

    <p>
        <fieldset id="addForm">
            <legend>Add Contact</legend>
            <label>Name</label>
            <input id="nameInput">
            <label>Email</label>
            <input id="emailInput">
            <button id="addButton">Add</button>
        </fieldset>
    </p>

    <table id="contacts">
        <thead>
            <tr><th>Name</th><th>Email</th></tr>
        </thead>
        <tbody>
        </tbody>
    </table>

    <p>
        <button id="populateButton">Populate</button>
        <button id="deleteButton">Delete</button>
    </p>

    <script>
        // Helpers for working with indexedDB API.
        var indexedDB = window.indexedDB ||
                        window.webkitIndexedDB ||
                        window.mozIndexedDB;
        function DB(name) {
            this.init = function(version, upgrade, done) {
                console.log('init');
                var openReq = indexedDB.open(name, version);
                openReq.onsuccess = function(e) {
                    var db = e.target.result;
                    // Chrome 23 still has setVersion so don't upgrade
                    // unless the version is still old.
                    if ('setVersion' in db && db.version < version) {
                        var setVerReq = db.setVersion(version);
                        setVerReq.onsuccess = function(e) {
                            console.log('upgrading');
                            upgrade(e.target.result.db);
                            done();
                        };
                    } else {
                        done();
                    }
                };
                openReq.onupgradeneeded = function(e) {
                    // Never gets raised before Chrome 23.
                    console.log('upgrading');
                    upgrade(e.target.result);
                };
                openReq.onerror = function(e) {
                    console.log('init error');
                };
                openReq.onblocked = function(e) {
                    console.log('init blocked');
                };
            };
            this.read = function(stores, fn, done) {
                return this.transaction('readonly', stores, fn, done);
            };
            this.readWrite = function(stores, fn, done) {
                return this.transaction('readwrite', stores, fn, done);
            };
            this.transaction = function(mode, stores, fn, done) {
                var openReq = indexedDB.open(name);
                openReq.onsuccess = function(e) {
                    var db = e.target.result;
                    var tx = db.transaction(stores, mode);
                    tx.oncomplete = function(e) {
                        if (done) {
                            done();
                        }
                    };
                    tx.onabort = function(e) {
                        console.log('tx abort');
                    };
                    tx.onerror = function(e) {
                        console.log('tx error');
                    };
                    fn(tx);
                };
                openReq.onerror = function(e) {
                    console.log('open tx error');
                };
            };
        }
        DB.deleteDatabase = function(name, done) {
            var delReq = indexedDB.deleteDatabase(name);
            delReq.onsuccess = function(e) {
                // Not triggered before Chrome 23.
                done();
            };
            delReq.onerror = function(e) {
                console.log('delete error');
            };
            delReq.onblocked = function(e) {
                console.log('delete blocked');
            };
        };
    </script>

    <!-- For generating fake names and email addresses. -->
    <script src="https://raw.github.com/Marak/Faker.js/master/Faker.js"></script>

    <script>
        // The actual script for this page.
        var databaseName = 'ContactsDB';
        var contactsStoreName = 'contacts';
        var contactsDB = new DB(databaseName);
        var contacts = document.getElementById('contacts');
        contactsDB.init(1, function(db) {
            db.createObjectStore(contactsStoreName, {
                autoIncrement: true
            });
        }, function() {
            console.log('ready');
            loadContactsTable();
        });
        function loadContactsTable() {
            contactsDB.read([ contactsStoreName ], function(tx) {
                var cursor = tx.objectStore(contactsStoreName).openCursor();
                cursor.onsuccess = function(e) {
                    if (e.target.result) {
                        addContactToTable(e.target.result.value);
                        e.target.result.continue();
                    }
                };
                cursor.onerror = function(e) {
                    console.log('cursor error');
                };
            });
        }
        function addContactToTable(contact) {
            var newRow = contacts.insertRow(-1);
            var nameCell = newRow.insertCell(-1);
            nameCell.textContent = contact.name;
            var emailCell = newRow.insertCell(-1);
            emailCell.textContent = contact.email;
        }
        var nameInput = document.getElementById('nameInput');
        var emailInput = document.getElementById('emailInput');
        document.getElementById('addButton').onclick = function(e) {
            e.preventDefault();
            var name = nameInput.value;
            var email = emailInput.value;
            console.log('adding');
            contactsDB.readWrite([ contactsStoreName ], function(tx) {
                var contact = {
                    name: name,
                    email: email
                };
                tx.objectStore(contactsStoreName).put(contact);
                addContactToTable(contact);
            }, function() {
                console.log('added');
                nameInput.value = '';
                emailInput.value = '';
                nameInput.focus();
            });
        };
        document.getElementById('populateButton').onclick = function(e) {
            e.preventDefault();
            createFakeContacts();
        };
        function createFakeContacts() {
            console.log('generating fake contacts');
            contactsDB.readWrite([ contactsStoreName ], function(tx) {
                for (var i = 0, n = 10; i < n; i++) {
                    var name = Faker.Name.firstName();
                    var email = name.toLowerCase() +
                                Math.round(Math.random() * 1000) +
                                '@example.' +
                                Faker.Helpers.randomize(definitions.domain_suffix());
                    var contact = {
                        name: name,
                        email: email
                    };
                    tx.objectStore(contactsStoreName).put(contact);
                    addContactToTable(contact);
                }
            }, function() {
                console.log('done generating fake contacts');
            });
        }
        document.getElementById('deleteButton').onclick = function(e) {
            e.preventDefault();
            console.log('deleting');
            DB.deleteDatabase(databaseName, function() {
                console.log('deleted');
            });
        };
    </script>
</body>
</html>
```



