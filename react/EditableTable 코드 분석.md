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
    
    // 해당 테이블의 row, col 좌표를 가지고 값을 변경한다
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
    return (
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
        
    );
};
~~~

> useTable - https://react-table.tanstack.com/docs/api/useTable 
