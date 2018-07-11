# Core Data

Use [Core Data](https://developer.apple.com/documentation/coredata) to manage the **model** layer objects in your application. It provides generalized and automated solutions to common tasks associated with object lifecycle and object graph management, including persistence.

Core Data reduces the complexity of creating and maintaining the model layer of your app. By using Core Data to define your data structures, you can remove most of the repetitive code that is typically required.

You perform the initial integration of Core Data into your app by either creating the traditional Core Data stack \([`NSManagedObjectModel`](https://developer.apple.com/documentation/coredata/nsmanagedobjectmodel), [`NSPersistentStoreCoordinator`](https://developer.apple.com/documentation/coredata/nspersistentstorecoordinator), and at least one instance of [`NSManagedObjectContext`](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext)\) or by initializing a `NSPersistentContainer`. 

```swift
lazy var persistentContainer: NSPersistentContainer = {
    let container = NSPersistentContainer(name: "whatsnew")
    container.loadPersistentStores(completionHandler: { (storeDescription, error) in
    if let error = error as NSError? {
        fatalError("Unresolved error \(error), \(error.userInfo)")
    }
    })
    return container
}()
```

You use instances of [`NSManagedObject`](https://developer.apple.com/documentation/coredata/nsmanagedobject) as data structures, and you define the relationships between those objects using the data model.

A managed object is associated with an entity description \(an instance of `NSEntityDescription`\) and with a managed object context that tracks changes to the object graph. 

`NSManagedObject` provides support for a range of common types. Sometimes, however, you want to use types that are not supported directly. For example, in a graphics application you might want to define a Rectangle entity that has attributes color and bounds that are an instance of `NSColor` and an `NSRect` struct respectively. For some types you can use a transformable attribute, for others this may require you to create a subclass of `NSManagedObject`.

Managed objects typically represent data held in a persistent store. In some situations a managed object may be a “**fault**”—an object whose property values have not yet been loaded from the external data store. When you access persistent property values, the fault “fires” and the data is retrieved from the store automatically. This can be a comparatively expensive process \(potentially requiring _a round trip to the persistent store_\), and you may wish to avoid unnecessarily **firing a fault**. For example, since `isEqual` and `hash` do not cause a fault to fire, managed objects can typically be placed in collections without firing a fault.

An instance of [`NSFetchRequest`](https://developer.apple.com/documentation/coredata/nsfetchrequest) collects the criteria needed to select held in a persistent store. A fetch request must contain an entity description \(an instance of `NSEntityDescription`\) or an entity name that specifies which entity to search. It frequently also contains a predicate \(an instance of `NSPredicate`\) that specifies which properties to filter, and an array of sort descriptors \(instances of `NSSortDescriptor`\) that specify how the returned objects should be ordered.

```swift
let fetchRequest = NSFetchRequest<NSFetchRequestResult>(entityName: "Ticket")
let predicate = NSPredicate(format: "ID > 50")
fetchRequest.predicate = predicate
```

You often predefine fetch requests in a managed object model. [`NSManagedObjectModel`](https://developer.apple.com/documentation/coredata/nsmanagedobjectmodel)provides an API to retrieve a stored fetch request by name. Stored fetch requests can include placeholders for variable substitution, and so serve as templates for later completion.

Core Data is designed to work in a **multithreaded** environment. However, not every object under the Core Data framework is thread safe. In general, avoid doing data processing on the main queue that is not user-related since data processing can be CPU-intensive. Do not pass `NSManagedObject` instances between queues.

