# testOrm_Mysql
Creata table:
```
CREATE TABLE `body` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `description` text,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=292 DEFAULT CHARSET=utf8

CREATE TABLE `telephones` (
  `id_telephones` int(11) NOT NULL AUTO_INCREMENT,
  `id_body` int(11) NOT NULL,
  `name` varchar(100) DEFAULT NULL,
  `date` datetime DEFAULT NULL,
  PRIMARY KEY (`id_telephones`)
) ENGINE=InnoDB AUTO_INCREMENT=164 DEFAULT CHARSET=utf8

CREATE TABLE `telephones` (
  `id_telephones` int(11) NOT NULL AUTO_INCREMENT,
  `id_body` int(11) NOT NULL,
  `name` varchar(100) DEFAULT NULL,
  `date` datetime DEFAULT NULL,
  PRIMARY KEY (`id_telephones`)
) ENGINE=InnoDB AUTO_INCREMENT=164 DEFAULT CHARSET=utf8

CREATE TABLE `table_1` (
  `id_table_1` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(200) DEFAULT NULL,
  PRIMARY KEY (`id_table_1`)
) ENGINE=InnoDB AUTO_INCREMENT=29351384 DEFAULT CHARSET=utf8

CREATE TABLE `testimage` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `image` longblob,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```
creation projections
```csharp
    [MapTableName("`testimage`")]
    class TestImage
    {
        [MapPrimaryKey("`id`", Generator.Native)]
        public int Id { get; set; }

        [MapColumnName("`image`")]
        public Image image { get; set; }
    }
    [MapTableName("body", "body.id > 0")]
    public class Body
    {
        [MapBaseKey]
        [MapPrimaryKey("id", Generator.Native)]
        public Int32 Id { get; set; }

        [MapColumnNameAttribute("description")]
        public string Description { get; set; }
    }
```
```csharp
    [MapTableJoin("inner")]
    [MapTableName("telephones", "telephones.id_body > 0")]
    public class Telephone : Body, IErrorDal<Telephone>, IValidateDal<Body>,IActionDal<Body>
    {
        [MapPrimaryKey("id_telephones", Generator.Native)]
        public Int32 IdTelephone { get; set; }

        [MapForeignKey]
        [MapColumnName("id_body")]
        public Int32 IdBody { get; set; }

        [MapColumnNameAttribute("`name`")]
        public string Name { get; set; }

        [MapColumnNameAttribute("`date`")]
        public DateTime Date { get; set; }
        public void ErrorDal(Telephone currentObject, Exception exception)
        {
            throw new NotImplementedException();
        }
        public void Validate(Body type) {}
        public void BeforeInsert(Body item){}
        public void AfterInsert(Body item){}
        public void BeforeUpdate(Body item) {}
        public void AfterUpdate(Body item){}
        public void BeforeDelete(Body item){}
        public void AfterDelete(Body item){}
    }
```
```csharp
    [MapTableName("`table_1`")]
    public class Table1
    {
        [MapPrimaryKeyAttribute("`id_table_1`", Generator.Assigned)]
        public Int32 Id { get; set; }

        [MapColumnNameAttribute("`name`")]
        public string Description { get; set; }
    }
```
initialization orm
 ```csharp
   new Configure(
                "Server=localhost;Database=test;Uid=root;Pwd=;charset=utf8;Allow User Variables=True;",
                ProviderName.MySql,
                true, // write log
                "D:/assa22.txt",//log file path
                true//using cache2 level
                );
  ```
  get session;
  !!when using a multi-thread,  create a session for each thread
```csharp
ISession ses = Configure.GetSessionCore();
```
work session;
add rows;
```csharp
 ses.Querion<Telephone>().Delete(a=>a.Date!=null);

   for (int i = 0; i < 10; i++)
   {
     Telephone t = new Telephone {Date = new DateTime(), Description = "simple", Name = "ss" + i};
     ses.Save(t);
   }
```
```csharp
   var t27 = ses.Querion<Telephone>().OrderBy(w => w.Description).ThenBy(s => s.IdTelephone).LastOrDefault(s => s.Description != null);
   var t28 = ses.Querion<Telephone>().OrderBy(a => a.Description == null && a.Description == "asad").LastOrDefault(a => a.Description != null);
   var t29 = ses.Querion<Telephone>().OrderBy(a => a.Description == null && a.Description == "asad").FirstOrDefault(a => a.Description != null);
   var t30 = ses.Querion<Telephone>().OrderByDescending(a => a.Description).ToList().FirstOrDefault(d => d.Description == null);
   var t31 = ses.Querion<Telephone>().Last(dd => dd.Description == null);
   var t32 = ses.Querion<Telephone>().OrderByDescending(a => a.Description).ToList().LastOrDefault(d => d.Description == null);
   var t35 = ses.Querion<Telephone>().All(a => a.Description != null || a.Description == null);
   var t36 = ses.Querion<Telephone>().All(a => a.Description == "dsaas");
   var t37 = ses.Querion<Telephone>().All(a => a.Id < 60000000);

   var t38 = ses.Querion<Telephone>().Any(a => a.Description != null || a.Description == null);
   var t39 = ses.Querion<Telephone>().Any(a => a.Description == "dsaas");
   var t40 = ses.Querion<Telephone>().Any(a => a.Id < 60000000);
   var t41 = ses.Querion<Telephone>().Where(s => s.Id > 0).Count(d => d.Description == null);
   var t42 = ses.Querion<Telephone>().Where(s => s.Description != null).Select(a => a.Description.Length).ToList();
   var t43 = ses.Querion<Telephone>().Select(a => a.Description.Length + a.Id).ToList();
   var t44 = ses.Querion<Telephone>().Select(a => a.Description.Length).ToList();
   var t45 = ses.Querion<Telephone>().Where(s => s.Description != null).Select(a => new { dd = a.Description.Length + a.Id, f = a.IdBody }).ToList();
   var t456 =
       ses.Querion<Telephone>()
          .Where(s => s.Description == "ttttttttt")
          .Select(a => new { dd = a.Description.Length + a.Id, f = a.IdBody }).ToList(); 
```
  getting over cache,  not using cache 
```csharp
var telovercache = ses.Querion<Telephone>().OverCache().Where(a => a.Date != null);
```  
  call procedure 

```csharp
 var p1 = new ParameterStoredPr("p1", "table_name", ParameterDirection.Input);
 var p2 = new ParameterStoredPr("p2", 2, ParameterDirection.Output);
 var res = ses.ProcedureCallParam<Body>("MyProc;", p1, p2).ToList();
```
free sql
not inherit
```csharp
var t1 = ses.FreeSql<Body>("select * from body");
var t1 = ses.FreeSql<int>("select id from body");
```
inherit type
```csharp
var t1 = ses.FreeSql<Object>("select * from telephones");
var t2 = ses.FreeSql<String>("select name from telephones");
```
Working with image
```csharp
TestImage e = new TestImage();
e.image = Image.FromFile("D:/qq.jpg");
ses.Save(e);
List<TestImage> images = ses.GetList<TestImage>().ToList();
```
write to logfile
```csharp
ses.WriteLogFile("simple text");
```
close session, clear cache1 level
```csharp
 ses.Dispose();
```
using transaction, inner transaction, transaction scope
```csharp
  var tr= ses.BeginTransaction();
  tr.Commit();
  tr.Rollback();
```

clearing the cache of type Level1 and Level2
```csharp
ses.ClearCache<Telephone>();
```
