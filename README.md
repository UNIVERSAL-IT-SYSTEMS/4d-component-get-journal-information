# 4d-component-get-journal-information
Returns the "include in journal" status of each table.

An example of using the new [EXPORT STRUCTURE](http://doc.4d.com/4Dv15/4D/15/EXPORT-STRUCTURE.301-2006617.en.html) command.

```
$info:=Get_journal_information 
```

Example
---
![](https://github.com/miyako/4d-component-get-journal-information/blob/master/images/screenshot.png)

Source
---
```
C_TEXT($structure)
EXPORT STRUCTURE($structure)

$dom:=DOM Parse XML variable($structure)

ARRAY TEXT($tables;0)
$table:=DOM Find XML element($dom;"base/table";$tables)

ARRAY OBJECT($tableInfos;0)
C_OBJECT($tableInfo)

C_LONGINT($id)
C_TEXT($uuid;$name)
C_BOOLEAN($prevent_journaling)

For ($i;0x0001;Size of array($tables))

$table:=$tables{$i}

CLEAR VARIABLE($id)
CLEAR VARIABLE($uuid)
CLEAR VARIABLE($name)
CLEAR VARIABLE($prevent_journaling)

ON ERR CALL("DOM_ERR")  //"prevent_journaling" attribute may not exist
DOM GET XML ATTRIBUTE BY NAME($table;"id";$id)
DOM GET XML ATTRIBUTE BY NAME($table;"uuid";$uuid)
DOM GET XML ATTRIBUTE BY NAME($table;"name";$name)
DOM GET XML ATTRIBUTE BY NAME($table;"prevent_journaling";$prevent_journaling)
ON ERR CALL("")

OB SET($tableInfo;"id";$id)
OB SET($tableInfo;"uuid";$uuid)
OB SET($tableInfo;"name";$name)
OB SET($tableInfo;"prevent_journaling";$prevent_journaling)

APPEND TO ARRAY($tableInfos;$tableInfo)
CLEAR VARIABLE($tableInfo)  //decouple array element
End for 

$journal_file:=DOM Find XML element($dom;"base/base_extra/journal_file")
C_BOOLEAN($journal_file_enabled)
DOM GET XML ATTRIBUTE BY NAME($journal_file;"journal_file_enabled";$journal_file_enabled)

C_OBJECT($journal_info)
OB SET($journal_info;"journal_file_enabled";$journal_file_enabled)
OB SET ARRAY($journal_info;"tables";$tableInfos)

DOM CLOSE XML($dom)

$0:=$journal_info
```
