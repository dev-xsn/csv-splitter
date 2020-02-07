This command can be used in Linux to split files with headers intact in each file.

```
awk -v l=11000 '(NR==1){header=$0;next}
                    (NR%l==2) { 
                       c=sprintf("%0.5d",c+1); 
                       close(file); file=FILENAME; sub(/csv$/,c".csv",file)
                       print header > file
                    }
                    {print $0 > file}' file.csv
```

This works in the following way:

* **`(NR==1){header=$0;next}`:** If the record/line is the first line, save that line as the _header_.
* **`(NR%l==2){...}`:** Every time we wrote `l=11000` records/lines, we need to start writing to a new file. This happens every time the modulo of the record/line number hits 2. This is on the lines _2, 2+l, 2+2l, 2+3l_,.... When such a line is found we do:
 * **`c=sprintf("%0.5d",c+1)`:** increase the counter with one, and print it as `000xx`
 * **`close(file)`:** close the file you just wrote too.
 * **`file=FILENAME; sub(/csv$/,c".csv",file)`:** define the new filename
 * **`print header > file`:** open the file and write the header to that file.
* **`{print $0 > file}`**: write the entries to the file.