# Core Data

## Get Started

Core Data is a framework that you use to manage the **model** layer objects in your application.

### Step 1: define object model

Creating an **Entity** and Its Properties: add source file that will have the extension`.xcdatamodeld` for the Core Data model as part of the template.

An entity’s properties are its **attributes** and **relationships**, including its **fetched properties** \(if it has any\). Core Data supports **to-one** and **to-many** relationships, and fetched properties. Fetched properties represent weak, one-way relationships. Relationships are defined from one direction at a time. To create a many-to-many relationship, you would need to create two to-many relationships and then set them up as **inverses** of each other.

### Step 2: initialize the Core Data stack

The Core Data stack is a collection of framework objects that mediate between the objects in your application and external data stores:

* NSManagedObjectContext
* NSPersistentStoreCoordinator, 
* NSManagedObjectModel
* NSPersistentContainer

Starting in iOS 10 and macOS 10.12, the **NSPersistentContainer** handles the creation of the Core Data stack and offers access to the NSManagedObjectContext as well as a number of convenience methods.

Whereas the **NSManagedObjectModel** defines the structure of the data, the **NSPersistentStoreCoordinator** realizes objects from the data in the persistent store and passes those objects off to the requesting **NSManagedObjectContext**, the one that is exposed to the rest of your application.

### Step 3: create and save objects

```swift
func addEmployee() {
    let context = persistentContainer.newBackgroundContext()
    let employee = NSEntityDescription.insertNewObject(forEntityName: "Employee", into: context) as! Employee
    // set employee's properties
    do {
        try context.save()
    } catch {
        fatalError("Failure to save context: \(error)")
    }
}
```

### Step 4: fetching objects

```swift
let employeesFetch = NSFetchRequest(entityName: "Employee")
do {
    let fetchedEmployees = try moc.executeFetchRequest(employeesFetch) as! [Employee]
} catch {
    fatalError("Failed to fetch employees: \(error)")
}
```

You can add an **NSPredicate** object to the fetch request to narrow the number of objects being returned.

## Integrate with iOS

When you use Core Data with a UITableView-based layout, the **NSFetchedResultsController** for your data is typically initialized.

* act as the table view's data source
* communicate the data changes to the table view
* add sections
* caches

