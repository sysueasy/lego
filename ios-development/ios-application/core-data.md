# Core Data

Use [Core Data](https://developer.apple.com/documentation/coredata) to manage the **model** layer objects in your application. It provides generalized and automated solutions to common tasks associated with object lifecycle and object graph management, including persistence.

You perform the initial integration of Core Data into your app by either creating the traditional Core Data stack or by initializing a [`NSPersistentContainer`](https://developer.apple.com/documentation/coredata/nspersistentcontainer), a container that encapsulates the Core Data stack in your application.

```swift
lazy var persistentContainer: NSPersistentContainer = {
    let container = NSPersistentContainer(name: "DataModel")
    container.loadPersistentStores(completionHandler: { (storeDescription, error) in
        if let error = error as NSError? {
            fatalError("Unable to load persistent stores: \(error)")
        }
    })
    return container
}()
```

If you decide not to use an `NSPersistentContainer` object, you need to initialize an instance of `NSManagedObjectModel`, `NSPersistentStoreCoordinator`, and at least one instance of `NSManagedObjectContext`.

The [`NSManagedObjectModel`](https://developer.apple.com/documentation/coredata/nsmanagedobjectmodel) is a programmatic representation of the managed object model that is created and maintained in the model editor of Xcode \(.xcdatamodeld\). The model contains one or more `NSEntityDescription` objects representing the entities in the schema.

If you want Core Data to persist your data model to disk, you will need to inform the [`NSPersistentStoreCoordinator`](https://developer.apple.com/documentation/coredata/nspersistentstorecoordinator) of where you want the file to reside and what format you want to use.

[`NSManagedObjectContext`](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext) is an object representing a single object space or scratch pad that you use to fetch, create, and save managed objects. Changes to managed objects are held in memory, in the associated context, until that context is saved to one or more persistent stores.

You use instances of [`NSManagedObject`](https://developer.apple.com/documentation/coredata/nsmanagedobject) as data structures, and you define the relationships between those objects using the data model. A managed object is associated with an entity description \(an instance of `NSEntityDescription`\) and with a managed object context that tracks changes to the object graph. If you instantiate a managed object directly, you must call the designated initializer \([`init(entity:insertInto:)`](https://developer.apple.com/documentation/coredata/nsmanagedobject/1506357-init)\).

Managed objects typically represent data held in a persistent store. In some situations a managed object may be a “**fault**”—an object whose property values have not yet been loaded from the external data store. When you access persistent property values, the fault “fires” and the data is retrieved from the store automatically. This can be a comparatively expensive process \(potentially requiring _a round trip to the persistent store_\), and you may wish to avoid unnecessarily **firing a fault**. For example, since `isEqual` and `hash` do not cause a fault to fire, managed objects can typically be placed in collections without firing a fault.

An instance of [`NSFetchRequest`](https://developer.apple.com/documentation/coredata/nsfetchrequest) collects the criteria needed to select held in a persistent store. A fetch request must contain an entity description \(an instance of `NSEntityDescription`\) or an entity name that specifies which entity to search. It frequently also contains a predicate \(an instance of `NSPredicate`\) that specifies which properties to filter, and an array of sort descriptors \(instances of `NSSortDescriptor`\) that specify how the returned objects should be ordered.

```swift
let fetchRequest = NSFetchRequest<NSFetchRequestResult>(entityName: "Ticket")
let predicate = NSPredicate(format: "ID > 50")
fetchRequest.predicate = predicate
```

You often predefine fetch requests in a managed object model. [`NSManagedObjectModel`](https://developer.apple.com/documentation/coredata/nsmanagedobjectmodel)provides an API to retrieve a stored fetch request by name. Stored fetch requests can include placeholders for variable substitution, and so serve as templates for later completion.

Core Data is designed to work in a **multithreaded** environment. However, not every object under the Core Data framework is thread safe. In general, avoid doing data processing on the main queue that is not user-related since data processing can be CPU-intensive. Do not pass `NSManagedObject` instances between queues.

