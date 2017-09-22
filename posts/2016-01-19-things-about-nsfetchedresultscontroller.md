---
title: Things about NSFetchedResultsController
permalink: nsfetchedresultscontroller-16-01-19
date: 2016-01-19 18:26
---

> Based on Xcode 7.2(7C68)

| age | name | |-----|------| | 20 | Jake | | 20 | Mike | | 31 | Sara | | 32 | Fish |

Suppose we have a database table like this. To fetch data, we need instant a `NSFetchedResultsController` then perform it (no sorting in fetchRequest):  
```
var fetchedAge = NSFetchedResultsController(fetchRequest: fetchRequest, managedObjectContext: managedContext, sectionNameKeyPath: <strong>"age"</strong>, cacheName: nil)<br></br>
do {<br></br>
try fetchedAge.performFetch()<br></br>
} catch {<br></br>
fatalError("Failure to save context: \(error)")<br></br>
}```

Notice that I set `sectionNameKeyPath` argument to **“age”**. This will make the `fetchAge` fetch 3 sections because the first two records with the same “age” value will merge into a single section. The fetch result will be like this:

| | | | age | name | |-----------|------------|---------------|-----|------| | object[0] | section[0] | sec[0].obj[0] | 20 | Jake | | object[1] | | sec[0].obj[1] | 20 | Mike | | object[2] | section[1] | sec[1].obj[0] | 31 | Sara | | object[3] | section[2] | sec[2].obj[0] | 32 | Fish |

Which means:  
`fetchAge.sections?.count` will return “option(3)”  
 while  
`fetchAge.fetchedObjects?.count` will return “option(4)”.

We can access the value “Mike” in two ways (“Events” is the name of my NSManagedObject subclass):  
`(fetchAge.fetchedObjects![1] as! Events).name`  
 or  
`(fetchRecords.sections?[0].objects?[1] as! Events).name`

**Of course we can do all of above to fetch data.**

**But typically, only when we want to know how many person are in age 20 will we set `sectionNameKeyPath` argument to “age”.  
`fetchAge.sections?[0].numberOfObjects`** will return the amount of person who is in age 20.

**Without specific requirement, people usually set `sectionNameKeyPath` argument to nil.**



