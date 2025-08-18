# House-Price-Prediction

import React, { useState, useMemo, useCallback, useRef, useEffect } from 'react';
import { AgGridReact } from 'ag-grid-react';
import 'ag-grid-community/styles/ag-grid.css';
import 'ag-grid-community/styles/ag-theme-alpine.css';
import { Eye, Download, Trash2 } from 'lucide-react';
import ReactPaginate from 'react-paginate';

// Dummy data for the table
const dummyData = [
  {
    id: 1,
    bu: 'BPC',
    region: 'North',
    setName: 'BPC Set 1',
    setId: 'BPC_North_BPC Set 1_24th July',
    goLiveDate: '01-06-2025',
    endDate: '01-07-2025',
    status: 'Ready'
  },
  {
    id: 2,
    bu: 'BPC',
    region: 'North',
    setName: 'BPC Set 2',
    setId: 'BPC_North_BPC Set 2_24th July',
    goLiveDate: '01-06-2025',
    endDate: '01-07-2025',
    status: 'Live'
  },
  {
    id: 3,
    bu: 'BPC',
    region: 'North',
    setName: 'BPC Set 3',
    setId: 'BPC_North_BPC Set 3_24th July',
    goLiveDate: '01-06-2025',
    endDate: '01-07-2025',
    status: 'Pending'
  },
  {
    id: 4,
    bu: 'BPC',
    region: 'North',
    setName: 'BPC Set 4',
    setId: 'BPC_North_BPC Set 4_24th July',
    goLiveDate: '01-06-2025',
    endDate: '01-07-2025',
    status: 'Live'
  },
  {
    id: 5,
    bu: 'BPC',
    region: 'North',
    setName: 'BPC Set 5',
    setId: 'BPC_North_BPC Set 5_24th July',
    goLiveDate: '01-06-2025',
    endDate: '01-07-2025',
    status: 'Pending'
  },
  {
    id: 6,
    bu: 'BPC',
    region: 'North',
    setName: 'BPC Set 6',
    setId: 'BPC_North_BPC Set 6_24th July',
    goLiveDate: '01-06-2025',
    endDate: '01-06-2025',
    status: 'Live'
  },
  {
    id: 7,
    bu: 'BPC',
    region: 'North',
    setName: 'BPC Set 7',
    setId: 'BPC_North_BPC Set 7_24th July',
    goLiveDate: '01-06-2025',
    endDate: '01-07-2025',
    status: 'Ready'
  },
  {
    id: 8,
    bu: 'BPC',
    region: 'North',
    setName: 'BPC Set 8',
    setId: 'BPC_North_BPC Set 8_24th July',
    goLiveDate: '01-06-2025',
    endDate: '01-06-2025',
    status: 'Live'
  },
  {
    id: 9,
    bu: 'BPC',
    region: 'North',
    setName: 'BPC Set 9',
    setId: 'BPC_North_BPC Set 9_24th July',
    goLiveDate: '01-06-2025',
    endDate: '01-07-2025',
    status: 'Pending'
  },
  {
    id: 10,
    bu: 'BPC',
    region: 'North',
    setName: 'BPC Set 10',
    setId: 'BPC_North_BPC Set 10_24th July',
    goLiveDate: '01-06-2025',
    endDate: '01-06-2025',
    status: 'Ready'
  },
  // Add more dummy data to test pagination
  {
    id: 11,
    bu: 'BPC',
    region: 'South',
    setName: 'BPC Set 11',
    setId: 'BPC_South_BPC Set 11_25th July',
    goLiveDate: '02-06-2025',
    endDate: '02-07-2025',
    status: 'Ready'
  },
  {
    id: 12,
    bu: 'BPC',
    region: 'South',
    setName: 'BPC Set 12',
    setId: 'BPC_South_BPC Set 12_25th July',
    goLiveDate: '02-06-2025',
    endDate: '02-07-2025',
    status: 'Live'
  }
];

// Status Badge Component
const StatusBadge = ({ value }) => {
  const getStatusStyle = (status) => {
    switch (status.toLowerCase()) {
      case 'live':
        return 'bg-green-100 text-green-600 border-green-200';
      case 'pending':
        return 'bg-orange-100 text-orange-600 border-orange-200';
      case 'ready':
        return 'bg-blue-100 text-blue-600 border-blue-200';
      default:
        return 'bg-gray-100 text-gray-600 border-gray-200';
    }
  };

  return (
    <span className={`inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium border ${getStatusStyle(value)}`}>
      {value}
    </span>
  );
};

