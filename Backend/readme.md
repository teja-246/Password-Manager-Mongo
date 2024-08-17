const express = require('express')
const dotenv = require('dotenv')
dotenv.config()
const bodyparser = require('body-parser')
const cors = require('cors')
const { MongoClient } = require('mongodb');
// or as an es module:
// import { MongoClient } from 'mongodb'

// Connection URL
const url = 'mongodb://localhost:27017';
const client = new MongoClient(url);

// Database Name
const dbName = 'PassGUARD';
const app = express()
const port = 3000
app.use(bodyparser.json())
app.use(cors())

client.connect()

app.get('/', async (req, res) => {
    const db = client.db(dbName)
    const collection = db.collection("passwords")
    const findResult = await collection.find({}).toArray();
    // console.log('Found documents =>', findResult);
    res.json(findResult)
})

app.post('/', async (req, res) => {
    const password = req.body
    const db = client.db(dbName)
    const collection = db.collection("passwords")
    const findResult = await collection.insertOne(password)
    res.send({ success: true })
})

app.delete('/', async (req, res) => {
    const password = req.body
    const db = client.db(dbName)
    const collection = db.collection("passwords")
    const findResult = await collection.deleteOne(password)
    res.send({ result: findResult })
})

app.listen(port, () => {
    console.log(`Example app listening on port ${port}`)
})