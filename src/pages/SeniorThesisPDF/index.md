---
title: Honey Encryption Python implementation
date: "2019-05-11"
featuredImage: './featured.jpg'
---

```
Technical Report CS2019-
```
```
theRightFit:
A scalable web-framework for land,
sea, and air quote estimating for
Freight Forwarders
```
```
Karol Vargas
```
```
Submitted to the Faculty of
The Department of Computer Science
```
```
Project Director: Dr. Aravind Mohan
Second Reader: Dr. Janyl Jumadinova
```
```
Allegheny College
2019
```
I hereby recognize and pledge to fulfill my
responsibilities as defined in the Honor Code, and
to maintain the integrity of both myself and the
college community as a whole.

```
Karol Vargas
```

```
ii
```
Copyright©c 2019

```
Karol Vargas
```
All rights reserved


```
iii
```
```
KAROL VARGAS. theRightFit:
A scalable web-framework for land, sea, and air quote estimating for
Freight Forwarders.
(Under the direction of Dr. Aravind Mohan.)
```
##### ABSTRACT

What I am planning to do for my Senior thesis is to create a scalable web-
framework that would allow freight forward companies to give instant quotes online
to customers. The languages I am using are to accomplish this is MongoDB(NoSQL),
MySQL, JavaScript(Express, Mocha, Node), and HTML. This has been done before
by other companies in the past but what seperates this application is its ability to
display prices to freight forwarders specifically. This application uses an algorithm
which will display rates for land, sea, and air freight for what the customer will pay.
In addition to that, and urgency of delivery. This application would also be able
to identify the class type for land freights which can provide a much more accurate
quote versus going by the density. I will create an algorithm in the application that
would take the dimensions of the cargo and see in what type of container it can fit
in. There are many different sized containers in the logistics world so not only would
this be helpful to the customer but also a help to the company to save time. The
application would then take in using geoLocation library in Google maps APIs the
current terminal location and the destination terminal the user enters. Once it knows
what types of freight are possible it will make a call to the MYSQL database for
the table of class types for density measurements. This would also hold a table for
different tariff rates for different countries.


```
iv
```
I dedicate this to my family for always supporting me through all my challenges
and for giving me the confidence to accomplish anything I set my mind to. It has
never been easy for me wherevever I have gone but I am always up for any challenge
knowing I have the greatest support in the world.
I thank youRuddy Sr., Karinna Arias, Jorge, Leila, Alexis, Pamela,
Ruddito, Didier, Leah, Brandon, Dhalig, and Ashley.


## v

# Contents


- 1 Introduction List of Figures vii
   - 1.1 Motivation
      - 1.1.1 The Process
      - 1.1.2 And in with the New
   - 1.2 an Algorithmic Approach
   - 1.3 Challenges
   - 1.4 Goals of the project
- 2 Related Work
   - 2.1 Client and Server security and data processing
   - 2.2 Method for Retaining JavaScript Data descriptions and Values
   - 2.3 Verifying Client-Side Input Validation
   - 2.4 geoLocation or Google Maps API?
- 3 Method of Approach
      - 3.0.1 Node.js vs React.js/Native.js
      - 3.0.2 ExpressJS: Back-End to Front-End
      - 3.0.3 RESTful API Shippos
      - 3.0.4 Geocodes implemented into Shippos RestAPI
   - 3.1 NoSQL/MongoDB vs MySQL
   - 3.2 Test Environment
   - 3.3 Experiments
   - 3.4 Threats to Validity
- 4 Implementation
   - 4.1 JavaScript code to Create server connection to NoSQL/SQL Database
      - 4.1.1 First Steps to Success
      - 4.1.2 backend to frontend: WARNING! useful content ahead
      - 4.1.3 Why don’t you just meet me in the middle(ware)
      - 4.1.4 json
      - 4.1.5 innerHTML forms
      - 4.1.6 testing
      - 4.1.7 Gators Win
   - 4.2 vi
   - 4.3 Creating MySQL table for quering into database
- 5 Discussion and Future Work
   - 5.1 Future Work
- A Java Code


```
vii
```
# List of Figures

```
1.1 Detailed workflow of a Cargo Corporation............... 1
1.2 Increasing demand for Freight Forwarders as reported by US Depart-
ment of Transportation.......................... 2
1.3 Current Workflow for LSP Cargo Incorporated............. 4
1.4 Projected Workflow for LSP Cargo Incorporated............ 5
1.5 FedEx Shipping calculator........................ 6
1.6 example of Javascripts geolocation library............... 7
1.7 example of a D-Container used at LSP Cargo............. 8
```
```
2.1 client server application example.................... 9
2.2 ”Example of a database in MYSQL which stores different datatype-
s/citeVeikkolainen”............................ 10
2.3 ”Example of the method to retain formal data model descriptions be-
tween server side and browser side objects”............... 12
2.4 ”Example of the method to retain formal data model descriptions be-
tween server side and browser side objects”............... 13
2.5 ”Google Maps location flowchart”.................... 14
```
```
3.1 NodeJS Server demonstrating Client-Server requests.......... 16
```
```
4.1 Hosting local web server on port 3000.................. 30
4.2 Hosting local web server on port 3000.................. 31
```

# Chapter 1

# Introduction

### 1.1 Motivation

The motivation behind this project is to create a new logarithm for calculating Land,
Sea, and Air freights in supply chain companies. Freight Forwarding is a branch in
the Supply Chain industry which has been greatly impacted by advances in tech-
nology. We see this from Amazon using KIVA system robots, to other companies
using shipping updates on packages ordered from their website. LSP Cargo Inc. is
a company which has been involved with international shipping and logistics for 20
years.
This is a company which has made little to no technological advancements in its
history as an NVOCC. This gives me a great opportunity to apply my knowledge of
cargo and of software to create an application which can make a huge impact. The
way logistics works is simple but a little complicated too. I will first go into detail
on the life cycle of an order in LSP(Logistic Service Providers) Cargo Inc. and how
it differs from one of the bigger shipping companies such as FedEx or USPS.

```
Figure 1.1: Detailed workflow of a Cargo Corporation
```

#### 1.1.1 The Process

The very first step in the process of freight forwaring is to place an order. What
seperates LSP Cargo and why people tend to go to freight forwarders is for businesses
which do not have the supply to fill an entire container to ship overseas or to send
across the country. These smaller suppliers will instead contract a freight forwarder
such as LSP Cargo Inc. for their logistic needs. This could include courier pick up, all
types of freight forwarding, as well as facilitating customs interaction when importing
or exporting internationally.
Another advantage of LSP Cargo and why this industry is in such high demand
is because LSP Cargo is a Non Vessel Owning Common Carrier or more commonly
known as an NVOCC. This type of carrier/freight forwarder is one which does not own
any vessels or actual means to ship something. Instead, NVOCC’s have the ability to
contract bigger carriers at more convenient times for big suppliers and more realistic
prices to smaller suppliers thanks to licensing. Lets say hypothetically I want to ship
600 boxes of shoes from Miami to the Dominican Republic.

Figure 1.2: Increasing demand for Freight Forwarders as reported by US Department
of Transportation

