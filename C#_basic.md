## 反射
- [vedio]()

- create object
```C#
Assembly assembly=Assembly.Load(Ruanmou.DB.SqlServer);
//  full path
Assembly assembly=Assembly.LoadFile(@"example.dll");
Type type=assembly.GetType("Ruanmou.DB.SqlServer.Singleton");
Singleton singleton4=(Singleton) Activator.CreateInstance(type,true);
object oReflectionTest= Activator.CreateInstance(type,new object[]{"123"});
```


- create class template variables
  
```C#
Assembly assembly=Assembly.Load(Ruanmou.DB.SqlServer);
Type type=assembly.GetType("Ruanmou.DB.SqlServer.GenericClass3");
Type newType=type.MakeGenericType(new Type[]{typeof(string),typeof(string),typeof(dateTime)})
object oGeneric=Activator.GreateInstance(newType)
```

- create function with diff variables

```C#
Assembly assembly=Assembly.Load(Ruanmou.DB.SqlServer);
Type type=assembly.GetType("Ruanmou.DB.SqlServer.ReflectionTest");
object oReflectionTest=Activator.GreateInstance(type)

// no para
MethodInfo method=type.GetMethod("show1", new Type[]{});
method.invoke(oReflectionTest,null);
method.invoke(oReflectionTest,new object[]{});
// dynamic para
MethodInfo method=type.GetMethod("show1", new Type[]{typeof(int),typeof(string)});
method.invoke(oReflectionTest,new object[]{4,"abc"});
```

## 工廠和DB的選擇使用

```xml
<appSettings>
    <add key="IDBHelperConfig" value="Ruanmou.DB.Mysql.MySqlHelper,Ruanmou.DB.Mysql">
</appSettings>
```

```C#
public class Factory
{
    // System.Configuration;
    private static string IDBHelperConfig=ConfigurationManager.AppSettings["IDBHelperConfig"];
    private static string DllName=IDBHelperConfig.Split(",")[1];
    private static string TypeName=IDBHelperConfig.Split(",")[0];
    public static IDBHelper CreateHelper()
    {
        Assembly assembly= Assembly.Load(DllName);
        foreach(var item in assembl.GetMethods)
        {
            Console.WriteLine(item.FullQualifiedName);
        }
        foreach(var item in assembl.GetTypes)
        {
            Console.WriteLine(item.FullName);
        }
        Type type=assembly.GetType(TypeName);
        IDBHelper iDBHelper=(IDBHelper) oDBHelper;
        // iDBHelper.Query();
        return iDBHelper;
    }
}

IDBHelper iDBHelper=Factory.CreateHelper();
iDBHelper.Query();
```


## 在database取得的people , 賦值給DTO

- Reflection/Property/Field
```C#
People people=new People();
People.Id=123;
People.Name="Lutte";
People.Desc="test";
//  begin reflection
Type type =typeof(People);
object oPeople=Activator.CreateInstance(type);
foreach(var prop in type.GetProperties())
{
    if (prop.Name.Equals("Id"))
    {
        prop.SetValue(oPeople,234)
    }
}
foreach(var field in type.GetFields())
{
    if (field.Name.Equals("Id"))
    {
        field.SetValue(oPeople,234)
    }
}

```

- 在database取得的people , 賦值給DTO

```C#
People people=new People();
People.Id=123;
People.Name="Lutte";
People.Desc="test";
//  begin reflection
Type typePeople =typeof(People);
Type typePeopleDTO =typeof(People);
object oPeople=Activator.CreateInstance(typePeopleDTO);
foreach(var prop in type.GetProperties())
{
    if (prop.Name.Equals("Id"))
    {
        object value=typePeople.GetProperty("Id").GetValue(people);
        prop.SetValue(peopleDTO,value);
    }
    // 或者以下方法, 循環 people.propertyName
    object value=typePeople.GetProperty(prop.Name).GetValue(people);
    rop.SetValue(peopleDTO,value);
}
```

## attribute


```C#

```