# Neo4J
Neo4j is a graph database management system developed by Neo4j. Neo4j is implemented in Java and accessible from software written in other languages using the Cypher query language .
In this file let's see how to `Create`, `Read`, `Update`, `Delete` data in graph database through Neo4J.

## Creating Nodes
To create a new node in database:<br>
    `create(n:"Label Name")` <br>Example<br> `create(n:"employee")`<br><br>
Create and Return new node in database.<br>
    `create(n:"Label Name") return n` <br>Example<br>`create(n:"employee") return n`<br><br>
Create Node with multiple labels:<br>
    `create(n:"First Name:Second Label:Third Label:....") return n` <br>Example<br><br>
    `create(n:"employee:proctor") return n`<br>
Creating Multiple Node with Single Command having same or different labels:<br><br>
`create(n:"Label Name"),(n1:"Label Name") return n,n1`
<br>Example<br> `create(n:employee),(n1:proctor) return n,n1`<br><br>
Creating Nodes with Properties:<br>
`create(n:"Label Name{propertyName:"PropertyValue",...."}"),(n1:"Label Name{propertyName:"PropertyValue",...."}") return n,n1`<br>Example<br>
`create(n:employee{name:'ram', branch:'cse'}),(n1:proctor{name:''someone}) return n,n1`<br><br>

Creating multiple nodes of same and different Label having different properties.
`CREATE(e:Label Name{propertyName:"PropertyValue",...."}),(n1:"Second Label{propertyName:"PropertyValue",...."}")return n,n1
e`<br>Example<br>
`CREATE(e:employee{name:"ram",branch:"cse"}),(f:employee{name:"sam",branch:"ece"}),(h:professor{name:"satish",designation:"ap1"}),(j:professor{name:"ramesh",designation:{"ap2"}})  return e,f,h,j
e`

## Reading/Finding Node
Match all nodes inside database but return none.<br>
`Match(n)`<br>

Match all nodes inside database and return all nodes.<br>
`Match(n) return n`<br>

Finding Nodes of specific labels and returning their properties.<br>
`MATCH(e:employee),(d:department) return e.name,d.name`<br>

Using `Where` clause to find specific person from selected label/tale.<br>
`MATCH(e:employee) where e.name="HaseebAhmad" return e`<br>

Finding specific person without using Where Clause.<br>
`MATCH(e:employee{name:"HaseebAhmad"}) return e`<br>

## Updating Node Parameters

Using `set` clause to update the property.<br>
`match(e:employee{name:???satish???}) set e.salary = 20000` <br>
This will set the salary property to 20000, this property was not included before, it is added now.

Removing property using `remove` clause.<br>
`match(e:employee{name:???satish???}) remove e.salary`<br>
This will remove the salary property from node.<br>

Adding and Removing Properties using the single command.<br>
`match(e:employee{name:???satish???}) remove  e.name set e.firstName=???satish??? return e`
<br>
This will return node of satish with ???name??? property being removed and firstName is added. <br>

Using ???Set??? and ???Remove??? clauses to change the `label`/`Type` of a node.<br>
`match(e:employee{name:???satish???}) remove e:employee set e:clerk return e`<br>
This will remove ???e??? from employee and set it to ???clerk??? label. <br>

Using ???Delete??? clause to delete the node.<br>
`match(e:employee) delete e`<br>
`match(n) delete n`<br>

## Using Boolean Operators
This ???IN??? keyword search for value to fall in specific set of values like in above array.
`match(e:employee)wheree.namein['satihs','Haseeb']return e`
<br>
Use of ???AND??? operator.<br>
`match(e:employee)wheree.name='Haseeb'ande.salary=20000 return e`
<br>
Use of ???OR??? operator.<br>
`match(e:employee)where e.name='Haseeb'or e.name='satish' return e`<br> 

## Creating Relationship between Nodes
In this we are first matching/finding the required nodes, then we are creating a link between two nodes by specifying the `Label` of relationship and then returning all ???nodes??? and their `relation`.<br>
`match(e:employee{name:???satish???}), (d:department{name:???scope???}) create (e) ???[w:worksFor] -> (d) return e,w,d` <br> 

This will find the all employees which have `workfor` relationship with department named as `scope`.<br>
`match(e:employee)-[w:worksfor]->(d:department{name:???scope???}) return e,w,d` <br> 

It will return all departments in which `satish` works.
`match(e:employee{name:???satish???})-[w:worksfor]->(d:department) return e,w,d`

Updating Relations between Nodes
To add more values/information to label attaching the two nodes.<br>
`match(e:employee{name:"Haseeb"})-[s:studiesIn]->(d:department{name:"SEECS"}) set s.studyingDepartment="DOC" return e,s,d`
<br> 

To remove the new value added on label.<br>
`match(e:employee{name:"Haseeb"})-[s:studiesIn]->(d:department{name:"SEECS"}) remove s.studyingDepartment return e,s,d`<br> 

