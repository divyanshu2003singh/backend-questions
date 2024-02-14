
1. **Connect to a MongoDB database:**

```javascript
const mongoose = require('mongoose');

mongoose.connect('mongodb://localhost:27017/myDatabase', { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log('Connected to MongoDB'))
  .catch(err => console.error('Error connecting to MongoDB:', err));
```

Explanation:
- Require the `mongoose` library, which provides a MongoDB object modeling tool.
- Use `mongoose.connect()` to establish a connection to the MongoDB database. Replace `'mongodb://localhost:27017/myDatabase'` with your MongoDB connection string.
- Handle the promise returned by `mongoose.connect()` to log success or error.

2. **Insert a document into a MongoDB collection:**

```javascript
const mongoose = require('mongoose');

const Schema = mongoose.Schema;

const mySchema = new Schema({
  name: String,
  age: Number
});

const MyModel = mongoose.model('MyModel', mySchema);

const newData = new MyModel({ name: 'John', age: 30 });
newData.save()
  .then(doc => console.log('Document inserted:', doc))
  .catch(err => console.error('Error inserting document:', err));
```

Explanation:
- Define a schema using `mongoose.Schema()` which represents the structure of documents within a collection.
- Create a model using `mongoose.model()` passing in the name of the collection and the schema.
- Create a new document instance using the model constructor and `new`.
- Use the `save()` method to insert the document into the collection.

3. **Update a document in a MongoDB collection:**

```javascript
MyModel.findOneAndUpdate({ name: 'John' }, { age: 35 }, { new: true })
  .then(updatedDoc => console.log('Document updated:', updatedDoc))
  .catch(err => console.error('Error updating document:', err));
```

Explanation:
- Use `findOneAndUpdate()` to find a document with the given criteria (in this case, where the name is 'John') and update it with the new values provided.
- `{ new: true }` option returns the modified document rather than the original one.

4. **Delete a document from a MongoDB collection:**

```javascript
MyModel.findOneAndDelete({ name: 'John' })
  .then(deletedDoc => console.log('Document deleted:', deletedDoc))
  .catch(err => console.error('Error deleting document:', err));
```

Explanation:
- Use `findOneAndDelete()` to find and delete a document matching the provided criteria.

5. **Node.js API endpoint to fetch all documents from a MongoDB collection:**

```javascript
const express = require('express');
const MyModel = require('./models/MyModel'); // Import your model

const app = express();

app.get('/api/documents', (req, res) => {
  MyModel.find()
    .then(docs => res.json(docs))
    .catch(err => res.status(500).json({ error: err.message }));
});

app.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```

Explanation:
- Use Express to create an API endpoint at `/api/documents`.
- Inside the route handler, use `MyModel.find()` to fetch all documents from the collection.
- Respond with the fetched documents in JSON format.
- Handle errors with appropriate status codes and error messages.

6. **Node.js API endpoint to fetch a single document by ID from a MongoDB collection:**

```javascript
app.get('/api/documents/:id', (req, res) => {
  MyModel.findById(req.params.id)
    .then(doc => {
      if (!doc) {
        return res.status(404).json({ error: 'Document not found' });
      }
      res.json(doc);
    })
    .catch(err => res.status(500).json({ error: err.message }));
});
```

Explanation:
- Define a route parameter `:id` to capture the document ID.
- Use `MyModel.findById()` to find the document by its ID.
- If the document is not found, respond with a 404 status code and an error message.
- Otherwise, respond with the fetched document.

7. **Node.js function to perform aggregation operations in MongoDB:**

```javascript
async function performAggregation() {
  try {
    const result = await MyModel.aggregate([
      { $group: { _id: '$name', totalAge: { $sum: '$age' } } }
    ]);
    console.log(result);
  } catch (error) {
    console.error('Aggregation error:', error);
  }
}

performAggregation();
```

Explanation:
- Use `MyModel.aggregate()` to perform aggregation operations.
- Provide an array of stages for the aggregation pipeline. In this example, we're grouping documents by `name` field and calculating the total age for each group.
- Handle the result or any errors accordingly.


8. **Node.js script to perform text search in a MongoDB collection:**

```javascript
MyModel.find({ $text: { $search: 'searchKeyword' } })
  .then(docs => console.log('Matching documents:', docs))
  .catch(err => console.error('Text search error:', err));
```

Explanation:
- Use `MyModel.find()` with `$text` operator to perform text search.
- Provide the search keyword within `$search`.
- Handle the matched documents or any errors.

9. **Node.js API endpoint to filter documents based on specific criteria in a MongoDB collection:**

```javascript
app.get('/api/documents/filter', (req, res) => {
  const { criteria } = req.query;
  const filter = JSON.parse(criteria);
  
  MyModel.find(filter)
    .then(docs => res.json(docs))
    .catch(err => res.status(500).json({ error: err.message }));
});
```

