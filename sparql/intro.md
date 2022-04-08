# Intro to working with Stardog and SPARQL

In this article we go over how to work with Stardog and SPARQL.

## Launch Stardog Environment

Register for a free license at https://www.stardog.com/get-started/

Create a directory called C:\Stardog

Monitor your inbox for an email with the license key and download the file into C:\Stardog

Launch a terminal and change into the `stardog-workshop` directory

Launch a local Stardog Environment from the terminal using the following command
```
docker compose up
```

Launch a browser and navigate to http://localhost:8085 to access Stardog Studio

On the `Connect to Stardog` Dialog, Connect to the Stardog Server by entering the following info:
- Username = admin
- Password = admin
- Endpoint = http://localhost:5820
- Add to My Connections = True and Name = Local

## Create a database

On the Left Navbar, look for the `Databases` tab and click on it

On the Side Panel, look for the `Create Database` button and click on it

On the `Create New Database` Dialog, enter the following info:
- Database Name = Intro
- Configuration = Use Defaults

On the Left Navbar, look for the `Workspace` tab and click on it

On the Main Panel/Query Editor, click on the `Select Database` Dropdown and select the `Intro` Database

## Add data to the database

Let's add some data to our database and get some practice authoring RDF triples/statements in the process. 

Make sure you are within the Query Editor

The most basic way of adding data to a database is using the `INSERT DATA ...` update

A basic `INSERT DATA ...` update looks like this
```
INSERT DATA { triples... }
```

For example, consider the following concepts:
- Cade is an American
- Cade is 27 years old
- Cade's favorite foods are Pizza and Burgers

If I wanted to add that knowledge to the default graph in the database, I might write something like this
```
INSERT DATA {
  :American rdf:type rdfs:Class .
  :ageIs rdf:type rdf:Property .
  :favoriteFoodIs rdf:type rdf:Property .

  :Cade 
    rdf:type :American ;
    :ageIs "27" ;
    :favoriteFoodIs :Pizza, :Burger .
}
```

Now it's your turn, try to define RDF triples/statements in SPARQL that express each of the following concepts:

- The capital of France is Paris
- Paris is a City in France
- 26 years is the age of Mary
- Mary's interests include hiking, chocolate, and biology
- Mary is a student
- Cade and Mary are kind people
- Cade is married to Mary
- Mary lives in Paris
- Cade lives with Mary

## Query data from the database

Now that we have some data in our database, let's see what kind of queries we can write.

The most basic way of querying data in a database is by using the `SELECT ... WHERE ...` query.

A basic `SELECT ... WHERE` query looks like this
```
SELECT expressions_and_variables...
WHERE {
  patterns...
}
```

For example, if I wanted to find out all the triples in the default graph, I might write something like this:
```
SELECT ?subject ?predicate ?object
WHERE {
  ?subject ?predicate ?object .
}
```

And if I wanted to find out what city is the capital of France, I might write something like this:
```
SELECT ?capital
WHERE {
  :France :capitalIs ?capital .
}
```

Or if I wanted to find out what resources are cities, I might write something like this:
```
SELECT ?city
WHERE {
  ?city rdf:type :City .
}
```

Pay attention to how certain words are prepended with `?`. Those are the variables, and their names don't matter. You can use whatever name is meaningful to you, though it is best practice to use a name that represents the information you are trying to extract. The only thing that really matters is the position. If a resource or variable appear in the first position, then it will match the subject. If a resource or variable appear in the second position, then it will match the predicate. Etc. etc. etc.

Now, try to write queries to answer the following questions:
- How old is Mary?
- How old is Cade?
- Who is married to Mary?
- Who is married to Cade?
- Who is interested in hiking and biology?
- Who is married to someone who is interested in chocolate?
- Who is married to someone who's favorite food is Pizza?
- Who is kind?
- Who is a kind person?
- Who is a kind student?
- Who lives in Paris?

## Remove data from the database

The most basic way of removing data is by using the `DELETE ... WHERE ...`  update.

A basic `DELETE ... WHERE ...` to remove all triples from the default graph would look something like this:
```
DELETE ?subject ?predicate ?object
WHERE {
  ?subject ?predicate ?object
}
```

If you wanted to remove just triples where the subject is the resource `:Cade`, then you might write something like this:
```
DELETE { :Cade ?predicate ?object }
WHERE {
  :Cade ?predicate ?object
}
```

## Change data in the store

Now that you have an idea of how to insert and delete data, how about updating or changing data? The topic of changing data is perhaps one of the turning points when learning about the RDF model compared to other data management models. The question then is this, "Can you actually mutate data in a graph"? or "What does it mean to mutate a graph"? Obviously there are semantic games we can play about what does it mean to mutate data, etc. Strictly speaking though, in RDF we don't mutate or change data per se, rather, we add triples and remove triples in order to represent changes in knowledge.

For example, let's say that Cade and Mary move to England. What should happen to the triple that states that Mary lives in France? Is it as simple as updating the object of that triple to England, or is there more to it? Well, the first thing to note is that there is no `UPDATE` operation in SPARQL, you have to model all changes to the graph through a combination of DELETE and INSERT updates. For example, to model the move of Mary to England, we might do something like:
```
DELETE { :Mary :livesIn :France }
INSERT { :Mary :livesIn :England }
WHERE { :Mary :livesIn :France }
```

Now, let's try to figure out together how we might deal with a situation where Cade had to stay behind in Paris...

## Conclusion

In this lab we have learned the basics of working with Stardog and SPARQL. To continue your journey, venture to explore one of the following tutorials:

Intro to working with DotNet and Stardog

Intro to working with Python and Stardog

Intro to full stack semantic development with Angular, DotNet and Stardog

Intro to full stack semantic development with Angular, Python and Stardog