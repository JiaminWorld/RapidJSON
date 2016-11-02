# RapidJSON

# Parse String to DOM
 
   Document d;
   
   d.Parse(string);

# Gernerate JSON in DOM

    // Define the document as an object rather than an array
    d.SetObject();

    // Create a rapidjson array type
    Value array(kArrayType);

    // must pass an allocator when the object may need to allocate memory
    Document::AllocatorType& allocator = d.GetAllocator();

    // add element to array 
    // ["hello","world","Mia"]
    array.PushBack("hello",allocator).PushBack("world",allocator).PushBack("Mia",allocator);

    // add array to DOM
    // {"array":["hello","world","Mia"]}
    d.AddMember("array",array,allocator);
    
    // Create a rapidjson object type
    Value object(kObjectType);

    // Add value to object
    // {"hello","roy"}
    object.AddMember("hello","roy",allocator);

    // Add object to DOM
    d.AddMember("object",object,allocator);

# Get part of JSON 

    // Get elements in a array
    Value& a = d["array"];
    for(SizeType i=0;i<a.Size();i++){
        std::cout << a[i].GetString() << std::endl;
    }
    
    // Get value of object
    d["object"]["hello"].GetString();
    
    // If object'value is a nested json object or array, parse it into a document then convert it to string
    Value& nest_json = d["object"];
    StringBuffer sb;
    Writer<StringBuffer> writer(sb);
    nest_json.Accept(writer);
    sb.GetString();
    
# Remove member

    d.RemoveMember("object");

# Modify JSON

    // modify value of object 
    d["object"]["hello"] = "Mia";

    // add member to object
    d["object"].AddMember("hello1","Roy1",allocator);

    // modify value of array
    d["array"][SizeType(0)] = "test";

    // add member to array
    d["array"].PushBack("test",allocator);

# Convert Value to string

    StringBuffer buffer;
    Writer<StringBuffer> writer(buffer);
    Value& temp = d["object"];
    temp.Accept(writer);
    std::cout << buffer.GetString() << std::endl;

# Convert DOM to string

    StringBuffer buffer;
    Writer<StringBuffer> writer(buffer);
    d.Accept(writer);
    std::cout << buffer.GetString() << std::endl;