Explanation:
- Define a route `/api/documents/filter` to handle filtered queries.
- Extract the filter criteria from the query parameters.
- Parse the criteria into a JavaScript object.
- Use `MyModel.find()` with the provided filter to fetch matching documents.
- Respond with the matched documents or handle errors.

10. **Node.js function to perform sorting on documents in a MongoDB collection:**

```javascript
MyModel.find().sort({ age: 1 })
  .then(docs => console.log('Sorted documents:', docs))
  .catch(err => console.error('Sorting error:', err));
```

Explanation:
- Use `MyModel.find().sort()` to perform sorting on documents.
- Pass an object specifying the field to sort by and the order (`1` for ascending, `-1` for descending).
- Handle the sorted documents or any errors.


11. **Node.js script to create indexes in a MongoDB collection:**

```javascript
MyModel.createIndexes()
  .then(() => console.log('Indexes created successfully'))
  .catch(err => console.error('Error creating indexes:', err));
```

Explanation:
- Use `MyModel.createIndexes()` to create indexes defined in the schema.
- This function creates indexes defined in the schema using the index specifications.
- Handle success or error messages accordingly.

12. **Node.js API endpoint to handle file uploads and store them in MongoDB GridFS:**

```javascript
const multer = require('multer');
const GridFsStorage = require('multer-gridfs-storage');
const Grid = require('gridfs-stream');

const storage = new GridFsStorage({
  url: 'mongodb://localhost:27017/myDatabase',
  file: (req, file) => ({ filename: file.originalname })
});

const upload = multer({ storage });

app.post('/api/upload', upload.single('file'), (req, res) => {
  res.json({ file: req.file });
});
```

Explanation:
- Use `multer` and `multer-gridfs-storage` packages to handle file uploads and store them in MongoDB GridFS.
- Define storage configuration specifying the MongoDB connection URL and how files should be stored.
- Use `multer({ storage })` to configure the upload middleware.
- Create a route to handle file uploads, utilizing the configured upload middleware.
- Respond with JSON containing information about the uploaded file.


13. **Node.js function to perform data validation before inserting into MongoDB:**

```javascript
const Joi = require('joi'); // Import Joi for validation

function validateData(data) {
  const schema = Joi.object({
    name: Joi.string().required(),
    age: Joi.number().min(18).required()
  });
  
  return schema.validate(data);
}

// Example usage:
const data = { name: 'John', age: 25 };
const { error, value } = validateData(data);

if (error) {
  console.error('Validation error:', error.details[0].message);
} else {
  console.log('Valid data:', value);
}
```

Explanation:
- Use the `Joi` library for data validation.
- Define a schema using `Joi.object()` with validation rules for each field.
- Use `schema.validate(data)` to validate the data against the schema.
- Check for errors and handle them accordingly.

14. **Node.js script to perform bulk insert operations in MongoDB:**

```javascript
const newData = [
  { name: 'Alice', age: 30 },
  { name: 'Bob', age: 35 },
  { name: 'Charlie', age: 40 }
];

MyModel.insertMany(newData)
  .then(docs => console.log('Documents inserted:', docs))
  .catch(err => console.error('Error inserting documents:', err));
```

Explanation:
- Use `MyModel.insertMany()` to insert an array of documents into the collection.
- Provide an array of documents to be inserted.
- Handle success or error messages accordingly.


15. **Node.js API endpoint to handle pagination of documents in a MongoDB collection:**

```javascript
app.get('/api/documents', async (req, res) => {
  const page = parseInt(req.query.page) || 1;
  const limit = parseInt(req.query.limit) || 10;

  try {
    const count = await MyModel.countDocuments();
    const totalPages = Math.ceil(count / limit);
    const skip = (page - 1) * limit;

    const docs = await MyModel.find().skip(skip).limit(limit);

    res.json({
      totalDocuments: count,
      totalPages,
      currentPage: page,
      documents: docs
    });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});
```

Explanation:
- Define a route `/api/documents` to handle pagination requests.
- Parse `page` and `limit` query parameters from the request URL, with default values if not provided.
- Use `MyModel.countDocuments()` to count the total number of documents in the collection.
- Calculate the total number of pages based on the total count and the limit.
- Calculate the number of documents to skip based on the page and limit.
- Fetch documents using `MyModel.find().skip().limit()`.
- Respond with JSON containing pagination information and the fetched documents.

16. **Node.js function to perform database transactions in MongoDB:**

```javascript
const session = await mongoose.startSession();
session.startTransaction();

try {
  await operation1();
  await operation2();
  
  await session.commitTransaction();
  session.endSession();
  console.log('Transaction committed successfully');
} catch (error) {
  await session.abortTransaction();
  session.endSession();
  console.error('Transaction aborted:', error);
}
```

