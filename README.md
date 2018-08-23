# openapi-service

This service acts as an example for how to resolve OpenAPI fragments that can be resolved in a jar.

### Versions

This is for OpenAPI 3, using swagger parser 2.0.2

### Issue

In creating this example, I discovered that resolving references is not working as expected. The references are as follows:

```
api.yaml (this project)
 -> persons.yaml (jar)
   -> person.yaml (same jar) 
```
e
When the parser resolves the references, it is able to resolve the reference from api.yaml to persons.yaml. The reference from persons.yaml to person.yaml generates an unexpected error:
 
```
[ERROR] unable to read
java.net.MalformedURLException: no protocol: /person.yaml
        at java.net.URL.<init>(URL.java:593)
        at java.net.URL.<init>(URL.java:490)
        at java.net.URL.<init>(URL.java:439)
        at io.swagger.v3.parser.util.RemoteUrl.urlToString(RemoteUrl.java:112)
        at io.swagger.v3.parser.util.RefUtils.readExternalRef(RefUtils.java:156)
        at io.swagger.v3.parser.ResolverCache.loadRef(ResolverCache.java:116)
        at io.swagger.v3.parser.processors.ExternalRefProcessor.processRefToExternalSchema(ExternalRefProcessor.java:47)
        at io.swagger.v3.parser.processors.ExternalRefProcessor.processRefSchema(ExternalRefProcessor.java:678)
        at io.swagger.v3.parser.processors.ExternalRefProcessor.processRefToExternalSchema(ExternalRefProcessor.java:169)
        at io.swagger.v3.parser.processors.SchemaProcessor.processReferenceSchema(SchemaProcessor.java:201)
        at io.swagger.v3.parser.processors.SchemaProcessor.processPropertySchema(SchemaProcessor.java:102)
        at io.swagger.v3.parser.processors.SchemaProcessor.processSchemaType(SchemaProcessor.java:53)
        at io.swagger.v3.parser.processors.SchemaProcessor.processSchema(SchemaProcessor.java:38)
        at io.swagger.v3.parser.processors.ComponentsProcessor.processSchemas(ComponentsProcessor.java:221)
        at io.swagger.v3.parser.processors.ComponentsProcessor.processComponents(ComponentsProcessor.java:71)
        at io.swagger.v3.parser.OpenAPIResolver.resolve(OpenAPIResolver.java:50)
        at io.swagger.v3.parser.OpenAPIV3Parser.readLocation(OpenAPIV3Parser.java:53)
        at io.swagger.parser.OpenAPIParser.readLocation(OpenAPIParser.java:19)
        at io.swagger.codegen.config.CodegenConfigurator.toClientOptInput(CodegenConfigurator.java:452)
        at io.swagger.codegen.plugin.CodeGenMojo.execute(CodeGenMojo.java:512)
        at org.apache.maven.plugin.DefaultBuildPluginManager.executeMojo(DefaultBuildPluginManager.java:134)
        at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:207)
        at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:153)
        at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:145)
        at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:116)
        at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:80)
        at org.apache.maven.lifecycle.internal.builder.singlethreaded.SingleThreadedBuilder.build(SingleThreadedBuilder.java:51)
        at org.apache.maven.lifecycle.internal.LifecycleStarter.execute(LifecycleStarter.java:128)
        at org.apache.maven.DefaultMaven.doExecute(DefaultMaven.java:307)
        at org.apache.maven.DefaultMaven.doExecute(DefaultMaven.java:193)
        at org.apache.maven.DefaultMaven.execute(DefaultMaven.java:106)
        at org.apache.maven.cli.MavenCli.execute(MavenCli.java:863)
        at org.apache.maven.cli.MavenCli.doMain(MavenCli.java:288)
        at org.apache.maven.cli.MavenCli.main(MavenCli.java:199)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced(Launcher.java:289)
        at org.codehaus.plexus.classworlds.launcher.Launcher.launch(Launcher.java:229)
        at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode(Launcher.java:415)
        at org.codehaus.plexus.classworlds.launcher.Launcher.main(Launcher.java:356)
[WARNING] Exception while reading:
java.lang.RuntimeException: Unable to load URL ref: /person.yaml path: /projects/pdx/external/openapi-service/src/main/resources
        at io.swagger.v3.parser.util.RefUtils.readExternalRef(RefUtils.java:183)
        at io.swagger.v3.parser.ResolverCache.loadRef(ResolverCache.java:116)
        at io.swagger.v3.parser.processors.ExternalRefProcessor.processRefToExternalSchema(ExternalRefProcessor.java:47)
        at io.swagger.v3.parser.processors.ExternalRefProcessor.processRefSchema(ExternalRefProcessor.java:678)
        at io.swagger.v3.parser.processors.ExternalRefProcessor.processRefToExternalSchema(ExternalRefProcessor.java:169)
        at io.swagger.v3.parser.processors.SchemaProcessor.processReferenceSchema(SchemaProcessor.java:201)
        at io.swagger.v3.parser.processors.SchemaProcessor.processPropertySchema(SchemaProcessor.java:102)
        at io.swagger.v3.parser.processors.SchemaProcessor.processSchemaType(SchemaProcessor.java:53)
        at io.swagger.v3.parser.processors.SchemaProcessor.processSchema(SchemaProcessor.java:38)
        at io.swagger.v3.parser.processors.ComponentsProcessor.processSchemas(ComponentsProcessor.java:221)
        at io.swagger.v3.parser.processors.ComponentsProcessor.processComponents(ComponentsProcessor.java:71)
        at io.swagger.v3.parser.OpenAPIResolver.resolve(OpenAPIResolver.java:50)
        at io.swagger.v3.parser.OpenAPIV3Parser.readLocation(OpenAPIV3Parser.java:53)
        at io.swagger.parser.OpenAPIParser.readLocation(OpenAPIParser.java:19)
        at io.swagger.codegen.config.CodegenConfigurator.toClientOptInput(CodegenConfigurator.java:452)
        at io.swagger.codegen.plugin.CodeGenMojo.execute(CodeGenMojo.java:512)
        at org.apache.maven.plugin.DefaultBuildPluginManager.executeMojo(DefaultBuildPluginManager.java:134)
        at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:207)
        at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:153)
        at org.apache.maven.lifecycle.internal.MojoExecutor.execute(MojoExecutor.java:145)
        at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:116)
        at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject(LifecycleModuleBuilder.java:80)
        at org.apache.maven.lifecycle.internal.builder.singlethreaded.SingleThreadedBuilder.build(SingleThreadedBuilder.java:51)
        at org.apache.maven.lifecycle.internal.LifecycleStarter.execute(LifecycleStarter.java:128)
        at org.apache.maven.DefaultMaven.doExecute(DefaultMaven.java:307)
        at org.apache.maven.DefaultMaven.doExecute(DefaultMaven.java:193)
        at org.apache.maven.DefaultMaven.execute(DefaultMaven.java:106)
        at org.apache.maven.cli.MavenCli.execute(MavenCli.java:863)
        at org.apache.maven.cli.MavenCli.doMain(MavenCli.java:288)
        at org.apache.maven.cli.MavenCli.main(MavenCli.java:199)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced(Launcher.java:289)
        at org.codehaus.plexus.classworlds.launcher.Launcher.launch(Launcher.java:229)
        at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode(Launcher.java:415)
        at org.codehaus.plexus.classworlds.launcher.Launcher.main(Launcher.java:356)
Caused by: java.net.MalformedURLException: no protocol: /person.yaml
        at java.net.URL.<init>(URL.java:593)
        at java.net.URL.<init>(URL.java:490)
        at java.net.URL.<init>(URL.java:439)
        at io.swagger.v3.parser.util.RemoteUrl.urlToString(RemoteUrl.java:112)
        at io.swagger.v3.parser.util.RefUtils.readExternalRef(RefUtils.java:156)
        ... 37 more
```

### To reproduce

```
# Clone and install the dependent jar that contains the OpenAPI fragments
git clone https://github.com/douglasbgray/openapi-fragments.git 
git checkout openapi3
cd openapi-fragments
mvn -U clean install

# Clone and build the service project
git clone https://github.com/douglasbgray/openapi-service.git
git checkout openapi3
cd openapi-service
mvn -U clean compile
```    
