Angel List
=============

AngelList wrapper purely written in Node.js

## Getting Started

Wrapper can be downloaded from npm:

`npm install angel.co --save`

Once installed, its easy to use it:

```javascript
var angel = require('angel.co')('APP_ID', 'APP_SECRET');
```

Please follow the documentation from [https://angel.co/api/](AngelList documentation)

## Authentication

Module includes ability to let the user authenticate from AngelList. For now, its hard coded and asks for all ther permissions but you can change sa you like ;-)

Below example has been implemented with `Express.js` you may modify it.

```javascript
app.get('/auth/angel-list', function(req, res) {
    res.redirect(angel.getAuthorizeUrl());
});

app.get('/auth/angel-list/callback', function(req, res) {
    angel.auth.requestAccessToken(req.query.code, function(err, response) {
        if ( err )
            return console.error(err); //Something went wrong.
            
        // I got the Token. Ain't you?
        app.set('my_key_to_token', response.access_token); // Persist it anywhere.
        res.redirect('/'); // Go back to the homepage.
    });
});
```

`PS:` Your callback url must be similar to what you have configured while creating an app on AngelList.

## Users

Specification for the user is available at: https://angel.co/api/spec/users

### me
It returns the data of logged in user.

```javascript
angel.users.me('access_token_here', function(err, body) {
    if ( err )
        return console.log(err);

    console.log(body);
});
```

### Get User
To get the user, first argument must be the userId while another would be the set for query parameters.

```javascript
angel.users.user('467664', {'include_details': 'investor'}, function(err, body) {
    if ( err )
        return console.log(err);

    console.log(body);
});
```

### Get User Roles
Returns all the roles of user in organizations in which user is tagged in.

```javascript
angel.users.roles('USER_ID', function(err, body) {
     if ( err )
        return console.log(err);

    console.log(body);
});
```

### Batch Request
Helps to query multiple users at a time. First argument must be an array with the list of user ids.

```javascript
angel.users.batch(['155', 671], function(err, body) {
     if ( err )
        return console.log(err);

    console.log(body);
});
```

### Users Search
Search the users by `slug` and/or `md5` of the user's `email`

```javascript
angel.users.search({'slug': "hamza-waqas"}, function(err, body) {
    if ( err )
        return console.log(err);

    console.log(body);
});
```

### Users by Tag
Returns all the users related to the tag. Tag must be either `LocationTag` or `MarketTag`. First argument is the tagId, while another is the `object` with k/v of query parameters.

```javascript
angel.users.tags('1654', {
    include_children: true,
    investors: 'by_residence'
}, function(err, body) {
    if ( err )
        return console.log(err);

    console.log(body);
});
```

## Startups

Specification of startup is available at https://angel.co/api/spec/startups

### Get Startup
Returns the information of the given startup Id.

```javascript
angel.startups.get(6702, function(err, body) {
    if ( err )
        return console.log(err);

    console.log(body);
});
```

### Get Startup Comments
Retrieves all the comments of startup.

```javascript
angel.startups.comments(6702, function(err, body) {
    if ( err )
        return console.log(err);

    console.log(body);
});
```

### Startup Roles
Returns the company's startup roles. Direction must be either `Incoming` or `Outgoing`.

```javascript
angel.startups.roles(6702, {direction: "outgoing"}, function(err, body) {
    if ( err )
        return console.log(err);

    console.log(body);
});
```

### Filter Startups
Filters the startups by criteria.

```javascript
angel.startups.filter({filter: "raising"}, function(err, body) {
    if ( err )
        return console.log(err);

    console.log(body);
});
```

### Get Startups by Tag
Returns companies that are tagged with the given tag or a child of the given tag. Results are paginated and ordered according to the order parameter.

```javascript
angel.startups.tags(1654, {order: "asc"}, function(err, body) {
    if ( err )
        return console.log(err);

    console.log(body);
});
```

## Feeds

Specification for feeds entity is available at: https://angel.co/api/spec/activity_feeds

### Consume
Returns site activity. If authenticated and the personalized parameter is passed in, only activity from the authenticated user's social graph is returned. No more than 25 items will be returned. Results are paginated and ordered by most recent story first, unless a since parameter is given.

```javascript
angel.feeds.consume({
    since: 'PRESAVED_UNIX_TIMESTAMP',
    personalized: 1,
    access_token: 'access_token_here'
}, function(err, body) {
    if ( err )
        return console.log(err);

    console.log(body);
});
```

## Contribution

This wrapper is solely written at `Kuew Inc` by [Hamza Waqas](http://github.com/ArkeologeN)