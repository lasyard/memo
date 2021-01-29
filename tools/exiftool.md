# exiftool

## 11.89

### See version

```bash
exiftool -ver
```

### Move files

```bash
exiftool -ext jpg -if '${DateTimeOriginal} and ${Keywords} and ${Model}' -d '%Y/%Y%m/%y%m%d_%H%M%S' "-FileName<${HOME}/Pictures/my_photo/\${DateTimeOriginal}_\${Keywords;s/, /_/}_\${Model;tr/ /_/}%-3c.%e" *.jpg
exiftool -ext jpg -if '${DateTimeOriginal} and ${Keywords} and not ${Model}' -d '%Y/%Y%m/%y%m%d_%H%M%S' "-FileName<${HOME}/Pictures/my_photo/\${DateTimeOriginal}_\${Keywords;s/, /_/}%-.3c.%e" *.jpg
```

### Modify date

```bash
exiftool -ext jpg -if 'not ${DateTimeOriginal}' '-AllDates<FileModifyDate' −overwrite_original *.jpg
exiftool -ext jpg -if 'not ${DateTimeOriginal}' '-AllDates=2020:02:02 20:20:20' −overwrite_original *.jpg
```
