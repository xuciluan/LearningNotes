## map的使用

### 1、getOrElse
用来返回默认值，使用如下：
```kotlin
val config = HashMap<String,Boolean>()
val value = config.getOrElse("haha",{false})
```

### 2、getOrPut
这个是用来解决以下情况：
```java
if(config.get("key") == null){
   List list = new ArrayList<String>();
   config.put("key",list);
}
config.get("key").add("haha");
```

如今用kotlin可以直接这么写：
```kotlin
val config = HashMap<String,List<String>>()
config.getOrPut("key",createdList).add("haha")
```

### 3、withDefault
```kotlin
val config = HashMap<String,Boolean>().withDefault { false }
val value1 = config["key2"] //返回null，并不会返回false
val value2 = config.getValue("key3") //返回false
```