The first step if I was placing this order with LSP Cargo would be to either make
a phone call to a cargo agent or send an email directly to a cargo agent that I would
have worked with in the past. The next step would be to see is this package moving
internationally or domestically. Since in this case we can tell the package is moving
internationally we know for a fact that it would be impossible to ship this cargo
by land. Knowing that we cannot ship this by land makes the shipping calculation
much easier since we do not have to deal with classes, trust me i’ll explain later just
stick with me. Like I was saying, we are going to be dealing with either Sea Freight
Shipping or Air freight shipping. For shipping with Air Freight we know that this is
simply going to be a factor of what is the weight and what is the volume. For Air
Freight Quotes we can expect to multiply the volume by 167 meters to to get the
volumetric weight. Whichever is greater out of the volumetric weight or the actual


weight is what you will be charged on. For sea freight however, the way we calculate
that is whether or not the supplier will be shipping a Less than Container Load (LCL)
or a Full Container Load(FCL).
People who ship a lot of LCLs will probably get most of their business from
an NVOCC. The advantage of choosing an NVOCC or a Freight Forwarder is for
convenience and efficiency. Normally with larger carriers who own there own vessels,
they might give you a better price because there is no middle man but there is a
downside. Normally with carriers of large size such as Seaboard Marine or King
Ocean or Fedex they tend to go by a schedule. They will ship out all their sea freight
on a specific day every week. If I am a supplier and I wish to ship with Seaboard for
example I would have to be prepared to only ship my cargo on a specific day out of
the week. However, If I were to go to an NVOCC I would be able to ship any day that
is most convenient for my company and I would have the luxury of only having to
rent a space of the container, I would not need to get an FCL like most large carriers
would make you do.
The customer will first decide (Since trucking is impossible ) whether they want
to put this shipment on a shipping container to be taken by ocean freight, or to be
taken by plane which would be more expensive but faster. After the customer has
decided how they want to have their goods shipped, LSP processes the order and sets
up the reservation with the sea freight or air freight company. After the appointment
has been set, the trucking company is called by LSP to pick up the package to take to
whatever form of shipment the customer chose. Then finally, it is delivered to another
warehouse where the customer has the option to have however many boxes of shoes
delivered to however many places he wants. This is the work flow of LSP Cargo and
has been that way since 2006. Below is the current workflow of LSP Cargo.


```
Figure 1.3: Current Workflow for LSP Cargo Incorporated
```
#### 1.1.2 And in with the New

This business model which has been so successful for so long is aching to have more
software implementation. This is a world where the processing power of computers
goes up every 6 months. This project is going to allow for two large changes within
the company. The ability to begin to implement software in a historically successful
company, and allow for marketing to new audiences with an easy to use interface in
the future. Below would be the workflow of the program I would make and how much
less strain it would put on employees at LSP Cargo and how it can open the door for
new customers.


```
Figure 1.4: Projected Workflow for LSP Cargo Incorporated
```
How this workflow differs from larger companies such as FedEx or UPS is through
human interference and the opportunity for human error. Larger cargo companies
or companies that are greatly involved in shipping leave little to no tasks to humans
and automate as much as possible. This allows them to advertise their business to a
much wider audience.


```
Figure 1.5: FedEx Shipping calculator
```
LSP tends to deal with much larger shipments and for this reason it brings in a
few big fish but misses out on everything in between the smallest shipment to the
largest because of its inability to market their services to small time shipments. With
this projected workflow we see a lot of automation taking place rather than a cargo
agent having to make the booking themselves and read in the order.

### 1.2 an Algorithmic Approach

The way this project takes an algorithmic approach to rate estimating and container
estimating is through an algorithm which will be explained in later chapters. This is
an algorithm which will take into account what type of shipping method is possible
between location A and B using the geolocation functions in Javascript.


```
Figure 1.6: example of Javascripts geolocation library
```
This function would then turn the given locations into longitude and latitude
points. There are numerous libraries online to find if one is on an island or a continent
or if it is possible to travel to one place by air land or sea. This would allow the
program to return only the functions that are relevant to the user in this case.
Using SQL one will be able to store all the data which we wish to mutate later
such as what are the current shipping rates from here to Bangkok? They could change
at the drop of a hat with new policy and this is something this project will be able
to accomplish in the future. Regardless, back to reality, the next step of the program
is to take in the dimensions of the package or packages being shipped. The program
is going to perform to functions with this data.
First thing the program will do is measure the volume of the package or packages.
Once the volume is obtained the code will then go into the SQL database which will
contain the database for all the rates of domestic and international shipping. After
it has obtained this data which will be collected from LSP Cargo first hand. The
program will then take in the weight of the package(s) and see what kind of container
it would be able to fit in or if it needs one at all. There are many different sized
containers ranging from D-G that decrease in size the higher you go in the alphabet.
The program will check from largest to smallest and see what is the container which
can hold the most of these dimensions while taking up the least amount of space.


```
Figure 1.7: example of a D-Container used at LSP Cargo
```
### 1.3 Challenges

One of the biggest challenges when faced with international shipping is surprise fees.
This is why normally express couriers who are often customs brokers who can process
the customs duties for a fee. This is a fee that is extremely difficult to predict as it
changes from country to country. There are also many other hidden fees when one
is faced with the challenge of shipping internationally such as redelivery fees. This
is why cargo is such a difficult industry to predict rates. This also goes the same for
flying goods internationally. When flying goods internationally it is often a different
rate depending on what kind of material is flying. It might cost one a different price
to fly 600 pairs of shoes than 600 pairs of shirts even if they occupy the same amount
of space and weigh the same. A great source to check out for materials which are not
allowed to ship or what materials or live goods one is allowed to ship under certain
guidlelines is the USPS shipping calculator.

### 1.4 Goals of the project

The goals of this project are to create a program which will implement automation in
LSP Cargo Inc. This will increase productivity from employees and give LSP Cargo
a competitive edge by being able to market automation. This is a project that will be
added as an application to LSP Cargos main page. This will give a great opportunity
to market to new customers. After being added as an extension of this website, it will
be extended to return real time international shipping rates and a way to update long
time customers with new shipping laws implemented in either their usual country of
origin or the country they ship to usually.
This can also be a way to create a brand which is recognizable as it has never been
done before.This would also be the first shipping calculator of its kind as it returns
information of the shipment to help with warehouse organization. This can be a way
in future works to hopefully return this data to a robot to directly package the box
once the order has been shipped. This project has the potential to change an industry
which had remained relatively the same until the introduction of automation.


# Chapter 2

# Related Work

### 2.1 Client and Server security and data processing

Being that this is a project with both client/server side programming this is where the
majority of the sources are from for this program. The first article we will observe is
”Executing SQL over encrypted data in the database-service-provider model”. This
paper speaks about how the emergence of software as a service has changed the way
we consume. It also goes into how the ”Database as a service” allows people to access
information around the globe.
This gives us a glimpse as to what the program falls under when we talk about it
as a service in one way or another. ”theRightFit” is a way for consumers who wish
to use shipping services for any size quantity to easily be quoted by using a software
and a database as a service. While there is a lot of data that means that there needs
to be even more security to protect that data. ”There are two main privacy issues.
First, the owner of the data needs to be assured that the data stored on the service-
provider site is protected against data thefts from outsiders. Second, data needs to
be protected even from the service providers, if the providers themselves cannot be
trusted[?].”

```
Figure 2.1: client server application example
```
```
This paper goes over exploring a large variety of techniques to execute SQL queries
```

