const {MongoClient} = require('mongodb'); //connect to mongodb

async function main() {
    const uri = "mongodb://localhost:27017"; //connects to the db
    
    const client= new MongoClient(uri);

    try{ 
     await client.connect();

//   await listDatabases(client);
    //calls the create method
    await createListing(client,
        {
            //input 
            Title: "Dr",
            Firstname: "Joshua",
            Surname: "Michael",
            Mobile: "0872018989",
            email: "joshthedr@outlook.com",
            AddressLine1: "Josh's house",
            AddressLine2: "on Josh's street",
            County: "Galway",
            Eircode: "wfwfhwfofw",
           
        },
      
    );
    

    //calls the read method to match given 'firstname'
    await readName(client, "Joshua");
    //calls the update method
    await updateCustomers(client, "Joshua", { firstName: "mark",email: "newmark@gmail.com",Title: "Mr" });
    //calls delete method
    await deleteName(client, "Joshua");
    }catch (e) {
        console.error(e);
    }
    finally{ 
        await client.close();
        }
 }



main().catch(console.error);
/*
async function listDatabases(client){
   const databasesList=await client.db().admin().listDatabases();
   
   console.log("Database:");
   databasesList.databases.forEach(db => console.log(` - ${db.name}`));
 Shows i successfully connected to db
}
*/

async function createListing(client, newListing) {
    const result=await client.db("Assigment5").collection("Customers").insertOne(newListing);
    //creates a new collection with newListing as the input parameter for the data
    console.log(`New listing created with the following id: ${result.insertedId}`);
    //prints the id of the newly created collection to the terminal
}

//read function
async function readName(client, nameOfListing) {
    const result = await client.db("Assigment5").collection("Customers").findOne({ Firstnameame:nameOfListing  });
    //sets result to the collection with the matching parameter Firstname
    if (result) {
        //if a match is found the result is printed to the terminal 
        console.log(`Found a listing in the collection with the name '${nameOfListing}':`);
        console.log(result);
    } else {
        console.log(`No listings found with the name '${nameOfListing}'`);
    }
}
//update function
async function updateCustomers(client, name1, name2) {
    const result = await client.db("Assigment5").collection("Customers").updateOne({ name: name1 }, { $set: name2 });

    console.log(`${result.matchedCount} document(s) matched the query criteria.`);
    console.log(`${result.modifiedCount} document(s) was/were updated.`);
}

//delete function
async function deleteName(client, name) {
    //sets result to document matching perimeter firstName
    const result = await client.db("Assigment5").collection("Customers").deleteOne({ Firtname: name });
    console.log(`${result.deletedCount} document(s) was/were deleted.`);
    //prints to terminal if successful
}

/*references
 ->https://www.youtube.com/watch?v=fbYExfeFsI0
 ->https://www.mongodb.com/docs/manual/indexes/?utm_source=compass&utm_medium=product#single-field

 */
