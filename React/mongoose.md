Everything in Mongoose starts with a Schema. Each schema maps to a MongoDB collection and defines the shape of the documents within that collection.

    import mongoose from 'mongoose';
    const { Schema } = mongoose;

    const blogSchema = new Schema({
        title:  String, // String is shorthand for {type: String}
        author: String,
        body:   String,
        comments: [{ body: String, date: Date }],
        date: { type: Date, default: Date.now },
        hidden: Boolean,
        meta: {
        votes: Number,
        favs:  Number
        }
    });

Notice above that if a property only requires a type, it can be specified using a shorthand notation (contrast the title property above with the date property).

The permitted SchemaTypes are:

- String
- Number
- Date
- Buffer
- Boolean
- Mixed
- ObjectId
- Array
- Decimal128
- Map

Schemas not only define the structure of your document and casting of properties, they also define document instance methods, static Model methods, compound indexes, and document lifecycle hooks called middleware.


### Creating a model

To use our schema definition, we need to convert our blogSchema into a Model we can work with. To do so, we pass it into mongoose.model(modelName, schema):

    const Blog = mongoose.model('Blog', blogSchema);
    // ready to go!

### Ids

By default, Mongoose adds an _id property to your schemas.

    const schema = new Schema();
    schema.path('_id'); // ObjectId { ... }

When you create a new document with the automatically added _id property, Mongoose creates a new _id of type ObjectId to your document.

    const Model = mongoose.model('Test', schema);
    const doc = new Model();
    doc._id instanceof mongoose.Types.ObjectId; // true

You can also overwrite Mongoose's default _id with your own _id. Just be careful: Mongoose will refuse to save a document that doesn't have an _id, so you're responsible for setting _id if you define your own _id path.

    const schema = new Schema({ _id: Number });
    const Model = mongoose.model('Test', schema);

    const doc = new Model();
    await doc.save(); // Throws "document must have an _id before saving"

    doc._id = 1;
    await doc.save(); // works

### Instance methods

Instances of Models are documents. Documents have many of their own built-in instance methods. We may also define our own custom document instance methods.

    // define a schema
    const animalSchema = new Schema({ name: String, type: String });

    // assign a function to the "methods" object of our animalSchema
    animalSchema.methods.findSimilarTypes = function(cb) {
        return mongoose.model('Animal').find({ type: this.type }, cb);
    };

- Overwriting a default mongoose document method may lead to unpredictable results. See this for more details.
- The example above uses the Schema.methods object directly to save an instance method. You can also use the Schema.method() helper as described here.
- Do not declare methods using ES6 arrow functions (=>). Arrow functions explicitly prevent binding this, so your method will not have access to the document and the above examples will not work.

### Statics

You can also add static functions to your model. There are two equivalent ways to add a static:

Add a function property to `schema.statics`
Call the `Schema#static()` function

    // Assign a function to the "statics" object of our animalSchema
    animalSchema.statics.findByName = function(name) {
        return this.find({ name: new RegExp(name, 'i') });
    };
    // Or, equivalently, you can call `animalSchema.static()`.
    animalSchema.static('findByBreed', function(breed) { return this.find({ breed }); });

    const Animal = mongoose.model('Animal', animalSchema);
    let animals = await Animal.findByName('fido');
    animals = animals.concat(await Animal.findByBreed('Poodle'));