// Actions Cell Renderer
const ActionsCellRenderer = ({ data, api, onPreview, onDownload, onDelete }) => {
  const handlePreview = (e) => {
    e.stopPropagation();
    if (onPreview) {
      onPreview(data);
    } else {
      console.log('Preview:', data);
      alert(`Preview: ${data.setName}`);
    }
  };

  const handleDownload = (e) => {
    e.stopPropagation();
    if (onDownload) {
      onDownload(data);
    } else {
      console.log('Download:', data);
      alert(`Download: ${data.setName}`);
    }
  };

  const handleDelete = (e) => {
    e.stopPropagation();
    if (onDelete) {
      onDelete(data);
    } else {
      if (window.confirm(`Delete ${data.setName}?`)) {
        console.log('Deleted:', data);
      }
    }
  };

  return (
    <div className="flex items-center justify-center gap-1">
      <button
        onClick={handlePreview}
        className="p-1.5 hover:bg-gray-100 rounded transition-colors"
        title="Preview"
      >
        <Eye className="w-4 h-4 text-gray-500" />
      </button>
      <button
        onClick={handleDownload}
        className="p-1.5 hover:bg-gray-100 rounded transition-colors"
        title="Download"
      >
        <Download className="w-4 h-4 text-gray-500" />
      </button>
      <button
        onClick={handleDelete}
        className="p-1.5 hover:bg-gray-100 rounded transition-colors"
        title="Delete"
      >
        <Trash2 className="w-4 h-4 text-gray-500" />
      </button>
    </div>
  );
};

