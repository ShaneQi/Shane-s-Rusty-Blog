---
title: Make tableView Cell Inserting/Deleting Smoothly
permalink: smooth-tableview-cell-inserting-delete-16-09-13
date: 2016-09-13 05:12
---

```swift
UIView.animateWithDuration(0, animations: {
	//  ...Change tableView height...
}) { 
	if $0 {
		tableView.deleteRowsAtIndexPaths([NSIndexPath(forRow: indexPath.row, inSection: indexPath.section)], withRowAnimation: .None) 
		//  Or insert...
		tableView.insertRowsAtIndexPaths([NSIndexPath(forRow: 0, inSection: indexPath.section)], withRowAnimation: .None)
	} 
}
```
