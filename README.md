# hapi-cron
Cron jobs for internal hapi.js routes

## Installation
```
npm install --save hapi-cron
```

## Usage
```
const Hapi = require('hapi');
const server = new Hapi.Server();
server.connection();

server.register({
    register: require('hapi-cron'),
    options: {
        jobs: [{
            name: 'testcron',
            time: '*/10 * * * * *',
            timezone: 'Europe/London',
            request: {
                method: 'GET',
                url: '/test-url'
            },
            callback
        }]
    },
}, (err) => {

    if (err) {
        return console.error(err);
    }
    
    server.start(() => {
    
        console.info(`Server started at ${ server.info.uri }`);
    });
});
```

## Options

* `name` - [REQUIRED] - The name of the cron job. This must be unique.
* `time` - [REQUIRED] - A valid cron value. [See cron configuration](#cron-configuration)
* `timezone` - [REQUIRED] - A valid timezone.
* `request` - [REQUIRED] - The request object containing the route url path. Other [options](https://hapijs.com/api#serverinjectoptions-callback) can also be passed into the request object.
    * `url` - [REQUIRED] - Route path to request
* `callback` - [OPTIONAL] - Callback function to run after the route has been requested. The function will contain the response object from the request.

Please note that the plugin only works when the server contains exactly one connection.

## Cron configuration
This plugin uses the [node-cron](https://github.com/kelektiv/node-cron) module to setup the cron job. 

### Available Cron patterns:

```
Asterisk. E.g. *
Ranges. E.g. 1-3,5
Steps. E.g. */2
```
    

[Read up on cron patterns here](http://crontab.org). Note the examples in the link have five fields, and 1 minute as the finest granularity, but the node cron module allows six fields, with 1 second as the finest granularity.

### Cron Ranges

When specifying your cron values you'll need to make sure that your values fall within the ranges. For instance, some cron's use a 0-7 range for the day of week where both 0 and 7 represent Sunday. We do not.

 * Seconds: 0-59
 * Minutes: 0-59
 * Hours: 0-23
 * Day of Month: 1-31
 * Months: 0-11
 * Day of Week: 0-6



