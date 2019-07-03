# Bash Cheetsheet

## Get file extension
```
filename=$(basename -- "$fullname")
extension="${filename##*.}"
filename="${filename%.*}"
```