over encrypted data. The main strategy which is brought up in this paper the most
or seen as the optimal technique is to process as much of the query as one can at
the service providers site. The paper also provides an algebraic framework to split
the query to minimize the computation that is done at the client site. The next
paper to observe is ”PALEOMAGIA: A PHP/MYSQL database of the Precambrian
paleomagnetic data[?]”. This is a paper whose topic is not really relevant but the
method in which the data is stored is very relevant. These data require a precise,
rationally organized database.
This topic requires storing data from the database geographically. One can exploit
these methods by storing geographical rating data into the SQL server. This data
would have to be part of the data protected like it says in the paragraph above. A
way that we can relate this to the geolocation function we plan on using in Javascript
is through the database table used in this paper to store paleomagnetic data. In
this data base one of the data types is a variable type varchar in MYSQL which is a
country. This would be a way that we could see if the Country is the same or if its
different.
If the country is different then we know that we need to access the international
shipping rate from the data table. Then using that exact same datatype to see if
the country is the same. We can also see what type of shipping methods would be
possible from these two locations.

Figure 2.2: ”Example of a database in MYSQL which stores different datatype-
s/citeVeikkolainen”.

So if the shipping is domestic then we can automatically tell the program that
the only two options are Trucking or Air Freight. Trucking once again is using a big
truck to be the primary form of transportation. Air Freight requires a truck from the
warehouse with the goods to the airport, an air bus which is another way to call just a
really big 747 airplane which is used for cargo, then another truck to the destination.


If we return that the country is different it gets a little bit more complicated. This is
because that if we were to return that the country is different, all three are possible.
But there are also other countries where the rules are the same as domestic besides
the shipping rates. A great example of this would be Canada and Mexico. To ship
to Canada and Mexico you can truck there technically but it also true that to some
locations you can ship it by sea as well. For these types of countries we will have
to use the technique of creating a column of column of columns within a dataset
table in MYSQL. The first two columns would be if the countries are international
or domestic to the United States. The next column would be if they can be shipped
by sea from the United States. Finally the last column would be what is the rate of
the country.

### 2.2 Method for Retaining JavaScript Data descrip-

### tions and Values

The next paper to be observed would be ”Method METHOD AND SYSTEM FOR
RETAINING FORMAL, DATA MODEL, DESCRIPTIONS BETWEEN SERVER-
SIDE AND BROWSER-SIDE JAVASCRIPT OBJECTS[?]”. This is a US Patent on
a method to basically keep the same data values from one programming language
and using cloud computing take those same data values and use another language
with them. This method is done using an EClassifier which represents a class in the
software model. The two Eclassifiers we would represent same as in the figure below.
There would be a User EClassifier as well as a Server/Portfolio EClassifier. This is
an algorithm which basically iterates through each property in a selected EClassifier.
And if the property in the EClassifier is an attribute, then it will output the Javascript
code with that value of the attribute. This could be useful when the user enters the
data in our algorithm for the dimensions of the package they wish to ship and to then
return that value to LSP Cargos PHP code to determine what is the correct container
to ship this box in. T
his algorithm can be used also when the user enters their two locations to return
the correct shipping rate for that country. The database would be in the backend
and once the user enters their two locations in the value of javascript geolocation.
The algorithm will then transfer this data to PHP to allow for the database with the
international shipping rates to be encrypted and then to use the same algorithm to
return the results to the user in Javascript.


Figure 2.3: ”Example of the method to retain formal data model descriptions between
server side and browser side objects”.

This would be a way that we could ensure the safety of the international shipping
rates and to keep the data as clean without having to create new descriptions or
descriptors between objects.

### 2.3 Verifying Client-Side Input Validation

Our next source is going to be ”Verifying client-side input validation functions using
string analysis[?]”. This paper has to do with verifying user input so as to ensure
security and performance are not at optimal levels when the user enters to use the
program. An example of this would be that you can return that if a field is empty
return an error but that would also have to take in that a row of spaces should also
count as nothing. It talks about what the very first step is in form validation, which
is to have an event handler.
An event handler in javaScript is what the user is going to do that is going to
trigger the validation of the input they have or have not entered. This could be an
onClick or an onSubmit but something along those lines. Once the user has inputed
their values we do not want the program to run slower than it needs to because it
has to ask the web for a task that we can validate before asking the web for our info.
This will allow for the user to get a better feel for the program first and foremost and
this would be something that can be used much faster if you arent sure how to use it
rather than giving up on it because you didn’t understand it because it was too hard.


Figure 2.4: ”Example of the method to retain formal data model descriptions between
server side and browser side objects”.

### 2.4 geoLocation or Google Maps API?

One of the more important sources in our research is ”Online Map Application De-
velopment Using Google Maps API, SQL Database, and ASP.NET[?]” This journal
talks about how there is an increase in demand for people to create map applications
using google maps API. This API came out when google launched The Google Maps
in 2005[?]. This journal goes in depth on using .ASP which is a good language to use
with SQL to analyze data from the dataset and put that into javascript. This type
of method and using this library will give the program a lot more versatility when it
comes to creating something that would work cleanly.
When seeing how the data can pull data with an extensive amount of data attached
to it. This might be a better method to begin to pull the data of rate shipping after
using the two algorithms mentioned above so that we can use those algorithms with
this source code so we can attach more variables and more data to one location than
previously. This will allow us to implement more accurate rate shipping and return
the values at a faster rate if its possible to attach so many variables to one column
in a dataset.


```
Figure 2.5: ”Google Maps location flowchart”.
```
The rest of the data I will be gathering will be first hand research from LSP Cargo.
The data will all be recovered by grabbing the research through previous orders for
countries which we do not ship to as popularly or through whatever database LSP
Cargo uses to ensure that all of their data is correct. After gathering all the data the
next step would be to put this data into a SQL database organized correctly to allow
for proper analysis. This is going to return the most accurate tax, import, and export
rates as well as what size the package is using the logarithms we displayed above.
The sources we have researched and continue to use are listed in the .bib file
attached but these were the sources specifically which required more explanation as
they are one hundred percent essential in accomplishing the goals of the project.
These papers and journals will provide the building blocks to the finished piece.


# Chapter 3

# Method of Approach

The method of Approach that we want to take to accomplish this task is to first
identify classes for Land Freights. This can prove to be the most complicated facet of
rate calculating and estimating since there are so many exceptions to why something
is a particular class. There are 18 different classes and as classes are higher in density
the lower their rate is per pound. This form of rate calculating is the main reason
why we want to store our database in the cloud. This will allow us in the future to
be able to expand and grow our database and hopefully in the future add real time
currency rates and tax rates for land and sea freighting to have the most accurate
quote calculator.

#### 3.0.1 Node.js vs React.js/Native.js

Nodejs is going to be the main programming language I will use for my project. This
language is going to allow for me to connect to my database and allow for client server
connection. This process is known as multithreading, hence the name Node. Tradi-
tional programming normally cannot continue processing until an operation finishes.
What a thread is able to do is share memory with every other thread that is within
the same process. The way we are planning on executing all of our threads And which
threads to specify for a given event is going to be using the Event-Driven program-
ming style. The program that we are going to be creating requires the user to enter
their products location, its final destination, what the dimensions of the product is,
as well as what type of class the product falls under.
Different classes will have descriptions on a separate HTML div. This is going to
increase customer awareness as well as to how much class factors into rate quoting
but this is only used for land freight. Anyway getting back to Node, talking about
the processes that this program will have to do it is clear that we will have to have
constant communication between the client-side and server-side of the application.
the way that we are going to be performing most of our queries to our MySQL server
is by using a variation of the code below:

```
query_finished = function(result) {
do_something_with(result);
}
```

