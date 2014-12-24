# Silverpop .NET API

This is a .NET API wrapper for [Silverpop](http://www.silverpop.com/) Transact XML email sending.

## Usage

**Prepare the client *(fill in the necessary details)***

```csharp
var configuration = new TransactClientConfiguration()
{
    PodNumber = your_pod_number,

    // Username and password are recommended, but can be omitted
    // if you only use HTTPS communication (non-batch message sending)
    // and specify OAuth information.

    Username = "username",
    Password = "password",

    // If wishing to use OAuth for HTTPS communication
    // provide the following:

    OAuthClientId = "00000000-0000-0000-0000-000000000000",
    OAuthClientSecret = "00000000-0000-0000-0000-000000000000",
    OAuthRefreshToken = "00000000-0000-0000-0000-000000000000"

    // OAuth is typically used when an application is hosted
    // somewhere with a non-static IP address (Azure Websites, etc.),
    // or when you don't want to specify the IP address with Silverpop.
};

var client = new TransactClient(configuration);
```

**Create a message**

```csharp
var message = new TransactMessage()
{
    CampaignId = "123456",
    TransactionId = "Test-" + Guid.NewGuid().ToString()),
    Recipients = new List<TransactMessageRecipient>()
    {
        new TransactMessageRecipient()
        {
            BodyType = TransactMessageRecipientBodyType.Html,
            EmailAddress = "user@example.com",
            PersonalizationTags = new Dictionary<string, string>()
            {
                { "tag1", "tag1-value" }
            }
        }
    }
};
```

**Send a message using the client *(1-10 recipients)***

```csharp
TransactMessageResponse response = client.SendMessage(message);
```

**Send a message using the client asynchronously *(1-10 recipients)***

```csharp
TransactMessageResponse response = await client.SendMessageAsync(message);
```

**Send a message batch using the client *(11 - 5,000 recipients)***

```csharp
IEnumerable<string> batchResponse = client.SendMessageBatch(message);
```

**Send a message batch using the client asynchronously *(11 - 5,000 recipients)***

```csharp
IEnumerable<string> batchResponse = await client.SendMessageBatchAsync(message);
```

**Get the status of a message batch**

```csharp
TransactMessageResponse response = client.GetStatusOfMessageBatch("a_filename_from_batchResponse.xml");
```

**Get the status of a message batch asynchronously**

```csharp
TransactMessageResponse response = await client.GetStatusOfMessageBatchAsync("a_filename_from_batchResponse.xml");
```

# License

MIT License
