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
`match(e:employee{name:’satish’}) set e.salary = 20000` <br>
This will set the salary property to 20000, this property was not included before, it is added now.

Removing property using `remove` clause.<br>
`match(e:employee{name:’satish’}) remove e.salary`<br>
This will remove the salary property from node.<br>

Adding and Removing Properties using the single command.<br>
`match(e:employee{name:’satish’}) remove  e.name set e.firstName=’satish’ return e`
<br>
This will return node of satish with ‘name’ property being removed and firstName is added. <br>

Using ‘Set’ and ‘Remove’ clauses to change the `label`/`Type` of a node.<br>
`match(e:employee{name:”satish”}) remove e:employee set e:clerk return e`<br>
This will remove ‘e’ from employee and set it to ‘clerk’ label. <br>

Using ‘Delete’ clause to delete the node.<br>
`match(e:employee) delete e`<br>
`match(n) delete n`<br>

## Using Boolean Operators
This “IN” keyword search for value to fall in specific set of values like in above array.
`match(e:employee)wheree.namein['satihs','Haseeb']return e`
<br>
Use of ‘AND’ operator.<br>
`match(e:employee)wheree.name='Haseeb'ande.salary=20000 return e`
<br>
Use of ‘OR’ operator.<br>
`match(e:employee)where e.name='Haseeb'or e.name='satish' return e`<br> 

## Creating Relationship between Nodes
In this we are first matching/finding the required nodes, then we are creating a link between two nodes by specifying the `Label` of relationship and then returning all ‘nodes’ and their `relation`.<br>
`match(e:employee{name:’satish’}), (d:department{name:’scope’}) create (e) –[w:worksFor] -> (d) return e,w,d` <br> 

This will find the all employees which have `workfor` relationship with department named as `scope`.<br>
`match(e:employee)-[w:worksfor]->(d:department{name:’scope’}) return e,w,d` <br> 

It will return all departments in which `satish` works.
`match(e:employee{name:’satish’})-[w:worksfor]->(d:department) return e,w,d`

Updating Relations between Nodes
To add more values/information to label attaching the two nodes.<br>
`match(e:employee{name:"Haseeb"})-[s:studiesIn]->(d:department{name:"SEECS"}) set s.studyingDepartment="DOC" return e,s,d`
<br> 

To remove the new value added on label.<br>
`match(e:employee{name:"Haseeb"})-[s:studiesIn]->(d:department{name:"SEECS"}) remove s.studyingDepartment return e,s,d`<br> 

To delete the relationship between the nodes and set them free of each other. <br>
`match(e:employee{name:"Haseeb"})-[s:studiesIn]->(d:department{name:"SEECS"})  delete s return e,d`<br> 

## Creating Relationships While Creating Nodes and More on Node Relationships
When we have relationship between two nodes, we can’t delete those nodes. The command for this will be, `match(n) detach delete n`. <br>For deleting a specific node we can first <br>
`select it using match() clause and then detach it and at the end delete it`.
<br> <br>
To create nodes and their relationship at the same time.<br>
`create(d:driver{name:'Haseeb'})-[r:drives]->(c:car{name:"Kia"}) return d,r,c`
<br> <br>
To select all nodes irrespective of their labels, which have incoming/outgoing “specific label” relationship attached with them.<br>
`match(e)-[r:drives]->(d) return e,r,d`
<br> <br>