```
query(’SELECT * FROM posts WHERE id = 1’, query_finished);
```
What is happening in this code above is that we are demonstrating what it is that
we are going to select from our dataset in MySQL. It demonstrates how NodeJS is
great for making asynchronous applications. This is because we can return a function
on an event but that is not going to block other processes going on in the program.
What this means is that we are going to be able to run two processes in parallel
without having to sacrifice runtime or a significant amount of memory. Another
great reason that we prefer to use NodeJS is its scalability. This system that we are
building is something that we want to continue to scale with other technologies which
are specified in CH.05.

```
Figure 3.1: NodeJS Server demonstrating Client-Server requests
```
#### 3.0.2 ExpressJS: Back-End to Front-End

The next step in our method of approach is to establish what web framework we
want to use to display our SQL table data. What better framework than NodeJS web
application framework ExpressJS. Express is a combination of middleware which is
executed from top to bottom. With this language, we are allowed to use several HTTP
verbs such as GET, PUT, POST AND DELETE. This is similar to CRUD(Create,
Read, Update, Delete) which is common terminology used when attempting to access
back-end databases. Among other web frameworks we could have chosen such as
HapiJS and SailsJS, Express is clearly the best choice.
The problem with Sails for example vs Express is that Sails was built on top of
Express so its easy if you know Express. This made it an easy choice to cross that


one out since I would’ve had to learn express anyways and it didn’t have any plug-ins
I couldn’t use in express. HapiJS is supported by Walmarts’ technology division so it
had the history behind it of dealing with a lot of data traffic. The biggest drawback
I saw when comparing Hapi’s documentation with ExpressJS documentation was the
amount of extra manual coding which was not needed in ExpressJS. For simplicity
and performance this is why we implemented Express.

#### 3.0.3 RESTful API Shippos

REST stands for Representational State Transfer. This is going to allow for the
retrieval of a large amount of real time data that can be accessed via HTTPS request.
Normally a RESTful service website will provide you with a key. This key is normally
private depending on the importance of the data or it can also be public if it is for
testing purposes. When we access this url, specifically the one for Shippos, we will
be entering data from the client into a JSON formatted document. This document is
then going to send the arguments the user has specified and these are going to be our
main variables for the application to perform our other javascript functions. These
paramaters are going to be things such as Address, Name, Zip, dimensions, Account
information. The whole 9 yards.
This data is going to give our web application the ability to provide accurate rate
information for land freight and store our results in our Mongo Database. If we refer
to our sample HTML code in our Appendix section we can clearly see how our div is
going to take in these requests as well as how our client is going to be able to send
the requests to the database.

```
$. getJSON (
shippo_test_1e249a5f6f08fe54db5df84b94ebdca05d50a537 ,
function ( data ) {
console. log ( data [ " Data " ][ " freightRates " ][3][ " USD "
]) ;
var freightRatesUSD = ( data [ " Data " ][ " freightRates "
][5][ "
USD " ]) ;
document. querySelector ( " # freightRatesUSD " ). innerHTML =
freightRatesUSD ;
document. querySelector ( " # freightRatesUSD " ). innerHTML =
FreightRates ;
```
This line of code is going to allow us to access as well as send parameters to update
Shippos. All of the parameters will be specified in the url and will hold our requests.
The reason the URL is shown as a bunch of numbers rather than what parameters
are specified is because this is an example of an API code that has been encrypted.
Shippos allows for us to use their program but not freely. As a developer, one can
request a private API key. The way that we store our requests is by storing our host
web server as a url and then parsing the data that the user must supply. The way we
are going to have the form on the HTML we will need to have a validator with our


express page to ensure that the user does not leave required variables blank. We will
do that with the code below:

```
const { body ,validationResult } = require(’express -validator/check ’)
;
const { sanitizeBody } = require(’express -validator/filter ’);
```
```
body(’city’).isLength ({ min: 1 }).trim().withMessage(’City empty.’),
body(’state’).isLength ({ min: 1 }).trim().withMessage(’State empty.’
),
body(’country ’).isLength ({ min: 1 }).trim().withMessage(’Country
empty.’),
body(’zipcode ’).isLength ({ min: 1 }).trim().withMessage(’Zip Code
empty.’)
```
```
sanitizeBody(’city’).trim().escape (),
sanitizeBody(’state’).trim().escape (),
sanitizeBody(’country ’).trim().escape (),
sanitizeBody(’zipcode ’).trim().escape (),
```
```
validattion=f a l s e;
(req , res , next) => {
// Extract the validation errors from a request.
const errors = validationResult(req);
```
```
i f (! errors.isEmpty ()) {
// There are errors. Render form again with sanitized values
/errors messages.
// Error messages can be returned in an array using ‘errors.
array() ‘.
}
e l s e {
validattion = true;
// Data from form is valid. POST queries into MongoDB and
GET updated values for rate calculating
}
}
```
We are going to have this for the above variables to ensure that these are the
required parameters to get an accurate rate. The next step would be to POST
onto our database and get the values from our API. The way we plan to POST to
the database is following the API documentation on SHIPPOS page. This code is
demonstrated below:

```
var addressFrom = shippo.address.create ({
"name":"Shawn Ippotle",
"company":"Shippo",
"street1":"215 Clayton St.",
"city":"San Francisco",
"state":"CA",
"zip":"94117",
"country":"US", // iso2 country code
"phone":"+1 555 341 9393",
```

```
"email":"shippotle@goshippo.com",
})
```
```
an example of the response when we get the data is Below:
{
"is_complete": true,
"object_created":"2014 -07 -09 T02 :19:13.174Z",
"object_updated":"2014 -07 -09 T02 :19:13.174Z",
"object_id":"d799c2679e644279b59fe661ac8fa488",
"object_owner":"shippotle@goshippo.com",
"validation_results": {},
"name":"Shawn Ippotle",
"company":"Shippo",
"street1":"215 Clayton St.",
"street2":"",
"city":"San Francisco",
"state":"CA",
"zip":"94117",
"country":"US",
"phone":"15553419393",
"email":"shippotle@goshippo.com",
"is_residential":true,
"metadata":"Customer ID 123456"
}
```
We are going to be able to access all of these ids using jQuery functions and post
them to our innerHTML divs and be able to have the user actually see the data. This
was the first step on getting the address and now we need to set up the parcel. The
parcel is another way to define the dimensions of the package. This is going to allow
us to also perform the volume weight function to calculate the air shipping cost. An
example of the parcel POST function is below:

```
var parcel = shippo.parcel.create ({
"length": "5",
"width": "5",
"height": "5",
"distance_unit": "in",
"weight": "2",
"mass_unit": "lb",
})
```
```
\\THE RESPONSE IS LISTED BELOW
```
```
{
"object_state":"VALID",
"object_created":"2014 -07 -08 T23 :19:19.565Z",
"object_updated":"2014 -07 -08 T23 :19:19.565Z",
"object_id":"7df2ecf8b4224763ab7c71fae7ec8274",
"object_owner":"shippotle@goshippo.com",
"template":n u l l,
"length":"5",
"width":"5",
```

```
"height":"5",
"distance_unit":"cm",
"weight":"2",
"mass_unit":"lb",
"metadata":"Customer ID 123456",
"test": true,
"extra": {
"reference_1": "",
"reference_2": ""
}
}
```
The length, width, height, weight are all stored as float values in javascript which
is exactly what we will need to perform our other volume functions for air and sea
shipping. The next step in this process is performing the actual shipment POST
method. This is going to include the two other functions responses that we just
perfromed. an example of the POST method for the shipping function is listed below:

