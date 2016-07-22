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
```
creation projections
```csharp
    [MapTableName("`testimage`")]
    class TestImage
    {
        [MapPrimaryKey("`id`", Generator.Native)]
        public int Id { get; set; }

        [MapColumnName("`image`")]
        public byte[] Image { get; set; }
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
```csharp

```
```csharp

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
