# House-Price-Prediction

import React, { useState, useMemo, useCallback, useRef } from 'react';
import { AgGridReact } from 'ag-grid-react';
import 'ag-grid-community/styles/ag-grid.css';
import 'ag-grid-community/styles/ag-theme-alpine.css';
import { Eye, Download, Trash2 } from 'lucide-react';

// Dummy data for the table
const dummyData = [
  { id: 1, bu: 'BPC', region: 'North', setName: 'BPC Set 1', setId: 'BPC_North_BPC Set 1_24th July', goLiveDate: '01-06-2025', endDate: '01-07-2025', status: 'Ready' },
  { id: 2, bu: 'BPC', region: 'North', setName: 'BPC Set 2', setId: 'BPC_North_BPC Set 2_24th July', goLiveDate: '01-06-2025', endDate: '01-07-2025', status: 'Live' },
  { id: 3, bu: 'BPC', region: 'North', setName: 'BPC Set 3', setId: 'BPC_North_BPC Set 3_24th July', goLiveDate: '01-06-2025', endDate: '01-07-2025', status: 'Pending' },
  { id: 4, bu: 'BPC', region: 'North', setName: 'BPC Set 4', setId: 'BPC_North_BPC Set 4_24th July', goLiveDate: '01-06-2025', endDate: '01-07-2025', status: 'Live' },
  { id: 5, bu: 'BPC', region: 'North', setName: 'BPC Set 5', setId: 'BPC_North_BPC Set 5_24th July', goLiveDate: '01-06-2025', endDate: '01-07-2025', status: 'Pending' },
];

// Status Badge Component
const StatusBadge = ({ value }) => {
  const getStatusStyle = (status) => {
    switch (status.toLowerCase()) {
      case 'live': return 'bg-green-100 text-green-600 border-green-200';
      case 'pending': return 'bg-orange-100 text-orange-600 border-orange-200';
      case 'ready': return 'bg-blue-100 text-blue-600 border-blue-200';
      default: return 'bg-gray-100 text-gray-600 border-gray-200';
    }
  };
  return (
    <span className={`inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium border ${getStatusStyle(value)}`}>
      {value}
    </span>
  );
};

// Actions Cell Renderer
const ActionsCellRenderer = ({ data, api }) => {
  const handlePreview = (e) => {
    e.stopPropagation();
    alert(`Preview: ${data.setName}`);
  };
  const handleDownload = (e) => {
    e.stopPropagation();
    alert(`Download: ${data.setName}`);
  };
  const handleDelete = (e) => {
    e.stopPropagation();
    if (window.confirm(`Delete ${data.setName}?`)) {
      api.applyTransaction({ remove: [data] });
    }
  };

  return (
    <div className="flex items-center justify-center gap-1">
      <button onClick={handlePreview} className="p-1.5 hover:bg-gray-100 rounded" title="Preview">
        <Eye className="w-4 h-4 text-gray-500" />
      </button>
      <button onClick={handleDownload} className="p-1.5 hover:bg-gray-100 rounded" title="Download">
        <Download className="w-4 h-4 text-gray-500" />
      </button>
      <button onClick={handleDelete} className="p-1.5 hover:bg-gray-100 rounded" title="Delete">
        <Trash2 className="w-4 h-4 text-gray-500" />
      </button>
    </div>
  );
};

// Main Table Component
const MerchandisingSetsTableView = () => {
  const [rowData, setRowData] = useState(dummyData);
  const gridRef = useRef();

  const columnDefs = useMemo(() => [
    { headerName: 'BU', field: 'bu', width: 80 },
    { headerName: 'Region', field: 'region', width: 100 },
    { headerName: 'Set Name', field: 'setName', width: 120 },
    { headerName: 'Set ID', field: 'setId', flex: 1, minWidth: 200 },
    { headerName: 'Go-live Date', field: 'goLiveDate', width: 130 },
    { headerName: 'End Date', field: 'endDate', width: 110 },
    { headerName: 'Status', field: 'status', width: 100, cellRenderer: StatusBadge },
    { headerName: '', field: 'actions', width: 120, cellRenderer: ActionsCellRenderer, pinned: 'right' }
  ], []);

  const defaultColDef = useMemo(() => ({
    resizable: true,
    sortable: true,
    filter: false,
  }), []);

  return (
    <div className="bg-white rounded-lg border border-gray-200 shadow-sm">
      <div className="ag-theme-alpine" style={{ height: '600px', width: '100%' }}>
        <AgGridReact
          ref={gridRef}
          rowData={rowData}
          columnDefs={columnDefs}
          defaultColDef={defaultColDef}
          rowHeight={50}
          headerHeight={45}
          animateRows={true}
          rowSelection="multiple"
          suppressRowClickSelection={true}
          getRowId={(params) => params.data.id}
        />
      </div>
    </div>
  );
};

export default MerchandisingSetsTableView;