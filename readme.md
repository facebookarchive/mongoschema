mongoschema
===========

`mongoschema` is a tool that runs over a Collection in a Mongo DB, and
generates a struct with appropriate types and bson tags.

  go get github.com/facebookgo/mongoschema

For example, lets make a collection with some data:

    # mongo
    MongoDB shell version: 2.4.8
    connecting to: test
    > db.company.insert({name:"Facebook", address:{street_1:"1 Hacker Way", city:"Menlo Park"}, jobs_url:"https://www.facebook.com/careers"})
    > db.company.insert({name:"Parse", address:{street_1:"1 Hacker Way", city:"Menlo Park"}, jobs_url:"https://parse.com/jobs"})
    > 
    bye

And now we can run our tool against this collection:

    # mongoschema -url=localhost -db=test -collection=company -package=main -struct=Company
    package main

    type Company struct {
      ID      bson.ObjectId `bson:"_id,omitempty"`
      Name    string        `bson:"name,omitempty"`
      Address struct {
        Street1 string `bson:"street_1,omitempty"`
        City    string `bson:"city,omitempty"`
      } `bson:"address,omitempty"`
      JobsURL string `bson:"jobs_url,omitempty"`
    }
