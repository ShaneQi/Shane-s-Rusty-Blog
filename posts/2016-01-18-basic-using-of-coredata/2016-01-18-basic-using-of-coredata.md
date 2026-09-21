---
title: Basic Using of CoreData
permalink: basic-using-of-coredata-16-01-18
date: 2016-01-18 22:33
---

> Based on Xcode 7.2(7C68)

## SETUP COREDATA

1. Add an entity to the .xcdatamodeld then attributes to the entity. When finish the entity and attributes, create NSManagedObject subclass from the “Editor” menu.
2. Declare a static managedObjectContext variable in the top of `AppDelegate.swift` to use conveniently in the future:  
`let managedContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext`

## WRITING

1. Get the entity out from ManagedContext:  `let entity = NSEntityDescription.entityForName("Entity", inManagedObjectContext: managedContext)`

2. Decalre a new record:  

```swift
//  "Entity" in the next line is the name of NSManagedObject subclass<br></br>
let newdiary = Entity(entity: entity!, insertIntoManagedObjectContext:managedContext)
```
3. Edit the attributes value of the new record: 
 
```swift
newdiary.day = Int(day.text!)<br></br>
newdiary.des = "This is Description."<br></br>
newdiary.fav = true<br></br>
newdiary.id = 20160117<br></br>
newdiary.img = nil // Here supposed to be a Date variable.<br></br>
newdiary.month = 1<br></br>
newdiary.year = 2016
```
4. Save the new record to CoreData:  
```swift
do {<br></br>
try managedContext.save()<br></br>
} catch {<br></br>
fatalError("Failure to save context: \(error)")<br></br>
}
```

## READ

1. Declare two NSFetchResultsController inside the ViewController class:  
```swift
var fetchedDay : NSFetchedResultsController!<br></br>
var fetchedDes : NSFetchedResultsController!
```

2. Create a fetch request (target entity is “Entity”):  `let fetchRequest = NSFetchRequest(entityName:"Entity")`

3. Sort the fetch result (ascending by the column “day”):  `fetchRequest.sortDescriptors = [NSSortDescriptor(key: "day", ascending: true)]`

4. Init two NSFetchResultsController:  

```swift
fetchedDay = NSFetchedResultsController(fetchRequest: fetchRequest, managedObjectContext: managedContext,sectionNameKeyPath: "day", cacheName: nil)<br></br>
fetchedDes = NSFetchedResultsController(fetchRequest: fetchRequest, managedObjectContext: managedContext,sectionNameKeyPath: "des", cacheName: nil)
```

5. Try to fetch data:  

```swift
try self.fetchedDay.performFetch()
try self.fetchedDes.performFetch()
```

6. Do not forget to handle errors thrown from try command (embed all the reading codes inside do{} and catch error):  

```swift
do {
......
} catch {
fatalError("Failure to save context: \(error)")
}
```

7. Get data out of fetch result (get the Xth data content):  

```
var day = fetchedDay.sections![X].name<br></br>
var des = fetchedDes.sections![X].name
```
