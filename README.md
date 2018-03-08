# openapi-service

This service acts as an example for how to resolve OpenAPI fragments that can be resolved in a jar.

### Issue

In creating this example, I discovered that resolving JSON references is not working as expected. The references are as follows:

```
api.json (this project)
 -> Persons.json (jar)
   -> Person.json (same jar) 
```

When the parser resolves the references, it is able to resolve the reference from api.json to Persons.json. The reference from Persons.json to Person.json generates an unexpected error:
 
```
[ERROR] unable to read
java.net.MalformedURLException: no protocol: /Person.json
	at java.net.URL.<init>(URL.java:593)
	at java.net.URL.<init>(URL.java:490)
	at java.net.URL.<init>(URL.java:439)
	at io.swagger.parser.util.RemoteUrl.urlToString(RemoteUrl.java:98)
	at io.swagger.parser.util.RefUtils.readExternalRef(RefUtils.java:133)
	at io.swagger.parser.ResolverCache.loadRef(ResolverCache.java:112)
	at io.swagger.parser.processors.ExternalRefProcessor.processRefToExternalDefinition(ExternalRefProcessor.java:37)
	at io.swagger.parser.processors.ExternalRefProcessor.processRefProperty(ExternalRefProcessor.java:134)
	at io.swagger.parser.processors.ExternalRefProcessor.processRefToExternalDefinition(ExternalRefProcessor.java:120)
	at io.swagger.parser.processors.PropertyProcessor.processRefProperty(PropertyProcessor.java:34)
	at io.swagger.parser.processors.PropertyProcessor.processProperty(PropertyProcessor.java:21)
	at io.swagger.parser.processors.ResponseProcessor.processResponse(ResponseProcessor.java:21)
	at io.swagger.parser.processors.OperationProcessor.processOperation(OperationProcessor.java:45)
	at io.swagger.parser.processors.PathsProcessor.processPaths(PathsProcessor.java:101)
	at io.swagger.parser.SwaggerResolver.resolve(SwaggerResolver.java:50)
	at io.swagger.parser.SwaggerParser.read(SwaggerParser.java:67)
```

### To reproduce

```
# Clone and install the dependent jar that contains the OpenAPI fragments
git clone https://github.com/douglasbgray/openapi-fragments.git 
cd openapi-fragments
mvn -U clean install

# Clone and build the service project
git clone https://github.com/douglasbgray/openapi-service.git
cd openapi-service
mvn -U clean compile
```    