Explanation:
- Use `mongoose.startSession()` to create a new session.
- Start a transaction using `session.startTransaction()`.
- Perform database operations within the try block.
- If all operations succeed, commit the transaction with `session.commitTransaction()`.
- If an error occurs, abort the transaction with `session.abortTransaction()`.
- End the session after completing the transaction.


17. **Node.js script to seed initial data into a MongoDB collection:**

```javascript
const initialData = [
  { name: 'John', age: 30 },
  { name: 'Alice', age: 25 },
  { name: 'Bob', age: 35 }
];

MyModel.insertMany(initialData)
  .then(docs => console.log('Initial data seeded:', docs))
  .catch(err => console.error('Error seeding initial data:', err));
```

Explanation:
- Define an array containing initial data to be seeded into the MongoDB collection.
- Use `MyModel.insertMany()` to insert the initial data into the collection.
- Handle success or error messages accordingly.

18. **Node.js API endpoint to handle user authentication using JWT and store user data in MongoDB:**

```javascript
const jwt = require('jsonwebtoken');
const bcrypt = require('bcrypt');

const secretKey = 'yourSecretKey';

app.post('/api/auth/login', async (req, res) => {
  const { username, password } = req.body;

  try {
    const user = await UserModel.findOne({ username });

    if (!user) {
      return res.status(401).json({ error: 'Invalid username or password' });
    }

    const validPassword = await bcrypt.compare(password, user.password);

    if (!validPassword) {
      return res.status(401).json({ error: 'Invalid username or password' });
    }

    const token = jwt.sign({ userId: user._id }, secretKey);
    res.json({ token });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// Middleware to verify JWT token
function verifyToken(req, res, next) {
  const token = req.headers.authorization;

  if (!token) {
    return res.status(401).json({ error: 'Unauthorized: Token missing' });
  }

  jwt.verify(token, secretKey, (err, decoded) => {
    if (err) {
      return res.status(401).json({ error: 'Unauthorized: Invalid token' });
    }
    req.userId = decoded.userId;
    next();
  });
}

app.get('/api/user/profile', verifyToken, async (req, res) => {
  try {
    const user = await UserModel.findById(req.userId).select('-password');
    res.json(user);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});
```

Explanation:
- Use `jsonwebtoken` for generating and verifying JSON Web Tokens (JWT).
- Use `bcrypt` for password hashing and comparison.
- Create an API endpoint `/api/auth/login` for user authentication.
  - Find the user by username in the MongoDB collection.
  - Compare the provided password with the hashed password stored in the database.
  - If authentication succeeds, generate a JWT and send it in the response.
- Implement middleware `verifyToken` to verify the JWT token in the request headers.
- Create an API endpoint `/api/user/profile` for fetching user profile data, protected by the `verifyToken` middleware.
  - Extract the user ID from the decoded token and fetch the user data from the MongoDB collection.
- Handle authentication errors and responses accordingly.


19. **Node.js function to handle error logging in MongoDB:**

```javascript
const ErrorLogModel = require('./models/ErrorLog'); // Import your error log model

async function logError(errorMessage, errorStack) {
  try {
    const errorLog = new ErrorLogModel({
      message: errorMessage,
      stack: errorStack
    });
    await errorLog.save();
    console.log('Error logged successfully');
  } catch (error) {
    console.error('Error logging:', error);
  }
}
```

Explanation:
- Define a function `logError` to log errors into the MongoDB collection.
- Create a new instance of the error log model with the error message and stack trace.
- Use `save()` to save the error log document into the collection.
- Handle any errors that occur during the logging process.

20. **Node.js script to perform backup and restore operations for a MongoDB database:**

```javascript
const fs = require('fs');
const { exec } = require('child_process');

// Backup
const backupFileName = 'backup.zip';
const backupCommand = `mongodump --archive=${backupFileName} --gzip`;

exec(backupCommand, (error, stdout, stderr) => {
  if (error) {
    console.error('Backup error:', error);
    return;
  }
  console.log('Backup successful');
});

// Restore
const restoreCommand = `mongorestore --gzip --archive=${backupFileName}`;

exec(restoreCommand, (error, stdout, stderr) => {
  if (error) {
    console.error('Restore error:', error);
    return;
  }
  console.log('Restore successful');
});
```

Explanation:
- Use `mongodump` command to create a backup of the MongoDB database into a compressed archive file.
- Use `mongorestore` command to restore the database from the backup archive.
- Execute the commands using `child_process.exec()` from Node.js.
- Handle errors and success messages accordingly.

These scripts and functions provide various functionalities to interact with MongoDB using Node.js, including CRUD operations, authentication, data validation, backup, and restore operations. Make sure to adapt these examples to your specific use case and environment.
