---
title: "jQuery DataTable 설치 및 사용"
date: 2019-05-30 00:00:00 -0400
categories: jquery
---
# 데이터테이블 - DataTable
---
    Create Date <2019-04-30>
---

## DataTable

* jQuery 라이브러리
* Grid Table, 페이징, 필터링, 정렬 등 지원



---

## DataTable 설치

### Add File

- [File Download](https://datatables.net/download/index)

```
    - <link rel="stylesheet" type="text/css" href="//cdn.datatables.net/1.10.12/css/jquery.dataTables.css">
    - <script type="text/javascript" charset="utf8" src="//cdn.datatables.net/1.10.12/js/jquery.dataTables.js"></script>
```

### Using npm

- npm install datatables.net
  : Core library
- npm install datatables.net-dt
  : Styling

### Using bower

- bower install --save datatables.net
- bower install --save datatables.net-dt



---

## Example Reference

- API URL : https://datatables.net/reference/api/

- Example URL : https://datatables.net/examples/index



---

## DataTable 객체 생성

* $(document).ready 단계에서 생성

  1. ```
     $('#tableid').DataTable(); //DataTable() returns an API instance immediately
     ```

  2. ```
     $('#tableid').dataTable().api(); //dataTable() returns a jQuery object
     ```

  3. ```
     new $.fn.dataTable.Api('#tableid');
     ```



---

## 기본 API 예제



#### 행 추가

- row.add ( [] )

  : add single row

- rows.add ( [], [], [] )

  : add multiple rows

```
$(document).ready(function() {
    var t = $('#example').DataTable();
    t.row.add([ // or t.rows.add
    	'col 1 value',
    	'col 2 value',
    	'col 3 value'
    ]).draw();
} );
```



#### 행 제거 

- row( this ).remove()

  : remove this row

```
var table = $('#example').DataTable();
 
$('#example tbody').on( 'click', 'img.icon-delete', function () {
    table
        .row( $(this).parents('tr') )
        .remove()
        .draw();
} );
```



#### 행 데이터 조회

- row( this ).data()

  : get existing data

- row( this ).data( d )

  : set new data

```
var table = $('#example').DataTable();
 
table.rows().every( function () {
    var d = this.data();
 
    d.counter++; // update data source for the row
 
    this.invalidate(); // invalidate the data DataTables has cached for this row
} );
```



#### 모든 열 검색

- 빠르게 테이블 내 모든 데이터 검색
- 특정 컬럼에서 검색 가능

```
<tfoot>
    <tr>
        <th>Name</th>
        <th>Position</th>
        <th>Office</th>
        <th>Age</th>
        <th>Start date</th>
        <th>Salary</th>
    </tr>
</tfoot>
```

```
$(document).ready(function() {
    // Setup - add a text input to each footer cell
    $('#example tfoot th').each( function () {
        var title = $(this).text();
        $(this).html( '<input type="text" placeholder="Search '+title+'" />' );
    } );
 
    // DataTable
    var table = $('#example').DataTable();
 
    // Apply the search
    table.columns().every( function () {
        var that = this;
 
        $( 'input', this.footer() ).on( 'keyup change', function () {
            if ( that.search() !== this.value ) {
                that
                    .search( this.value )
                    .draw();
            }
        } );
    } );
} );
```



####  행/열 하이라이팅

- 행 하이라이팅

```
<table class="hover"></table>
```

```
td.highlight {
    background-color: whitesmoke !important;
}
```

- 열 하이라이팅
  - cell().index()

  - cells().nodes()
  - column().nodes() 

```jsp
$(document).ready(function() {
    var table = $('#example').DataTable();
     
    $('#example tbody')
        .on( 'mouseenter', 'td', function () {
            var colIdx = table.cell(this).index().column;
 
            $( table.cells().nodes() ).removeClass( 'highlight' );
            $( table.column( colIdx ).nodes() ).addClass( 'highlight' );
        } );
} );
```



#### 선택

- row().select()
  - row( '.selected').data()

```
$(document).ready(function() {
    var table = $('#example').DataTable();
 
    $('#example tbody').on( 'click', 'tr', function () {
        if ( $(this).hasClass('selected') ) {
            $(this).removeClass('selected');
        }
        else {
            table.$('tr.selected').removeClass('selected');
            $(this).addClass('selected');
        }
    } );
 
    $('#button').click( function () {
        console.log( table.row('.selected').data() );
    } );
} );
```

- rows().select()
  - rows('.selected').data()

```
$(document).ready(function() {
    var table = $('#example').DataTable();
 
    $('#example tbody').on( 'click', 'tr', function () {
        $(this).toggleClass('selected');
    } );
 
    $('#button').click( function () {
        alert( table.rows('.selected').data().length +' row(s) selected' );
    } );
} );
```

- column().select()
  - column( {selected: true } ).data()
- columns().select()
  - columns( { selected: true } ).data()



 #### Input Form Data Sumbit

- get all `input`elements from HTML table.

```
$(document).ready(function() {
    var table = $('#example').DataTable({
        columnDefs: [{
            orderable: false,
            targets: [1,2,3]
        }]
    });
 
    $('button').click( function() {
        var data = table.$('input, select').serialize();
        alert(
            "The following data would have been submitted to the server: \n\n"+
            data.substr( 0, 120 )+'...'
        );
        return false;
    } );
} );
```



---

## javascript 사용을 통한 기본 예제



### HTML Table Form 생성

```
<table id="example" class="display" width="100%">
</table>
```



### 초기 세팅

- Searching
- Ordering
- Paging

```
var dataSet = [
        [ "Column1 Data", "Column2 Data", "Column3 Data", "Column4 Data", "Column5 Data", "Column6 Data" ],
        [ "Tiger Nixon", "System Architect", "Edinburgh", "5421", "2011/04/25", "$320,800" ],
    ];
var columnSet = [
	{ title: "Name" },
    { title: "Position" },
    { title: "Office" },
    { title: "Extn." },
    { title: "Start date" },
    { title: "Salary" }
];
$(document).ready(function() {
	$('#example').DataTable( {
        data: dataSet,
        columns: columnSet,
	} );
} );
```



### 명칭 커스터마이징

- 별도 json 파일을 통한 지정 가능

```
$(document).ready(function() {
    "language": {
    	// "url": "//cdn.datatables.net/plug-ins/9dcbecd42ad/i18n/German.json"
        "decimal":        "",
        "emptyTable":     "No data available in table",
        "info":           "Showing _START_ to _END_ of _TOTAL_ entries",
        "infoEmpty":      "Showing 0 to 0 of 0 entries",
        "infoFiltered":   "(filtered from _MAX_ total entries)",
        "infoPostFix":    "",
        "thousands":      ",",
        "lengthMenu":     "Show _MENU_ entries",
        "loadingRecords": "Loading...",
        "processing":     "Processing...",
        "search":         "Search:",
        "zeroRecords":    "No matching records found",
        "paginate": {
            "first":      "First",
            "last":       "Last",
            "next":       "Next",
            "previous":   "Previous"
        },
        "aria": {
            "sortAscending":  ": activate to sort column ascending",
            "sortDescending": ": activate to sort column descending"
        }
    }
} );
```



### DataTable 열 옵션 설정

- structure of columnDefs object
  - "targets" :  index number of target column
  - "data"  : display value
  - "orderData" : sort data of "targets" column
  - "visible" : show or hide
  - "searchable" : set search enable or disable
  - "render" : set function when rendered "targets" column

```
"columnDefs": [ {
    "targets": 4,
    "data": "description",
    "orderData": [4, 'desc'],
    "visible": false,
    "searchable": false,
    "render": function ( data, type, row, meta ) {
      return type === 'display' && data.length > 40 ?
        '<span title="'+data+'">'+data.substr( 0, 38 )+'...</span>' :
        data;
    }
  } ]
```



### DataTable 기능 사용 설정

```
$(document).ready(function() {
    $('#example').DataTable( {
        "paging":   false,
        "ordering": false,
        "info":     false
    } );
} );
```



### 정렬

#### 한 열에 대한 기본 정렬

- 생성 될 때 정렬옵션 주기

```
$(document).ready(function() {
    $('#example').DataTable( {
        "order": [[ 3, "desc" ]]
    } );
} );
```

#### 다중 열에 대한 기본 정렬

```
$(document).ready(function() {
    $('#example').DataTable( {
        columnDefs: [ {
            targets: [ 0 ],
            orderData: [ 0, 'desc' ]
        }, {
            targets: [ 1 ],
            orderData: [ 1, 'desc' ]
        }, {
            targets: [ 4 ],
            orderData: [ 4, 'desc' ]
        } ]
    } );
} );
```



### 히든 컬럼

- 특정 컬럼에 대한 visible 여부 설정

- 히든 데이터 또한, 여전히 존재하는 데이터기에 참조하여 사용 가능


```
$(document).ready(function() {
    $('#example').DataTable( {
        "columnDefs": [
            {
                "targets": [ 2 ],
                "visible": false,
                "searchable": false
            }
        ]
    } );
} );
```



### 페이징 타입 설정

- `numbers` - Page number buttons only
- `simple` - 'Previous' and 'Next' buttons only
- `simple_numbers` - 'Previous' and 'Next' buttons, plus page numbers
- `full` - 'First', 'Previous', 'Next' and 'Last' buttons
- `full_numbers` - 'First', 'Previous', 'Next' and 'Last' buttons, plus page numbers
- `first_last_numbers` - 'First' and 'Last' buttons, plus page numbers

```
$(document).ready(function() {
    $('#example').DataTable( {
        "pagingType": "full_numbers"
    } );
} );
```



### 스크롤바 사용 설정

- shows vertical and horizontal direction scroll bar in the DataTable's table body

```
$(document).ready(function() {
    $('#example').DataTable( {
    	"scrollX": true
        "scrollY":        "200px", // use vh for dynamic : "50vh"
        "scrollCollapse": true,
        "paging":         false
    } );
} );
```



### 이벤트 설정

#### DOM / jQuery events

- control DataTable with jQuery's method

```
$(document).ready(function() {
    var table = $('#example').DataTable();
     
    $('#example tbody').on('click', 'tr', function () {
        var data = table.row( this ).data();
        alert( 'You clicked on '+data[0]+'\'s row' );
    } );
} );
```

#### DataTables Events

- namespace `dt` is prevent colflicts arising with other jQuery plug-ins

```
$(document).ready(function() {
    var eventFired = function ( type ) {
        var n = $('#demo_info')[0];
        n.innerHTML += '<div>'+type+' event - '+new Date().getTime()+'</div>';
        n.scrollTop = n.scrollHeight;      
    }
 
    $('#example')
        .on( 'order.dt',  function () { eventFired( 'Order' ); } )
        .on( 'search.dt', function () { eventFired( 'Search' ); } )
        .on( 'page.dt',   function () { eventFired( 'Page' ); } )
        .DataTable();
} );
```



### 콜백함수 생성

- run the callback function when draw time

#### row callback ( row, data, index )

- `row` is row element
- `data` is this row data source (array or object)
- `index` is row index of DataTable's internal storage

```
$(document).ready(function() {
    $('#example').DataTable( {
        "createdRow": function ( row, data, index ) {
            if ( data[5].replace(/[\$,]/g, '') * 1 > 150000 ) {
                $('td', row).eq(5).addClass('highlight');
            }
        }
    } );
} );
```

#### headerCallback ( thead, data, start, end, display )

- `thead` is element of tables' header
- `data` is full data array of table
- `start` is index starting point in the current display array
- `end` is index end point in the current display array
- `display` is index array to translate the visual position to the full data array

```
$('#example').dataTable( {
  "headerCallback": function( thead, data, start, end, display ) {
    $(thead).find('th').eq(0).html( 'Displaying '+(end-start)+' records' );
  }
} );
```

#### footer callback ( tfoot, data, start, end, display )

- `tfoot` is element of tables' footer
- `data` is full data array of table
- `start` is index starting point in the current display array
- `end` is index end point in the current display array
- `display` is index array to translate the visual position to the full data array

```
$(document).ready(function() {
    $('#example').DataTable( {
        "footerCallback": function ( row, data, start, end, display ) {
            var api = this.api(), data;
 			$( api.column( 5 ).footer() ).html(
                api.column( 5 ).data().reduce( function ( a, b ) {
                    return a + b;
                }, 0 )
            );
        }
    } );
} );
```



### CSS Style 변경

- apply in HTML `<table classes=" ">` tag

  ```
  <table id="example" class="display" style="width:100%">
  ```

- display

- cell-border

- display compact

- hover

- order-column

- row-border

- stripe



# 참고문헌

- [Datatables.net](https://datatables.net)

  