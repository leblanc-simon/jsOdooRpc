# jsOdooRpc

Library to communicate into Odoo and other with Javascript and JSON-RPC.

This Library has no dependency. It's [Vanilla Javascript](http://vanilla-js.com/) !

*All return of the methods are Promise (except setHost method)*

# Installation

```html
<script src="/path/to/odoo_jsonrpc.js"></script>
```

# Usage

* [Set the hostname](#set-the-hostname)
* [Connect into Odoo](#connect-into-odoo)
* [Session](#session)
    * [Get informations about current session](#get-informations-about-current-session)
    * [Change password of the account](#change-password-of-the-account)
    * [Get the list of availables languages](#get-the-list-of-availables-languages)
    * [Get the list of availables modules](#get-the-list-of-availables-modules)
* [Database](#database)
    * [List databases](#list-databases)
    * [Create a new database](#create-a-new-database)
    * [Duplicate a database](#duplicate-a-database)
    * [Drop a database](#drop-a-database)
* [Model](#model)
    * [Find a record by id](#find-a-record-by-id)
    * [Search and read into a model](#search-and-read-into-a-model)
    * [Create a record](#create-a-record)
    * [Update a record](#update-a-record)
    * [Remove a record](#remove-a-record)
    * [Call a custom method](#call-a-custom-method)
    

## Set the hostname

It's the only method with doesn't return a Promise.

```javascript
// Without schema and port
odoo_jsonrpc.setHost('localhost');

// Or with scheme
odoo_jsonrpc.setHost('https://localhost');

// Or with no standard port
odoo_jsonrpc.setHost('localhost:8000');
```

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

### Get the list of availables languages

```javascript
odoo_jsonrpc.session.getLanguages().then(
    function (result) {
        var iterator = 0,
            length = result.length
        ;
        for (; iterator < length; iterator += 1) {
            console.log('Language : ' + result[iterator][1]);
        }
    },
    function (error) {
        console.error(error);
    }
);
```

### Get the list of availables modules

```javascript
odoo_jsonrpc.session.getModules().then(
    function (result) {
        var iterator = 0,
            length = result.length
        ;
        for (; iterator < length; iterator += 1) {
            console.log('Module : ' + result[iterator]);
        }
    },
    function (error) {
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

### Duplicate a database

```javascript
odoo_jsonrpc.database.duplicate('database_source', 'new_database_name', 'master_password').then(
    function (result) {
        console.log('Database duplicated')
    },
    function (error) {
        console.error('Fail to duplicate database');
        console.error(error);
    }
);
```

### Drop a database

```javascript
odoo_jsonrpc.database.drop('database_name', 'master_password').then(
    function (result) {
        console.log('Database droped')
    },
    function (error) {
        console.error('Fail to drop database');
        console.error(error);
    }
);
```

## Model

### Find a record by id

```javascript
odoo_jsonrpc.model.find('res.partner', 1, ['name']).then(
    function (result) {
        console.log('Partner : ' + result.name);
    },
    function (error) {
        // an error occured
         console.error(error);
    }
);
```

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

### Create a record

```javascript
var datas = {
    name: 'Partern name',
    phone: '+33100000000'
};

odoo_jsonrpc.model.create('res.partner', datas).then(
    function (result) {
        console.log(result);
    },
    function (error) {
        // an error occured
         console.error(error);
    }
);
```

### Update a record

```javascript
var datas = {
    name: 'Partern name updated',
    phone: '+33100000000'
};

odoo_jsonrpc.model.write('res.partner', 1, datas).then(
    function (result) {
        console.log(result);
    },
    function (error) {
        // an error occured
         console.error(error);
    }
);
```

### Remove a record

```javascript
odoo_jsonrpc.model.remove('res.partner', 5).then(
    function (result) {
        console.log(result);
    },
    function (error) {
        // an error occured
         console.error(error);
    }
);
```

### Call a custom method

```javascript
var args = {
    arg1: 'value1'
};
var kwargs;

odoo_jsonrpc.model.call('res.partner', 'my_custom_method', args, kwargs).then(
    function (result) {
        console.log(result);
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