```
\\POST function to place in app.js function
var shipment = shippo.shipment.create ({
"address_from": addressFrom ,
"address_to": addressTo ,
"parcels": parcel ,
"async": true
})
```
```
\\THE RESPONSE IS LISTED BELOW with the GET method needed to
retrieve it.
\\ shippo.shipment.retrieve(’89436997 a794439ab47999701e60392e ’);
```
```
{
"object_created":"2014 -07 -17 T00 :04:06.163Z",
"object_updated":"2014 -07 -17 T00 :04:06.163Z",
"object_id":"89436997 a794439ab47999701e60392e",
"object_owner":"shippotle@goshippo.com",
"status":"SUCCESS",
"address_from":{
"object_id": "fdabf0abb93c4460b60aa596116872a7",
"validation_results": {},
"is_complete": true,
"name": "Mrs Hippo",
"street1": "1092 Indian Summer Ct",
"city": "San Jose",
"state": "CA",
"zip": "95122",
"country": "US",
"phone": "4159876543",
"email": "mrshippo@goshippo.com"
},
"address_to":{
"object_id": "0476 d70c612a423f9509ba5f807569db",
```

"is_complete": true,
"validation_results": {},
"name": "Mr Hippo",
"street1": "965 Mission St #572",
"city": "San Francisco",
"state": "CA",
"zip": "94103",
"country": "US",
"phone": "4151234567",
"email": "mrhippo@goshippo.com"
},
"address_return":{
"object_id": "fdabf0abb93c4460b60aa596116872a7",
"is_complete": true,
"validation_results": {},
"name": "Mrs Hippo",
"street1": "1092 Indian Summer Ct",
"city": "San Jose",
"state": "CA",
"zip": "95122",
"country": "US",
"phone": "4159876543",
"email": "mrshippo@goshippo.com"
},
"parcels":[{
"object_id": "7df2ecf8b4224763ab7c71fae7ec8274",
"length": "10",
"width": "15",
"height": "10",
"distance_unit": "in",
"weight": "1",
"mass_unit": "lb"
}],
"shipment_date":"2013 -12 -03 T12 :00:00Z",
"extra":{
"insurance": {
"amount": "",
"currency": ""
},
"reference_1":"",
"reference_2":""
},
"customs_declaration":n u l l,
"rates": [
{
"object_created": "2014 -07 -17 T00 :04:06.263Z",
"object_id": "545 ab0a1a6ea4c9f9adb2512a57d6d8b",
"object_owner": "shippotle@goshippo.com",
"shipment": "89436997 a794439ab47999701e60392e",
"attributes": [],
"amount": "5.50",
"currency": "USD",


```
"amount_local": "5.50",
"currency_local": "USD",
"provider": "USPS",
"provider_image_75": "https :// cdn2.goshippo.com/
providers /75/ USPS.png",
"provider_image_200": "https :// cdn2.goshippo.com/
providers /200/ USPS.png",
"servicelevel": {
"name": "Priority Mail",
"token":"usps_priority",
"terms": "",
},
"estimated_days": 2,
"arrives_by": n u l l,
"duration_terms": "Delivery in 1 to 3 business days.",
"messages": [],
"carrier_account": "078870331023437 cb917f5187429b093",
"test": true
},
...
],
"carrier_accounts": [],
"messages":[],
"metadata":"Customer ID 123456",
"test": true
}
```
This is going to give us the most accurate land shipping. We can then use these
same variables we have taken from the form to our other volume functions for both
air and sea shipping. This technology is going to assist with getting the final rate on
land shipping. The code for calling the rate JSON file from the API of Shippos is
only going to require us to send a get request with out object id as the parameters.
This technique is listed below:

```
\\ GET METHOD
shippo.shipment.rates(’5e40ead7cffe4cc1ad45108696162e42 ’);
```
```
\\ RESPONSE
{
"next":n u l l,
"previous":n u l l,
"results":[
{
"object_created":"2014 -07 -17 T00 :04:10.118Z",
"object_id":"ee81fab0372e419ab52245c8952ccaeb",
"object_owner":"shippotle@goshippo.com",
"shipment":"89436997 a794439ab47999701e60392e",
"attributes":[
"CHEAPEST",
"BESTVALUE"
],
"amount":"5.35",
```

"currency":"USD",
"amount_local":"5.35",
"currency_local":"USD",
"provider":"USPS",
"provider_image_75":"https :// cdn2.goshippo.com/providers
/75/ USPS.png",
"provider_image_200":"https :// cdn2.goshippo.com/providers
/200/ USPS.png",
"servicelevel": {
"name":"Priority Mail",
"token":"usps_priority",
"terms":"",
},
"estimated_days":1,
"duration_terms":"Delivery within 1, 2, or 3 days based on
where your package started and where i t s being sent."
,
"carrier_account": "b741b99f95e841639b54272834bc478c",
"zone":"1",
"messages":[],
"test": true
},
{
"object_created":"2014 -07 -17 T00 :04:10.334Z",
"object_id":"fd31f301b61c49048f861573d4364ff6",
"object_owner":"shippotle@goshippo.com",
"shipment":"89436997 a794439ab47999701e60392e",
"attributes":[],
"amount":"16.03",
"currency":"USD",
"amount_local":"16.03",
"currency_local":"USD",
"provider":"USPS",
"provider_image_75":"https :// cdn2.goshippo.com/providers
/75/ USPS.png",
"provider_image_200":"https :// cdn2.goshippo.com/providers
/200/ USPS.png",
"servicelevel": {
"name":"Priority Mail Express",
"token":"usps_priority_express",
"terms":"",
},
"estimated_days":1,
"duration_terms":"Overnight delivery to most U.S. locations
.",
"zone":"1",
"messages":[],
"test": true
}
]
}

```
to
```

#### 3.0.4 Geocodes implemented into Shippos RestAPI

Something that this program that we did not know it could until late was it’s ability
to return whether or not the shipment is going to be international. If the answer is
true then we are going to need to begin to POST customs data from the shipment
and the country that we are exporting to that is going to be charging the taxes. This
POST method is shown below:

```
shippo.customsitem.create ({
"description": "T-Shirt",
"quantity": 2,
"net_weight": "400",
"mass_unit": "g",
"value_amount": "20",
"value_currency": "USD",
"tariff_number": "",
"origin_country": "US",
"metadata": "Order ID #123123"
});
```
```
\\ Response is listed below
shippo.customsitem.retrieve(’55358464 c7b740aca199b395536981bd ’);
```
```
{
"object_created":"2014 -07 -17 T00 :49:20.631Z",
"object_updated":"2014 -07 -17 T00 :49:20.631Z",
"object_id":"55358464 c7b740aca199b395536981bd",
"object_owner":"shippotle@goshippo.com",
"object_state":"VALID",
"description":"T-Shirt",
"quantity":2,
"net_weight":"400",
"mass_unit":"g",
"value_amount":"20",
"value_currency":"USD",
"origin_country":"US",
"tariff_number":"",
"metadata":"Order ID ’123123’",
"test": true
}
```
### 3.1 NoSQL/MongoDB vs MySQL

The reason for me using MySQL vs using NoSQL is for mainly issues with Scala-
bility. MySQL is a library created by Microsoft which allows you to connect to a
database from one machine on any machine as long as it is connected through an
instance through an application like MySQLWorkbench for example. A reason that
these Scalability issues wouldve been harder to solve with NoSql for example was
because the lack of a unique ID per entry in a table or database. MongoDB is also a