To delete the relationship between the nodes and set them free of each other. <br>
`match(e:employee{name:"Haseeb"})-[s:studiesIn]->(d:department{name:"SEECS"})  delete s return e,d`<br> 

## Creating Relationships While Creating Nodes and More on Node Relationships
When we have relationship between two nodes, we can???t delete those nodes. The command for this will be, `match(n) detach delete n`. <br>For deleting a specific node we can first <br>
`select it using match() clause and then detach it and at the end delete it`.
<br> <br>
To create nodes and their relationship at the same time.<br>
`create(d:driver{name:'Haseeb'})-[r:drives]->(c:car{name:"Kia"}) return d,r,c`
<br> <br>
To select all nodes irrespective of their labels, which have incoming/outgoing ???specific label??? relationship attached with them.<br>
`match(e)-[r:drives]->(d) return e,r,d`
<br> <br>

## Updating Relations between Nodes
This will add more values/information in label attaching the two nodes.<br>
`match(e:employee{name:"Haseeb"})-[s:studiesIn]->(d:department{name:"SEECS"}) set s.studyingDepartment="DOC" return e,s,d`<br>

This will remove the new value added on label as `studyingDepartment`.<br>
`match(e:employee{name:"Haseeb"})-[s:studiesIn]->(d:department{name:"SEECS"}) remove s.studyingDepartment return e,s,d`

## Creating Relationships While Creating Nodes and More on Node Relationships
### Deletion of Nodes having relationship
When we have relationship between two nodes, we can???t delete those nodes. The command for this will be<br>
`match(n) detach delete n`.<br>
For deleting a specific node we can <br>
`select it using match() clause and then detach it and at the end delete it`.
<br><br>
### Creating Node along with Relations
This will create both nodes and their relationship at the same time.<br>
`create(d:driver{name:'Haseeb'})-[r:drives]->(c:car{name:"Kia"}) return d,r,c`
<br><br>
This will select all nodes irrespective of their labels, which have incoming/outgoing ???drives??? relationship attached with them.<br>
`match(e)-[r:drives]->(d) return e,r,d`
<br><br>
This query will add two nodes in the same query time to a single department.<br>
`create(e:employee{name:"Waleed"})-[s:studiesIn]->(d:department{name:"SEECS"}) <-[s1:studiesIn]-(e1:employee{name:"Kalasra"}) return e, e1, s, s1, d`
<br><br>
This will create three Node in which one is working in two departments and other two are working in same department along with the departments in which they are working. This operation is completed using a single command.
<br>
`CREATE(e:employee{name:"satish"})-[w:worksFor]->(d:department{name:"SMME"})<-[w1:workFor]-(e1:employee{name:"sam"})-[m:manages]->(d1:department{name:"sbst"})<-[m1:manages]-(e2:employee{name:"tom"}) return e, w, d, w1, e1, m,d1, m1,e2`

## Finding all In-coming and Out-Going Relationships
This will return all incoming relationships from to ???SMME??? department, from any other node. <br>
`match(d:department{name:"SMME"})<--(e) return d,e`
<br><br>
This return all out-going nodes of all employees, like to which other node they are attached with. 
<br>
`match(d:employee)-->(e) return d,e `
<br><br>
This query will return nodes which have ???manages???, ???workFor??? relationships between. Each node can have both or any one relationship.<br>
`match(e)-[m:manages|worksFor]->(d) return m,d,e`
<br>
## More on Clauses
### Order By Clause
This will return employee names in ascending order.
<br>
`match(e:employee) return e.name order by e.name`
<br><br>
This will return names of employees ordered by multiple properties.
<br>
`match(e:employee) return e.name, e.salary order by e.name, e.salary`
<br>

### Skip Clause
This command will skip the first ???Integer??? value from result list and return the remaining list.
<br>
`match(e:employee) return e.name skip {Any Integer Value}`
<br><br>
### Limit Clause
This returns only top 3 results from the result list.
<br>
`match(e:employee) return e.name limit 3`
<br><br>
### Union and Union All Clause
This command will return unique result by joining two sets described in our query labels.
<br>
`match(e:employee) return e.name UNION match (e:employee) return e.name`
<br><br>

This command will return all values irrespective of rather they occur single time or twice or more, that is the main difference between ???UNION??? and ???UNION ALL??? clause. 
<br>
`match(e:employee) return e.name UNION ALL match (e:employee) return e.name`
<br><br>

### Merge Clause
This query creates a new node if it does not exist otherwise return it as it is, with its value.
<br>
`merge(e:driver) return e`
<br><br>


This command will create a new node if it does not exist and if it is created by this command then it will set its property ???Joined??? on creating this node.
<br>
`merge(e:clerk) on create set e.Joined="Today" return e`
<br><br>

This command will create a new node if it does not exist and if it existed then this command will change the ???Joined??? property to a new value.
<br>
`merge(e:clerk) on match set e.Joined="Yesterday" return e`
<br><br>

Single command to run above both cases.
<br>
`merge(e:clerk) on match set e.Joined="Yesterday" on create set e.Joined="Today" return e`
<br><br>
