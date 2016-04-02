# bson2csv
BSON to CSV converter written in C

## Usage
### Invokation
```
bson2csv <collection.bson> <fields.txt> [[start index] <stop index>]
```
`collection.bson` - BSON file with documents (produced by mongodump, for example)

`fields.txt` - File with the field or fields to include in the export. The file must have only one field per line, and the line(s) must end with the LF character (0x0A). Field paths must be specified in standard Mongo dot notation. Order of extracted fields in resulting CSV file is the same as in fields file. All values are casted to string and escaped; structured fields are serialised and exported as JSON.

`start index` - How many records from BSON file program should skip.

`stop index` - Consecutive number of last exported document from BSON file.

### Example
Let\`s assume we have BSON file with documents having following structure:
```
$ bsondump sample.bson
{"_id":{"$oid":"000000000000000000012345"},"tags":["1","2","3","4"],"text":"asdfanother","source":{"name":"blah"},"type":["source"],"missing":["server_created_at"]}
2016-04-02T16:06:43.332+0300	1 objects found
```
And we need to extract `_id` field, first and second element of `tags` array, `text` field and `name` field from object in `source` field.

If so, we need to write following fields file:
```
_id
tags.0
tags.1
text
source.name
```

And run `bson2csv`:
```
$ bson2csv sample.bson sample.txt > example.csv
```

It will produce output file:
```
$ cat example.csv
"000000000000000000012345","1","2","asdfanother","blah"
```