very scalable database to support mutiple entries and would assist in developing the
program further, a problem however with MongoDB was the lack of resources I could
find using Node.js framework to connect from MongoDB to have a good client-server
connection.
The reason I also chose to use MySQL was because of my familiarity with its
syntax. This was only the second time I would be initializing a relational database
and the only other time was with AWS cloud services. I was familiar with the order
of operations that the cloud would have to go through but most of my issues came
when attempting to create the instance locally on my laptop. This is what I faced the
most trouble with but after making some practice applications connecting to mySQL
workbench I was able to better understand what it is that the database is doing on
the backend and what is the best way to optimize this task of freight quoting.
The very first thing when implementing code was a realization that hit me later
rather than earlier. When thinking of the future works I want to accomplish for
the project. The main function that I want this application to provide is real time
accurate shipping rates. As we have spoken about so extensively in previous chapters
the hardest thing to rate is land freight. This is because of the liability shipping
goods on the ground can change the price per volume weight. We know that we need
to constantly be updating the database to provide read data to the client and provide
write access for the company.
To update the shipping rates for land freight we had to remember how a freight
forwarding company actually rates ground shipping. As spoken about in previous
chapters we know this is by being a broker to the client using licenses that allow for
the freight broker to get discounts on shipping and then sell a local carriers’ services
to the customer. Being that we could use local carriers I dove deeper to see how this
would be accomplished and what database languages would be optimal.
I found that most large corporations normally use both SQL and NoSQL databases
as different facets of companies require different tasks. NoSQL databases are great
for reading data and SQL databases are great for writing data. For the program
that we are writing currently we want to be able to provide write access after a user
has entered an order and a database which will return real time rates for shipping
classes for anywhere in the world. In my future works I had planned previously to
add the real time data but then we stumbled on RESTful services and thats when
we struck gold. Using RESTful services I will be able to use SHIPPOS APIkey for
free which allow me to return exactly what the freight broker will be able to see as
they can apply their company code for each carrier they have a contract with as well
as take that value and allow for us to then add the percent the broker will bill to the
customer.
This is when I realized that for this application to be successful, the best way to
go about it will be to first create a MongoDB database that will allow us to return
the real time land freight prices to the customer as well as identify what shipping
methods will be possible. Then using jQuery we will be able to return the data from
the carriers that will be able to provide the correct service and also allow us to use


the data to know what other shipping method is possible to also return this as well.
This is going to also allow for the user to get tax rates and tariffs for door to door
container shipping and terminal to terminal shipping as well. terminal to terminal
shipping is really the service we are looking for but we can set the terminals for
specific locations using our SQL database we will be setting up over the summer.
Long story short, The future works became the present works and the present works
became the future works with relation to the database technology we are using.
When we create the SQL database for write access, this is going to allow the
company to write the cargo terminals where the warehouse will be transported to
and whatever terminal is closest to the address will be the price returned to the
customer. Rather than the final destination. The shipping from terminal to terminal
will also allow for us to give a better rate on our shipping methods and allow us to
include new carriers and also combine our methods to allow for shipping calculation
from terminal to docks and then from the dock to ship or airplane and then the
terminal to terminal shipping again.

### 3.2 Test Environment

To ensure that our program is set up for expansion and growth upon completion.
There are certain checks that we have to do being that it is a web application. The
first of these checks would be funcitionality testing. This would be to ensure that
all the links, dropdown menus, buttons, and return statements all work exactly as
planned at first glance. This is a type of test that I would be able to carry out myself
as a stranger or a program might not know exactly what the requirements for the
system are. The next step would be to do Usability, Interface, and Database testing.
This is where I plan to get a focus group of 20 students to run these tests to test
for correct clientserver connection, no query error messages, and that the programs
usability is easy for a new user.
Using RESTapis we want to ensure that the information we are getting is actually
the information we are going to be querying for. Using Node we have access to the
Mocha library which is the best way I could find to test asynchronous web applica-
tions. Mocha tests run serially, allowing for flexible and accurate reporting, while
mapping uncaught exceptions to the correct test cases. Below is an example of the
format of the process of how Mocha tests asynchronous web applications:

```
run ’mocha spec.js’
|
spawn child process
|
|--------------> inside child process
process and apply options
|
run spec file/s
|
|--------------> per spec file
```

```
suite callbacks (e.g., ’describe ’)
|
’before ’ root -level pre -hook
|
’before ’ pre -hook
|
|--------------> per test
’beforeEach ’ root -level pre -hook
|
’beforeEach ’ pre -hook
|
test callbacks (e.g., ’it’)
|
’afterEach ’ post -hook
|
’afterEach ’ root -level post -hook
|<-------------- per test end
|
’after’ post -hook
|
’after’ root -level post -hooks
|<-------------- per spec file end
|<-------------- inside child process end
```
### 3.3 Experiments

The experiments I ran with this code enviornment involved the installation and testing
that a hello world line was logged into the console. Mocha allows for you to create
great custom tests for your web applications and run all the tests in the terminal.
This is similar to Travis which is used in the CS department with entry level students
labs and testing larger programs in the upper classes. below is a sample test code I
modified to test for the hello world line in the console:

```
var assert = require(’assert ’);
describe(’String ’, function () {
describe(’#typeOf ()’, function () {
it(’Should return a string with hello world’, function (){
assert.equal("Hello World", [0]. typeOf("String"));
});
});
});
```
### 3.4 Threats to Validity

The main threat I see to validity would be in the beginning stages of the application
we will not be able to implement web hooks to watch for changes in our RESTapi.
This is going to require us to refresh the page if we want to make another order to


ensure that the rates we are posting from our Mongo database are correct. This is
really the only threat to validity I see currently as the program is pretty straight
forward and the call to the API is only going to happen once the user enters all of
the information they need to enter to get the shipping rate.
Another possible threat to validity would be a faulty test case. To ensure that the
tests we are running are actually testing what we ask we have done this sample code
and from Mocha we got the following command line args:

```
$ ./ node_modules /.bin/mocha mocha.test.js
```
```
Hello World done
1) done
```
```
1 passing (6ms)
0 failing
```

# Chapter 4

# Implementation

### 4.1 JavaScript code to Create server connection to NoSQL/SQL Database

#### 4.1.1 First Steps to Success

To create this program we have decided to go with the prototype software engineering
model. What this basically means is that instead of trying to fulfill all of the require-
ments for the system immediately and build a system and iteratively develop new
test cases. We are releasing a few of the requirements and ensuring that the system is
still scalable. This program is going to first off be based in one database and that will
be a MongoDB hosted on my local port. The very first step of the implementation
at this point would be to create a local instance on my computer using just regular
server code to get the server started. This is a process that is not hard but it needs
to be the very first step.


```
Figure 4.1: Hosting local web server on port 3000
```
#### 4.1.2 backend to frontend: WARNING! useful content ahead

The next step in the process is going to be what we are populating the database
with. We have established in previous chapters that what we are going to do is have
a Shippo API key which I have already obtained as a part of my free account. This
is going to give us the chance to be able to find out the class type for items when we
are performing landfreight options as well as help us store our variables for our other
two shipping methods. We then populate the database as we begin to get POST
requests from the user on the front-end. For now, lets stay on the backend just a
little while longer. We need to be sure that we are going to be able at some point
in time after the prototype is successful to be able to query and write into our SQL
database which is going to allow for us to enter more data on the shipment process
like actual terminal locations which is going to allow to use our system dynamically.
When I say dynamically I mean that although parts of the system will be constantly
changing due to the never not updating API, there are still many parts of the system
that we won’t have to change(prototype ¡ everything).

