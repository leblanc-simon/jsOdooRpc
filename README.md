# jsOdooRpc

Library to communicate into Odoo and other with Javascript and JSON-RPC.

This Library has no dependency. It's [Vanilla Javascript](http://vanilla-js.com/) !

# Installation

```html
<script src="/path/to/odoo_jsonrpc.js"></script>
```

# Usage

## Connect into Odoo

__It's require only once__. After a success login, all futures requests will be executed with our rights.

```javascript
odoo_jsonrpc.login('database', 'login', 'password').then(
    function (result) {
        console.log('You are logged !');
        console.log('Your UID is : ' + result.uid);
    },
    function (error) {
        console.error(error);
    }
);
```

## Session

### Get informations about current session

```javascript
odoo_jsonrpc.session.getInfos().then(
    function (result) {
        console.log(result);
    },
    function (error) {
        console.error(error);
    }
);
```

### Change password of the account

```javascript
odoo_jsonrpc.session.changePassword('your old password', 'your new pssword').then(
    function (result) {
        console.log('Password successful changed');
    },
    function (error) {
        console.error('Fail to change password');
        console.error(error);
    }
);
```

## Database

### List databases

```javascript
odoo_jsonrpc.database.list().then(
    function (result) {
        var iterator = 0, 
            length = result.length
        ;
        for (; iterator < length; iterator += 1) {
            console.log('Database : ' + result[iterator]);
        }
    },
    function (error) {
        console.error(error);
    }
);
```

### Create a new database

```javascript
odoo_jsonrpc.database.create('database_name', 'master_password', false, 'fr_FR', 'the admin password for the new database').then(
    function (result) {
        console.log('Database created')
    },
    function (error) {
        console.error('Fail to create database');
        console.error(error);
    }
);
```

## Model

### Search and read into a model

```javascript
odoo_jsonrpc.model.searchRead('res.partner', [['id', '=', 1]], ['name']).then(
    function (result) {
        // read the result
        if (0 === result.length) {
            console.log('No result for your request !');
            return;
        }

        var iterator = 0, 
            length = result.records.length
        ;
        for (; iterator < length; iterator += 1) {
            console.log('Partner : ' + result.records[iterator].name);
        }
    },
    function (error) {
        // an error occured
         console.error(error);
    }
);
```

License
=======

Licence WTFPL : http://sam.zoy.org/wtfpl/

Author
======

Simon Leblanc <contact@leblanc-simon.eu>
