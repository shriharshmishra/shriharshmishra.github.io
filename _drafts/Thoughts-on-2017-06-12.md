## Tips and Tricks 

Recently, I had used a trick to run a unix command in each folder of a directory list. 

```
for d in ./*/ ; do (cd "$d" && mvn clean); done
```

The above command run `mvn clean` in each folder of the directory list defined by the path `./*/`.