#### 4.1.3 Why don’t you just meet me in the middle(ware)

We are already well aware of how to CRUD in SQL so we can skip ahead. That
was kind of a long section. Anyways, After setting up our back-end functions and


schemas, we want to begin to look into how we are going to get from the back to
the front end. If your memory serves you right you will remember that we talked
about how ExpressJS will act as a middle ware. Well after the back, you guessed it,
its the middle! We now are going to use our Node.js modules (express, https, assert,
mongoose etc.) and ensure that these are stored in our program. We need to make
sure that the application is going to be able to run if the customer does not have all
of these installed. The next step would be to add these modules to our dependencies
list in our package.json file.

#### 4.1.4 json

This file is going to have all the information on what port our application is running
on, what is version, who is the author, the whole enchilada. The next step in this
process would be to create the node application which is going to take our back-end
work and with the help of our web framework (express). We are going to be able
to output the data into divs and style it how we please. This is what proved to be
the most challenging part of the process as there are so many new technologies with
similar functions that it was a lot of trial and error. With NodeJS we will set our
port and start the app in the terminal using the ’node app.js’ CLI command.

```
Figure 4.2: Hosting local web server on port 3000
```

#### 4.1.5 innerHTML forms

The next step in our code after getting the data, we need to connect to our front-
end friends who want to give us their money. The way we can do this is by using
our web framework express. This is how we are going to be sending our POST and
GET commands from the user to the database and keep the cycle going until the
user is happy. The next step in this process is to set up our html forms. The code
is shown above but the inpur from these forms are going to be the parameters for
our application so we are going to need to do some testing to ensure that everything
is working and returning information exactly as it should. To ensure that this is
happening we will then move forward into how we are implementing our testing
functions.

#### 4.1.6 testing

One thing that we want to do in this testing phase is to check for a few things listed
below: 1. Is the system built right? 2. Are we querying the correct data? 3. Is the
data matching in the database? 4. Is the user entering the any information? 5. Is
the user entering the correct information
We are going to be using the unit-testing library known as mocha and install it
as a dependency in our package.json file. How we are going to address issue number
one and two is by using the unit test to ensure that the system compiles and we are
able to pull the data into the form. The way we can ensure the data is matching the
database is by having Mocha return a failed test if a get method is run attempting
to query a shipment or an address or a parcel that the shipmentID is not available or
does not match any of the shipmentIDs in the database. In ch03 we spoke more on
how we are going to use assertion testing to ensure that everytime the user attemps
to access the API key he must have all of the sections filled out.

#### 4.1.7 Gators Win

Using this methodology and these practices a user will be able to access the server
on my port, enter their city and country of origin and the program will then identify
what forms of transportation are possible and how much it will cost to ship them
both. This will return what is the cheapest and what is the fastest way to get your
cargo from point A to point B. The program will also be very scalable since it was
built from its foundation with technologies such as MongoDB, JavaScript, HTMl,
NodeJS, and Express.

```
%{
% "name": "therightfit",
% "version": "1.0.1",
% "description": "theRightFit",
% "main": "sandbox.js",
% "dependencies": {
```

```
% \\ modules that must be installed in order to correctly run
the application
% },
% "devDependencies": {
% \\ Dependencies which are not required but good to have
% },
% "scripts": {
% "test": "echo \"Error: no test specified \" && exit 1"
% },
% "keywords": [
% "any",
% "keyword"
% ],
% "author": "Karol Alberto Vargas",
% "license": "ISC"
%}
%
```
### 4.2

```
¡/table¿ ¡/body¿ ¡/html¿
```
### 4.3 Creating MySQL table for quering into database

CREATE TABLE ClassIdentification(
MINDENSITY INTEGER,
CLASSINTEGER NOT NULL PRIMARY KEY,
RATEPER f l o a t NOT NULL
);

```
INSERT INTO ClassIdentification ()VALUES (’50’, ’50’, ’1.25’);
INSERT INTO ClassIdentification ()VALUES (’35’, ’55’, ’2.5’);
INSERT INTO ClassIdentification ()VALUES (’30’, ’60’, ’3.75’);
INSERT INTO ClassIdentification ()VALUES (’22.5’, ’65’, ’6.25’);
INSERT INTO ClassIdentification ()VALUES (’15’, ’70’, ’9.40’);
INSERT INTO ClassIdentification ()VALUES (’13.5’, ’77.5’, ’12.55’);
INSERT INTO ClassIdentification ()VALUES (’’, ’’, ’18.85’);
INSERT INTO ClassIdentification ()VALUES (’’, ’’, ’25.10’);
INSERT INTO ClassIdentification ()VALUES (’’, ’’, ’31.35’);
INSERT INTO ClassIdentification ()VALUES (’’, ’’, ’34.50’);
INSERT INTO ClassIdentification ()VALUES (’’, ’’, ’39.20’);
INSERT INTO ClassIdentification ()VALUES (’’, ’’, ’47.10’);
INSERT INTO ClassIdentification ()VALUES (’’, ’’, ’54.95’);
INSERT INTO ClassIdentification ()VALUES (’’, ’’, ’62.85’);
INSERT INTO ClassIdentification ()VALUES (’’, ’’, ’78.45’);
INSERT INTO ClassIdentification ()VALUES (’’, ’’, ’94.15’);
INSERT INTO ClassIdentification ()VALUES (’’, ’’, ’125.55 ’);
INSERT INTO ClassIdentification ()VALUES (’0’, ’’, ’156.95 ’);
```

# Chapter 5

# Discussion and Future Work

### 5.1 Future Work

The way I plan to use this program in the future is simple. I am going to first
implement this application into the database for LSPCargo.com. Afterwards I plan
on beginning to add more server side queries which is going to allow for me to make
changes to the way the data is being viewed by both client and server. The server side
needs to see what rate is the best that the customer will make and the server needs
to know what carrier to use to get the most money out of the deal. The way that I
plan to implement this is having a different node.js application for a different HTML
that can be accessed on the same main page HTML of the website. This application
however is going to be only for workers at LSPCargo and it is going to be able to
show the orders people have made in the time they were not awake or the time they
were closed and begin to make reservations for whatever vessel is needed to get that
cargo to its final destination. This is going to allow for LSP to have more revenue by
allowing users to be able to see what it is that they will pay for their service without
it having to be business hours.
The next step of my future works would be to use this experience that I have in
developing web applications to begin to create software for supply chain companies as
there is an upward trend on ecommerce. This application has given me the experience
I feel I really had to work hard for and that it will show the more and more time I
put in it. I hope that this application can be the beginning to a new wave at LSP
to move into the more technical side of supply chain as there are many NVOCCs in
Miami where they are located but they all want the fastest way to get their rates to
their customers. This is the reason I felt kept me motivated throughout the entire
comping process. Realizing how stiff the competition is and how hard it would be
to excel, I knew hard work was the only thing that could guarantee success. This
whole application has let me see how much really goes on in the backend of computing
that I didn’t get the experience in but it has made me want to pursue a career as a
Database Manager or Data Scientist someday.
I hope this application can bring as much joy to those who use it as it did for me
while making it. This was an incredibly difficult process and the only way I feel I can


