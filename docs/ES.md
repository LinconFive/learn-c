```js
// Use this to give interviewees short questions
```


### [Python]lambda print calculation
```
python3 -c 'x = lambda a, b : a * b; print(x(5,6))'
```

### [Python]dump good view of dict, or dict in dict
```
import json
print(json.dumps(model.instance_infos, sort_keys=False, indent=4))
```

### [Python]split string into groups
```
string='7fffffffffffffff'
[string[i:i+4] for i in range(0, len(string), 4)]
```

### [Python]print in hex
```
i=10
print(hex(i)[2:])
# convert to upper, lower
'a'.upper()

```

### [Python]break in newline
```
for i in a.split(): print(i, end="\n")

# write needs \n
f = open("cpu0.reg", "w")
for i in r:
    #print(dir(i))
    #print(i.name.ljust(40), i.symbols, i.type, i.description)
    f.write(i.name)
f.write("\n")

f.close()

```

### [Bash]Make 99 Table
```
#!/bin/bash
#print 99 table
for Row in {1..9};do
 for Column in `seq $Row`;do
  echo -ne "${Column}x${Row}=$[$Row*$Column]\t"
 done
 echo
done
```

<a href="index.md">Back to home</a>
