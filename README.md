# TreeTables plug-in for jQuery
TreeTables is a jQuery plugin that enhances the functionality of the
popular [DataTables](https://github.com/DataTables/DataTables) plugin.

DataTables does not support tree data by default, so this plugin adds
that support.

## Usage

Include the following scripts on your page:

```
<script src="node_modules/jquery/dist/jquery.js"></script>
<script src="node_modules/datatables.net/js/jquery.dataTables.js"></script>
<script src="treeTable.js"></script>
```

And the following css in your document head:
```
    <link rel="stylesheet" href="node_modules/datatables.net-dt/css/jquery.dataTables.css"/>
    <link rel="stylesheet" href="tree-table.css"/>
```

### Basic Usage

```
 const organisationData = [
            {key: 1, parent: 0, level: 0, name: "CEO", hasChild: true},
            {key: 2, parent: 1, level: 1, name: "CTO", hasChild: true},
            {key: 3, parent: 2, level: 2, name: "developer", hasChild: false},
            {key: 4, parent: 1, level: 1, name: "CFO", hasChild: false}
        ];

        $('#my-table').treeTable({
            "data": myData,
            "columns": [
                {
                    "data": "name"
                }
            ]
        });
```

Data provided to the table must include the following fields:
* key: number - a unique row identifier
* parent: number - the key of this row's parent row
* level: number - how deeply nested is this row (a row with no parent is level 0,
a row with a parent is level 1, a row with a grandparent is level 2, etc)
* hasChild: bool - does this row have any children

### Options
TreeTable options are all DataTable options plus:
* collapsed: bool - whether to start with all child rows collapsed

```
        $('#my-table').treeTable({
            "data": myData,
            "collapsed": true,
            "columns": [
                {
                    "data": "name"
                }
            ]
        });
```

Please note that the TreeTable plugin adds 5 invisible columns to the table.
So user provided columns start from index 5 and any references to columns
must take this into account.

E.g., this table will be initially sorted by name:


```
        $('#my-table').treeTable({
            "data": myData,
            "collapsed": true,
            "columns": [
                {
                    "data": "name"
                },
                {
                    "data": "salary"
                }
            ],
            "order": [[ 5, 'asc' ]]
        });
```


### API
The datatables API will be attached to the table element in the usual way,
accessible by ```$('#my-table').DataTable()```

Please note that as with the options, any API operation that references columns by
index must take into account the 5 invisible columns. E.g. to re-sort the
above table by salary:

```
 $('#my-table').DataTable().order([ 6, 'asc' ])
 ```

## Credit
The approach used here was inspired by a [jsfiddle](http://jsfiddle.net/hcke44hy/8)
posted by a user called Mytko in the datatables forum:
https://datatables.net/forums/discussion/25045/treetable-in-datatables
