---
layout: page
title: Integrazione database Express
menu: guide
lang: it
---

# Integrazione database

L'aggiunta della funzionalità che consente di connettere i database alle applicazioni Express è solo un modo per caricare un driver Node.js appropriato per il database nell'applicazione. Questo documento spiega brevemente come aggiungere e utilizzare alcuni dei moduli Node.js più noti per i sistemi database nell'applicazione Express:

* [Cassandra](#cassandra)
* [CouchDB](#couchdb)
* [LevelDB](#leveldb)
* [MySQL](#mysql)
* [MongoDB](#mongo)
* [Neo4j](#neo4j)
* [PostgreSQL](#postgres)
* [Redis](#redis)
* [SQLite](#sqlite)
* [ElasticSearch](#elasticsearch)

<div class="doc-box doc-notice" markdown="1">
Questi driver di database sono tra quelli più disponibili.  Per altre opzioni,
effettuare una ricerca nel sito [npm](https://www.npmjs.com/).
</div>

<a name="cassandra"></a>

## Cassandra

**Modulo**: **Installazione**
[cassandra-driver](https://github.com/datastax/nodejs-driver)


<pre>
<code class="language-sh" translate="no">
$ npm install cassandra-driver
</code>
</pre>

**Esempio**

<pre>
<code class="language-javascript" translate="no">
var cassandra = require('cassandra-driver');
var client = new cassandra.Client({ contactPoints: ['localhost']});

client.execute('select key from system.local', function(err, result) {
  if (err) throw err;
  console.log(result.rows[0]);
});
</code>
</pre>

<a name="couchdb"></a>

## CouchDB

**Modulo**: **Installazione**
[nano](https://github.com/dscape/nano)


<pre>
<code class="language-sh" translate="no">
$ npm install nano
</code>
</pre>

**Esempio**

<pre>
<code class="language-javascript" translate="no">
var nano = require('nano')('http://localhost:5984');
nano.db.create('books');
var books = nano.db.use('books');

//Insert a book document in the books database
books.insert({name: 'The Art of war'}, null, function(err, body) {
  if (!err){
    console.log(body);
  }
});

//Get a list of all books
books.list(function(err, body){
  console.log(body.rows);
}
</code>
</pre>

<a name="leveldb"></a>

## LevelDB

**Modulo**: **Installazione**
[levelup](https://github.com/rvagg/node-levelup)


<pre>
<code class="language-sh" translate="no">
$ npm install level levelup leveldown
</code>
</pre>

**Esempio**

<pre>
<code class="language-javascript" translate="no">
var levelup = require('levelup');
var db = levelup('./mydb');

db.put('name', 'LevelUP', function (err) {

  if (err) return console.log('Ooops!', err);
  db.get('name', function (err, value) {
    if (err) return console.log('Ooops!', err);
    console.log('name=' + value)
  });

});
</code>
</pre>

<a name="mysql"></a>

## MySQL

**Modulo**: **Installazione**
[mysql](https://github.com/felixge/node-mysql/)


<pre>
<code class="language-sh" translate="no">
$ npm install mysql
</code>
</pre>

**Esempio**

<pre>
<code class="language-javascript" translate="no">
var mysql      = require('mysql');
var connection = mysql.createConnection({
  host     : 'localhost',
  user     : 'dbuser',
  password : 's3kreee7'
});

connection.connect();

connection.query('SELECT 1 + 1 AS solution', function(err, rows, fields) {
  if (err) throw err;
  console.log('The solution is: ', rows[0].solution);
});

connection.end();
</code>
</pre>

<a name="mongo"></a>

## MongoDB

**Modulo**: **Installazione**
[mongodb](https://github.com/mongodb/node-mongodb-native)


<pre>
<code class="language-sh" translate="no">
$ npm install mongodb
</code>
</pre>

**Esempio**

<pre>
<code class="language-javascript" translate="no">
var MongoClient = require('mongodb').MongoClient;

MongoClient.connect('mongodb://localhost:27017/animals', function(err, db) {
  if (err) {
    throw err;
  }
  db.collection('mammals').find().toArray(function(err, result) {
    if (err) {
      throw err;
    }
    console.log(result);
  });
});
</code>
</pre>

Se si desidera un driver del modello oggetto per MongoDB, consultare [Mongoose](https://github.com/LearnBoost/mongoose).

<a name="neo4j"></a>

## Neo4j

**Modulo**: **Installazione**
[apoc](https://github.com/hacksparrow/apoc)


<pre>
<code class="language-sh" translate="no">
$ npm install apoc
</code>
</pre>

**Esempio**

<pre>
<code class="language-javascript" translate="no">
var apoc = require('apoc');

apoc.query('match (n) return n').exec().then(
  function (response) {
    console.log(response);
  },
  function (fail) {
    console.log(fail);
  }
);
</code>
</pre>

<a name="postgres"></a>

## PostgreSQL

**Modulo**: **Installazione**
[pg](https://github.com/brianc/node-postgres)


<pre>
<code class="language-sh" translate="no">
$ npm install pg
</code>
</pre>

**Esempio**

<pre>
<code class="language-javascript" translate="no">
var pg = require('pg');
var conString = "postgres://username:password@localhost/database";

pg.connect(conString, function(err, client, done) {

  if (err) {
    return console.error('error fetching client from pool', err);
  }
  client.query('SELECT $1::int AS number', ['1'], function(err, result) {
    done();
    if (err) {
      return console.error('error running query', err);
    }
    console.log(result.rows[0].number);
  });

});
</code>
</pre>

<a name="redis"></a>

## Redis

**Modulo**: **Installazione**
[redis](https://github.com/mranney/node_redis)


<pre>
<code class="language-sh" translate="no">
$ npm install redis
</code>
</pre>

**Esempio**

<pre>
<code class="language-javascript" translate="no">
var client = require('redis').createClient();

client.on('error', function (err) {
  console.log('Error ' + err);
});

client.set('string key', 'string val', redis.print);
client.hset('hash key', 'hashtest 1', 'some value', redis.print);
client.hset(['hash key', 'hashtest 2', 'some other value'], redis.print);

client.hkeys('hash key', function (err, replies) {

  console.log(replies.length + ' replies:');
  replies.forEach(function (reply, i) {
    console.log('    ' + i + ': ' + reply);
  });

  client.quit();

});
</code>
</pre>

<a name="sqlite"></a>

## SQLite

**Modulo**: **Installazione**
[sqlite3](https://github.com/mapbox/node-sqlite3)


<pre>
<code class="language-sh" translate="no">
$ npm install sqlite3
</code>
</pre>

**Esempio**

<pre>
<code class="language-javascript" translate="no">
var sqlite3 = require('sqlite3').verbose();
var db = new sqlite3.Database(':memory:');

db.serialize(function() {

  db.run('CREATE TABLE lorem (info TEXT)');
  var stmt = db.prepare('INSERT INTO lorem VALUES (?)');

  for (var i = 0; i < 10; i++) {
    stmt.run('Ipsum ' + i);
  }

  stmt.finalize();

  db.each('SELECT rowid AS id, info FROM lorem', function(err, row) {
    console.log(row.id + ': ' + row.info);
  });
});

db.close();
</code>
</pre>

<a name="elasticsearch"></a>

## ElasticSearch

**Modulo**: **Installazione**
[elasticsearch](https://github.com/elastic/elasticsearch-js)


<pre>
<code class="language-sh" translate="no">
$ npm install elasticsearch
</code>
</pre>

**Esempio**

<pre>
<code class="language-javascript" translate="no">
var elasticsearch = require('elasticsearch');
var client = elasticsearch.Client({
  host: 'localhost:9200'
});

client.search({
  index: 'books',
  type: 'book',
  body: {
    query: {
      multi_match: {
        query: 'express js',
        fields: ['title', 'description']
      }
    }
  }
}).then(function(response) {
  var hits = response.hits.hits;
}, function(error) {
  console.trace(error.message);
});
</code>
</pre>
