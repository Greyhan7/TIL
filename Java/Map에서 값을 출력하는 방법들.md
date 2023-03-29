# Map에서 값을 출력하는 방법들

## entrySet() : key와 value를 모두 출력
```java
Map<String, String> map = new HashMap<>();
map.put("key1","value1");
map.put("key2","value2");
map.put("key3","value3");

for(Map.Entry<String, String> entry : map.entrySet()){
        System.out.println("key : " + entry.getKey() + ",  value :" + entry.getValue());
}
```

```
결과값

key : key1,  value :value1
key : key2,  value :value2
key : key3,  value :value3

```


## keySet() : key 값만 출력
```java
Map<String, String> map = new HashMap<>();
map.put("key1","value1");
map.put("key2","value2");
map.put("key3","value3");

for(String key : map.keySet()){
    String value = map.get(key);
    System.out.println("key :" + key + ", value : " + value);
}
```

```
결과값

key : key1,  value :value1
key : key2,  value :value2
key : key3,  value :value3

```


## Iterator : 컬력센에 저장되어 있는 요소들 읽기
```java
Map<String, String> map = new HashMap<>();
map.put("key1","value1");
map.put("key2","value2");
map.put("key3","value3");

Iterator<String> iteratorK = map.keySet().iterator();
        while (iteratorK.hasNext()) {
            String key = iteratorK.next();
            String value = map.get(key);
            System.out.println("key:" + key + ", value: " + value);
        }

```


```
결과값

key : key1,  value :value1
key : key2,  value :value2
key : key3,  value :value3

```

## forEach 활용
```java
Map<String, String> map = new HashMap<>();
map.put("key1","value1");
map.put("key2","value2");
map.put("key3","value3");

map.forEach((key, value) -> {
	System.out.println("key: " + key + ", value: " + value);
});


```


```
결과값

key : key1,  value :value1
key : key2,  value :value2
key : key3,  value :value3

```

