# EditableTable 코드 분석

> https://previews.aspirity.com/easydev/tables/editable_table  ver 3.4.0
>
> ✅TODO : EditableTable 코드를 분석하여, Table의 row를 수정할 수 있도록

## 코드 구조

~~~
EditableReactTable							// Table Container
	ReactTableBase							// 테이블의 옵션 지정
		ReactTableConstructor				// Header + Body
			ReactTableHeader
			BodyReactTable
				ReactTableDNDBody
				ReactTableDefaultBody
~~~



### EditableReactTable.jsx

> src/containers/Tables/EditableTable/components/EditableReactTable.jsx

~~~jsx
const EditableReactTable = ({reactTableData}) => {
    const [rows, setData] = useState(reactTableData.tableRowsData);
  	const [withSearchEngine, setWithSearchEngine] = useState(false);
    
    // 해당 테이블의 row, col 좌표를 가지고 값을 변경한다 -- 테이블 내용 수정 로직
    const updateEditableData = (rowIndex, columnId, value) => {
    	setData(old => old.map((item, index) => {
      		if (index === rowIndex) {
        		return {
          			...old[rowIndex],
          			[columnId]: value,
        		};
      		}
      		return item;
    	}));
  	};
    
    const tableConfig = {
    	isEditable: true,
    	isSortable,
    	isResizable: false,
    	withPagination,
    	withSearchEngine,
    	manualPageSize: [10, 20, 30, 40],
    	placeholder: 'Search by First name...',
  	};
    
    return (
        <ReactTableBase
            key={withSearchEngine ? 'searchable' : 'common'}
            columns={reactTableData.tableHeaderData}
            data={rows}
            updateEditableData={updateEditableData}
            tableConfig={tableConfig}
          />
    );
};
~~~

> Editable하게 만들기 위해서는 tableConfig.isEditable = `true`

### ReactTableBase.jsx

> src/shared/components/table/ReactTableBase.jsx

~~~jsx
const ReactTableBase = ({
  columns,
  data,
  updateEditableData,
  updateDraggableData,
  tableConfig,
}) => {
    const {
    	isEditable,
    	isResizable,
    	isSortable,
    	withDragAndDrop,
    	withPagination,
    	withSearchEngine,
    	manualPageSize,
  	} = tableConfig;										// table options
    const [filterValue, setFilterValue] = useState(null);	// search filter
  	const tableOptions = {
    	columns,
    	data,
    	updateDraggableData,
    	updateEditableData,
    	setFilterValue,
    	defaultColumn: {},									// ?
    	isEditable,
   		withDragAndDrop: withDragAndDrop || false,
    	dataLength: data.length,
    	autoResetPage: false,
    	disableSortBy: !isSortable,
    	manualSortBy: !isSortable,
    	manualGlobalFilter: !withSearchEngine,
    	manualPagination: !withPagination,
    	initialState: {										// paging
      		pageIndex: 0,
      		pageSize: manualPageSize ? manualPageSize[0] : 10,
      		globalFilter: withSearchEngine && filterValue ? filterValue : '',
    	},
  	};
    
    let tableOptionalHook = [];
  	if (isResizable) tableOptionalHook = [useFlexLayout];	// table layout
    
    ///////////////////////////////
    if (withSearchEngine) {
    	tableOptions.defaultColumn = {
      		Cell: ReactTableCell,
    	};
  	}
  	if (isEditable) {
    	tableOptions.defaultColumn = {
      		Cell: ReactTableCellEditable,
    	};
  	}
    return (
        <ReactTableConstructor
      		key={isResizable || isEditable ? 'modified' : 'common'}
     		tableConfig={tableConfig}
      		tableOptions={tableOptions}
     		tableOptionalHook={tableOptionalHook}
    	/>
        
    );
};
~~~

> useFlexLayout -  https://react-table.tanstack.com/docs/api/useFlexLayout



### ReactTableConstructor

> src/shared/components/table/ReactTableConstructor.jsx

~~~jsx
const ReactTableConstructor = ({
    tableConfig, tableOptions, tableOptionalHook,
}) => {
  	const {
    	isEditable,
    	isResizable,
    	isSortable,
    	withPagination,
    	withSearchEngine,
    	manualPageSize,
    	placeholder,
  	} = tableConfig;
    const {
    	getTableProps,
    	getTableBodyProps,
    	headerGroups,
    	footerGroups,
    	state,
    	rows,
    	prepareRow,
    	page,
    	pageCount,
    	pageOptions,
    	gotoPage,
    	previousPage,
    	canPreviousPage,
    	nextPage,
    	canNextPage,
    	setPageSize,
    	setGlobalFilter,
    	withDragAndDrop,
    	updateDraggableData,
    	updateEditableData,
    	dataLength,
    	state: { pageIndex, pageSize },
  	} = useTable(
    	tableOptions,
    	useGlobalFilter,
    	useSortBy,
    	usePagination,
    	useResizeColumns,
    	useRowSelect,
    	...tableOptionalHook,
  	);
    return (
        <div className={withPagination ? 'table react-table' : 'table react-table table--not-pagination'}>
        	<table
                {...getTableProps()}
          		className={isEditable ? 'react-table editable-table' : 'react-table resizable-table'}
           	>
        		<ReactTableHeader />
        		<BodyReactTable
            		page={page}
            		getTableBodyProps={getTableBodyProps}
            		prepareRow={prepareRow}
            		updateDraggableData={updateDraggableData}
            		updateEditableData={updateEditableData}
            		isEditable={isEditable}
            		withDragAndDrop={withDragAndDrop}
         		/>
            </table>
        </div>
        
    );
};
~~~

> useTable - https://react-table.tanstack.com/docs/api/useTable 



### ReactTableBody

> src/shared/components/table/ReactTableBody.jsx

~~~jsx
const ReactTableDefaultBody = ({ page, getTableBodyProps, prepareRow }) => (
  <tbody className="table table--bordered" {...getTableBodyProps()}>
    {page.map((row) => {
      prepareRow(row);
      return (
        <tr {...row.getRowProps()}>
          {row.cells.map(cell => (
            <td {...cell.getCellProps()}>{cell.render('Cell')}</td>
          ))}
        </tr>
      );
    })}
  </tbody>
);

const ReactTableBody = ({
  page, getTableBodyProps, prepareRow, withDragAndDrop, updateDraggableData, theme,
}) => (
  <Fragment>
    {withDragAndDrop
      ? (
        <ReactTableDnDBody
          page={page}
          getTableBodyProps={getTableBodyProps}
          prepareRow={prepareRow}
          updateDraggableData={updateDraggableData}
          theme={theme}
        />
      ) : (
        <ReactTableDefaultBody
          page={page}
          getTableBodyProps={getTableBodyProps}
          prepareRow={prepareRow}
        />
      )
    }
  </Fragment>
);
~~~









# Editable Library

https://github.com/jamesmart77/react-form-editables

> 클래스 함수로만 작성 

https://github.com/bfischer/react-inline-editing



# Editable Code 작성

https://blog.logrocket.com/the-complete-guide-to-building-inline-editable-ui-in-react/

## Simple Editable Code

> isEdit변수를 지정하여 true/false일때 화면 결과를 다르게 렌더링한다

