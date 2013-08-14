ios-circlesio-sdk
=================


### Circles object

To begin using the SDK you must first create a Circles object. When creating the Circles object you must enter your circles.io url and your token.

```
Circles* circlesObject = [[Circles alloc] initWithURL:@"your-url.circles.io" Version:@"version" andToken:@"your-token"]; 
```
The delegate must be set in order for the view controller to receive the response from any api call. 

` [circlesObject setCirclesDelegate:viewController]; `

Select the header file and make sure that the view controller is also setup as a CirclesDelegate.

`@interface yourViewController : UIViewController <CirclesDelegate>`

### API Calls

After creating the circles object, you are now able to use any of the following methods.

#### Create

There are two types of methods to create. One method takes in a NSDictionary and the other takes in json data passed as a NSString. You must pass the data collection name you want to create and the data.

```
-(void) Create:(NSString *)collection withDictionary:(NSDictionary *) dict;
-(void) create:(NSString *) collection WithJSON:(NSString *) jsonData;
```
Creating the new object will return that newly created object on success.

__Example:__ Using NSDictionary and json to create a new item with the "title" : "test_item" and "group" : "main group" 

```
//Creating using NSMutableDictionary 
NSMutableDictionary* data = [[NSMutableDictionary alloc] init];
[data setValue:@"test_item" forKey:@"title"];
[data setValue:@"main group" forKey:@"group"];
[circlesObject Create:@"item" withDictionary:data];

//Creating using json
//Note that you must enclose you json in {} when creating
 NSString* json = @"{\"title\" : \"test_item\" , \"group\" : \"main group\"}";
[circlesObject Create:@"item" WithJSON:json];
```
#### Read

For reading in data there are four types of methods to use. 

```
-(void) ReadAll:(NSString *) collection;
-(void) Read:(NSString *) collection withID:(NSString *) identifier;
-(void) Read:(NSString *)collection WithDictionary:(NSDictionary *) dict;
-(void) Read:(NSString *) collection withQuery:(NSString *)jsonString;
```
ReadAll takes in the data collection type as a NSString and returns all data from that collection. An example is reading all data that are items:

__Example:__ `[circlesObject ReadAll:@"item"]`

You can also read for an object with a specific id using:

__Example:__ `[circlesObject Read:@"item" withID:@"123456789"]`

This will return the item with the id "123456789".

Reads can also be made by using queries. For example you can search an all the items that belong to a certain group named "main group".

__Example:__ `[circlesObject Read:@"item" withQuery:@" \"group\" : \"main group\" "];`

You have to put add \" \" around fields and string type values

Making any read call will return a NSDictionary that contains all the data in the following format:

```
{
    collection = item;
    error = "<null>";
    length = 1;
    options = {
        limit = 20;
        skip = 0;
    };
    query =     {
    };
    request = read;
    result =({
            "__v" = 0;
            "_id" = 520529c769691fc94a000003;
            body = {};
            date = "2013-10-12T23:00:00.000Z";
            geo = ("40.836022","-73.926315");
            group = "main group";
            permission = ();
            tags = ();
            title = "test_item";
        });
}
``` 
#### Update

To update information you can use the following four methods:

```
-(void) Update:(NSString *)collection WithDictionary:(NSDictionary *) dict;
-(void) Update:(NSString *)collection WithKey:(NSString *) key Value:(NSString *)value forObjectID:(NSString *) objectID;
-(void) Update:(NSString *)collection WithKey:(NSString *) key Value:(NSString *)value withQuery:(NSString *) query;
-(void) UpdateAll:(NSString *) collection withModel:(NSString *) model;
```
__Example:__ 

```
```
#### Delete

There are two methods for deleting data. One method takes in the id of the object you want to delete. The other method takes in a query as a NSString which specifies which objects to delete.

```
-(void) Delete:(NSString *) collection withID:(NSString *) identifier;
-(void) Delete:(NSString *) collection withQuery:(NSString *) query;
```

__Example:__

```
//This will delete the item with id "123456789"
[circlesObject Delete:@"item" withID:@"123456789"]

//This will delete all items from the group "main group"
[circlesObject Delete:@"item" withQuery:@" \"group\" : \"main group\" "]
```
### Delegate Methods

There are six delegate methods that are called in response to any of the api calls. Three are for user authorization while the other three are for handling the response from create, read, update and delete.

Delegate methods for user authorization.

```
-(void) OAuthDidReceiveResponse:(NSDictionary *) response;
-(void) OAuthResponseFailure:(NSString *)error ErrorCode:(int)statusCode;
-(void) OAuthConncectionFailed:(NSError *)error;
```

`-(void) OAuthDidReceiveResponse:(NSDictionary *) response;` is only called when circles object is finished recieving the user's data. This is where you handle the user's data in the form of a NSDictionary.

`-(void) OAuthResponseFailure:(NSString *)error ErrorCode:(int)statusCode;` is only called when there is a failure after making a api call. An error and error code is sent. This method is for handling any kind of errors.

`-(void) OAuthConncectionFailed:(NSError *)error;` is only called when the circles object fails to connect.

Delegate methods for regular api calls.

```
-(void) didRecieveResponse:(NSDictionary *) dictionary;
-(void) ResponseFailure:(NSString *)error ErrorCode:(int)statusCode;
-(void) conncectionFailed:(NSError *)error;
```
`-(void) didReceiveResponse:(NSDictionary *) response;` is only called when circles object is finished recieving data. This is where you handle the data in the form of a NSDictionary.

`-(void) ResponseFailure:(NSString *)error ErrorCode:(int)statusCode;` is only called when there is a failure after making a api call. An error and error code is sent. This method is for handling any kind of errors.

`-(void) conncectionFailed:(NSError *)error;` is only called when the circles object fails to connect.
