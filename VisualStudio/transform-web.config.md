### Transform web.config 

```console
1. Click Build -> Configuration Manager... -> Active solution configuration -> <New...>
2. Type "Test" as the value of Name, choose "Release" from the drop-down list of "Copy settings from", click "OK"
3. Right-click Web.config in the Solution Explorer, click "Add Config Transform", then Web.Test.config will be created automatically
4. Double-click Web.Test.config and change the value of connectionString for Test env
5. Right-click project ToiWhanauHealth, click "Publish..."
6. Edit Configuration, choose "Test - Any CPU", click "Save"
```

https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/iis/transform-webconfig?view=aspnetcore-3.1
