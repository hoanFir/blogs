[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)

||Object|Map|
|-|-|-|
|Accidental Keys|An Object has a prototype, so it contains default keys that could collide with your own keys if you're not careful|does not contain any keys by default, it only contains what is explicitly put into it|
|Key Types|The keys of an Object must be either a String or a Symbol|can be any value, including functions, objects, or any primitive|
|Key Order|The keys of an Object are not ordered|The keys in Map are ordered|
|Size|The number of items in an Object must be determined manually|is easily retrieved from its size property|
|Iteration|Iterating over an Object requires obtaining it keys in some fashion and iterating over them|A Map is an iterable, so it can be directly iterated|
|Performance|Not optimized for frequent additions and removals of key-value pairs(没有针对键值对的频繁增删进行优化)|对于键值对的频繁增删性能更好|