// Main Integrated Table Component
const IntegratedMerchandiseTable = ({ 
  data = dummyData, 
  onPreview, 
  onDownload, 
  onDelete,
  height = '600px',
  pagination = true,
  pageSize = 10,
  isMobile = false,
  styles = {} // Accept styles prop for CSS modules
}) => {
  const [rowData, setRowData] = useState(data);
  const [currentPage, setCurrentPage] = useState(0); // ReactPaginate uses 0-based indexing
  const [loading, setLoading] = useState(false);
  const gridRef = useRef();

  const pageCount = Math.ceil(rowData.length / pageSize);

  // Get current page data
  const currentData = useMemo(() => {
    const startIndex = currentPage * pageSize;
    const endIndex = startIndex + pageSize;
    return rowData.slice(startIndex, endIndex);
  }, [rowData, currentPage, pageSize]);

  // Column definitions
  const columnDefs = useMemo(() => [
    {
      headerName: 'BU',
      field: 'bu',
      width: isMobile ? 60 : 80,
      sortable: true,
      filter: false,
      cellStyle: { fontSize: '14px', color: '#374151' }
    },
    {
      headerName: 'Region',
      field: 'region',
      width: isMobile ? 80 : 100,
      sortable: true,
      filter: false,
      cellStyle: { fontSize: '14px', color: '#374151' }
    },
    {
      headerName: 'Set Name',
      field: 'setName',
      width: isMobile ? 100 : 120,
      sortable: true,
      filter: false,
      cellStyle: { fontSize: '14px', color: '#374151' }
    },
    {
      headerName: 'Set ID',
      field: 'setId',
      flex: 1,
      minWidth: isMobile ? 200 : 280,
      sortable: true,
      filter: false,
      cellStyle: { fontSize: '14px', color: '#374151' }
    },
    {
      headerName: 'Go-live Date',
      field: 'goLiveDate',
      width: isMobile ? 110 : 130,
      sortable: true,
      filter: false,
      cellStyle: { fontSize: '14px', color: '#374151' }
    },
    {
      headerName: 'End Date',
      field: 'endDate',
      width: isMobile ? 100 : 110,
      sortable: true,
      filter: false,
      cellStyle: { fontSize: '14px', color: '#374151' }
    },
    {
      headerName: 'Status',
      field: 'status',
      width: isMobile ? 90 : 100,
      cellRenderer: StatusBadge,
      sortable: true,
      filter: false,
      cellStyle: { display: 'flex', alignItems: 'center' }
    },
    {
      headerName: '',
      field: 'actions',
      width: isMobile ? 100 : 120,
      cellRenderer: (params) => (
        <ActionsCellRenderer
          {...params}
          onPreview={onPreview}
          onDownload={onDownload}
          onDelete={onDelete}
        />
      ),
      sortable: false,
      filter: false,
      pinned: 'right',
      cellStyle: { display: 'flex', alignItems: 'center', justifyContent: 'center' }
    }
  ], [onPreview, onDownload, onDelete, isMobile]);

  const defaultColDef = useMemo(() => ({
    resizable: true,
    sortable: false,
    filter: false,
  }), []);

  const onGridReady = useCallback(() => {
    setLoading(false);
  }, []);

  // Handle page change for ReactPaginate
  const handlePageChange = ({ selected }) => {
    setCurrentPage(selected);
  };

  // Loading overlay template
  const overlayLoadingTemplate = '<span className="ag-overlay-loading-center">Loading...</span>';
  const overlayNoRowsTemplate = '<span className="ag-overlay-loading-center">No Data</span>';

  // API Methods for external use
  const tableAPI = {
    addRow: (newRow) => {
      const updatedData = [...rowData, { ...newRow, id: Date.now() }];
      setRowData(updatedData);
    },
    
    updateRow: (id, updatedData) => {
      const updatedRows = rowData.map(row => 
        row.id === id ? { ...row, ...updatedData } : row
      );
      setRowData(updatedRows);
    },
    
    deleteRow: (id) => {
      const filteredData = rowData.filter(row => row.id !== id);
      setRowData(filteredData);
    },
    
    getAllData: () => rowData,
    
    getSelectedRows: () => {
      if (gridRef.current?.api) {
        return gridRef.current.api.getSelectedRows();
      }
      return [];
    },
    
    exportToCsv: (fileName = 'merchandise-table.csv') => {
      if (gridRef.current?.api) {
        gridRef.current.api.exportDataAsCsv({ fileName });
      }
    },
    
    refreshGrid: () => {
      if (gridRef.current?.api) {
        gridRef.current.api.refreshCells();
      }
    },

    setLoading: (isLoading) => {
      setLoading(isLoading);
      if (gridRef.current?.api) {
        if (isLoading) {
          gridRef.current.api.showLoadingOverlay();
        } else {
          gridRef.current.api.hideOverlay();
        }
      }
    }
  };

  // Show loading overlay if needed
  useEffect(() => {
    if (loading && gridRef.current?.api) {
      gridRef.current.api.showLoadingOverlay();
    }
  }, [loading]);

  return (
    <div className="bg-white rounded-lg border border-gray-200 shadow-sm">
      <div className="ag-theme-alpine" style={{ height, width: '100%' }}>
        <AgGridReact
          ref={gridRef}
          rowData={currentData} // Use paginated data
          columnDefs={columnDefs}
          defaultColDef={defaultColDef}
          pagination={false} // Disable AG Grid pagination, use ReactPaginate
          rowHeight={50}
          headerHeight={45}
          animateRows={true}
          rowSelection="multiple"
          suppressRowClickSelection={true}
          getRowId={(params) => params.data.id}
          onGridReady={onGridReady}
          suppressPaginationPanel={true}
          overlayLoadingTemplate={overlayLoadingTemplate}
          overlayNoRowsTemplate={overlayNoRowsTemplate}
          context={{ tableAPI }}
        />
      </div>
      
      {/* ReactPaginate Integration */}
      {pagination && rowData && rowData.length > 0 && (
        <div className={styles.pagination || 'border-t border-gray-200 py-4'}>
          <div className={styles.reportx || 'flex justify-center'}>
            <ReactPaginate
              breakLabel="..."
              nextLabel="›"
              onPageChange={handlePageChange}
              pageRangeDisplayed={isMobile ? 3 : 3}
              marginPagesDisplayed={isMobile ? 1 : 1}
              pageCount={pageCount}
              previousLabel="‹"
              renderOnZeroPageCount={null}
              forcePage={currentPage}
              className="flex items-center space-x-1"
              pageClassName="page-item"
              pageLinkClassName="w-8 h-8 flex items-center justify-center rounded border text-sm text-gray-600 border-gray-300 hover:bg-gray-50"
              activeLinkClassName="bg-blue-600 text-white border-blue-600"
              previousClassName="page-item"
              previousLinkClassName="w-8 h-8 flex items-center justify-center rounded border text-gray-600 border-gray-300 hover:bg-gray-50"
              nextClassName="page-item"
              nextLinkClassName="w-8 h-8 flex items-center justify-center rounded border text-gray-600 border-gray-300 hover:bg-gray-50"
              breakClassName="page-item"
              breakLinkClassName="px-2 text-gray-400"
              disabledClassName="opacity-50 cursor-not-allowed"
            />
          </div>
        </div>
      )}

      <style jsx global>{`
        .ag-theme-alpine {
          --ag-header-background-color: #f9fafb;
          --ag-header-foreground-color: #374151;
          --ag-border-color: #e5e7eb;
          --ag-row-hover-color: #f9fafb;
          --ag-selected-row-background-color: #eff6ff;
          --ag-font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', sans-serif;
          --ag-font-size: 14px;
        }
        
        .ag-header-cell-text {
          font-weight: 500;
          color: #6b7280;
          font-size: 13px;
        }
        
        .ag-row {
          border-bottom: 1px solid #f3f4f6;
        }
        
        .ag-row:hover {
          background-color: #fafafa;
        }
        
        .ag-cell {
          border: none;
          display: flex;
          align-items: center;
          padding: 12px 8px;
        }
        
        .ag-header {
          border-bottom: 1px solid #e5e7eb;
        }
        
        .ag-root-wrapper {
          border: none;
        }

        .ag-header-cell {
          border-right: 1px solid #f3f4f6;
        }
        
        .ag-cell {
          border-right: 1px solid #f3f4f6;
        }

        /* ReactPaginate custom styling */
        .page-item a:hover {
          background-color: #f9fafb !important;
        }
        
        .page-item .active {
          background-color: #2563eb !important;
          color: white !important;
          border-color: #2563eb !important;
        }
      `}</style>
    </div>
  );
};

export default IntegratedMerchandiseTable;