sum it up is with my favorite quote from Teddy Roosevelt
”It is not the critic who counts; not the man who points out how the strong man
stumbles, or where the doer of deeds could have done them better. The credit belongs
to the man who is actually in the arena, whose face is marred by dust and sweat and
blood; who strives valiantly; who errs, who comes short again and again, because
there is no effort without error and shortcoming; but who does actually strive to do
the deeds; who knows great enthusiasms, the great devotions; who spends himself in
a worthy cause; who at the best knows in the end the triumph of high achievement,
and who at the worst, if he fails, at least fails while daring greatly, so that his place
shall never be with those cold and timid souls who neither know victory nor defeat.”
Thank you for the opportunity to pursue something that will make a massive
impact on my family and to make me fall in love with learning again. I guess it was
just theRightFit.


# Appendix A

# Java Code

All program code should be fully commented. Authorship of all parts of the code
should be clearly specified.

```
/***
package.json
*/
```
```
"name": "therightfit",
"version": "1.0.1",
"description": "theRightFit",
"main": "sandbox.js",
"dependencies": {
\\ modules that must be installed in order to correctly run the
application
},
"devDependencies": {
\\ Dependencies which are not requiered but good to have
},
"scripts": {
"test": "echo \"Error: no test specified \" && exit 1"
},
"keywords": [
"any",
"keyword"
],
"author": "Karol Alberto Vargas",
"license": "ISC"
```
```
/***
SQL.js
*/
```
```
var mysql = require(’mysql’);
```
```
var con = mysql.createConnection ({
host: "localhost",
```

user: "root",
password: "Kv #446324"
});

module.exports{
\\ where we will export our modules containing to functions to run
specific queries depending on where we will ship
}

con.connect(function(err) {
i f (err) throw err;
console.log("Connected!");
con.query("SELECT mindensity FROM sandbox.ClassIdentification",
function (err , result , fields) {
i f (err) throw err;
console.log(result [1]. mindensity);
});
});

/***
SQL.js
*/

var mysql = require(’mysql’);

var con = mysql.createConnection ({
host: "localhost",
user: "root",
password: "Kv #446324"
});

module.exports{
\\ where we will export our modules containing to functions to run
specific queries depending on where we will ship
}

con.connect(function(err) {
i f (err) throw err;
console.log("Connected!");
con.query("SELECT mindensity FROM sandbox.ClassIdentification",
function (err , result , fields) {
i f (err) throw err;
console.log(result [1]. mindensity);
});
});

<html>
<head>
<t i t l e>Karol ’s sample page </t i t l e>
<s c r i p t s r c="sandbox.js"></s c r i p t>


```
<body>
<t a b l e>
<t r>
<td>
<b> $("document").ready(function (){
$.getJSON(’
shippo_test_1e249a5f6f08fe54db5df84b94ebdca05d50a537 ’,
function(data){
console.log(data["Data"]["freightRates"][3]["USD"]);
```
```
var freightRatesUSD = (data["Data"]["freightRates"][5]["
USD"]);
```
```
document.querySelector("#freightRatesUSD").innerHTML =
freightRatesUSD;
document.querySelector("#freightRatesUSD").innerHTML =
FreightRates;
}); </b> </td>
```
```
</t a b l e>
</body>
</html>
```
```
/***
ClassIdentification.sql
*/
```
CREATE TABLE ClassIdentification(
MINDENSITY INTEGER,
CLASSINTEGER NOT NULL PRIMARY KEY,
RATEPER f l o a t NOT NULL
);

```
INSERT INTO ClassIdentification ()VALUES (’50’, ’50’, ’1.25’);
INSERT INTO ClassIdentification ()VALUES (’35’, ’55’, ’2.5’);
INSERT INTO ClassIdentification ()VALUES (’30’, ’60’, ’3.75’);
INSERT INTO ClassIdentification ()VALUES (’22.5’, ’65’, ’6.25’);
INSERT INTO ClassIdentification ()VALUES (’15’, ’70’, ’9.40’);
INSERT INTO ClassIdentification ()VALUES (’13.5’, ’77.5’, ’12.55’);
INSERT INTO ClassIdentification ()VALUES (’20’, ’60’, ’18.85’);
INSERT INTO ClassIdentification ()VALUES (’16’, ’55’, ’25.10’);
INSERT INTO ClassIdentification ()VALUES (’52’, ’18’, ’31.35’);
INSERT INTO ClassIdentification ()VALUES (’48’, ’56’, ’34.50’);
INSERT INTO ClassIdentification ()VALUES (’45’, ’48’, ’39.20’);
```
```
const { body ,validationResult } = require(’express -validator/check ’)
;
const { sanitizeBody } = require(’express -validator/filter ’);
```

body(’city’).isLength ({ min: 1 }).trim().withMessage(’City empty.’),
body(’state’).isLength ({ min: 1 }).trim().withMessage(’State empty.’
),
body(’country ’).isLength ({ min: 1 }).trim().withMessage(’Country
empty.’),
body(’zipcode ’).isLength ({ min: 1 }).trim().withMessage(’Zip Code
empty.’)

sanitizeBody(’city’).trim().escape (),
sanitizeBody(’state’).trim().escape (),
sanitizeBody(’country ’).trim().escape (),
sanitizeBody(’zipcode ’).trim().escape (),

validattion=f a l s e;
(req , res , next) => {
// Extract the validation errors from a request.
const errors = validationResult(req);

i f (! errors.isEmpty ()) {
// There are errors. Render form again with sanitized values
/errors messages.
// Error messages can be returned in an array using ‘errors.
array() ‘.
}
e l s e {
validattion = true;
// Data from form is valid. POST queries into MongoDB and
GET updated values for rate calculating
}
}

var addressFrom = shippo.address.create ({
"name":"Shawn Ippotle",
"company":"Shippo",
"street1":"215 Clayton St.",
"city":"San Francisco",
"state":"CA",
"zip":"94117",
"country":"US", // iso2 country code
"phone":"+1 555 341 9393",
"email":"shippotle@goshippo.com",
})

var parcel = shippo.parcel.create ({
"length": "5",
"width": "5",
"height": "5",
"distance_unit": "in",
"weight": "2",
"mass_unit": "lb",
})


var shipment = shippo.shipment.create ({
"address_from": addressFrom ,
"address_to": addressTo ,
"parcels": parcel ,
"async": true
})

shippo.shipment.rates(’’);

shippo.customsitem.create ({
"description": "T-Shirt",
"quantity": 2,
"net_weight": "400",
"mass_unit": "g",
"value_amount": "20",
"value_currency": "USD",
"tariff_number": "",
"origin_country": "US",
"metadata": "Order ID #123123"
});


# Bibliography

[1] Muath Alkhalaf. Verifying client-side input validation functions using string anal-
ysis. 2012.

[2] Neil A. Barni. Method for online display and negotiation of cargo rates. 1999.

[3] Matteo Biagiola, Filippo Ricca, and Paolo Tonella. Search based path and input
data generation for web application testing, 2017.

[4] Andreas Gizas. Comparative evaluation of javascript frameworks. 2012.

[5] Laurent D. Hasson. Method and system for retaining formal, data model, descrip-
tions between server-side browser-side javascript objects. 2004.

[6] Shunfu Hu. Online map application development using google maps api, sql
database, and asp.net. 2013.

[7] Perdro A. Castillo J.L.J Laredo A. Mora Garcia A. Prieto Juan Julian, Merelo-
Guervos. Asynchronous distributed genetic algorithms with javascript and json,
”June” 2008.

[8] Sharad Mehrotra. Executing sql over encrypted data in the database-service-
provider model. 2002.

[9] Veikkolainen. Paleomagia: A php/mysql database of the precambrian paleomag-
netic data. 2014.
