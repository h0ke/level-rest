# level-rest [![Build Status](https://secure.travis-ci.org/shama/level-rest.png?branch=master)](http://travis-ci.org/shama/level-rest)

> A REST adapter for [LevelUP](https://github.com/rvagg/node-levelup)

## Example

```js
var levelup = require('levelup')
var LevelREST = require('level-rest')

var db = levelup('./mydb', {valueEncoding: 'json'})
var rest = new LevelREST(db, {
  id: 'id',
  separator: ':',
  metaKey: 'meta',
  serialize: function(url) {
    url = url.split('/')
    return { api: url[0], id: url[1] }
  },
})

// POST data in
rest.post('people', { id: 1, name: 'Kyle' }, function() {
  console.log('Saved!')
})

// GET all people
rest.get('people').on('data', function(data) {
  data.people.forEach(function(person) {
    console.log('Hi ' + person.name)
  })
})

// GET one person by id
rest.get('people/1').on('data', function(data) {
  console.log('Hi ' + data.person.name)
})

// PUT data into one person
rest.put('people/1', { name: 'Dude' }, function() {
  console.log('Person 1 is now named Dude')
})

// DELETE a person
rest.delete('people/1', function() {
  console.log('Person 1 has been deleted')
})
```

## License
Copyright (c) 2013 Kyle Robinson Young  
Licensed under the MIT license.