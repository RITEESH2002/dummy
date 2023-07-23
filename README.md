import React, { useState } from 'react';
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableRow,
  Select,
  MenuItem,
  TextField,
} from '@mui/material';

const TableWithDropdowns = () => {
  const [data, setData] = useState(tableData);

  const handleChange = (rowIndex, columnName, newValue) => {
    setData((prevData) =>
      prevData.map((row, index) =>
        index === rowIndex ? { ...row, [columnName]: newValue } : row
      )
    );
  };

  return (
    <Table>
      <TableHead>
        <TableRow>
          <TableCell>BR_CD</TableCell>
          <TableCell>DOC_TYP_DESC</TableCell>
          <TableCell>
            IN_DTS_IND
            <br />
            <small>Edit with dropdown of values Y, N</small>
          </TableCell>
          <TableCell>
            DOC_LVL_IN
            <br />
            <small>Edit with dropdown of values C, A</small>
          </TableCell>
        </TableRow>
      </TableHead>
      <TableBody>
        {data.map((row, index) => (
          <TableRow key={index}>
            <TableCell>{row.BR_CD}</TableCell>
            <TableCell>
              <TextField
                value={row.DOC_TYP_DESC}
                onChange={(e) =>
                  handleChange(index, 'DOC_TYP_DESC', e.target.value)
                }
              />
            </TableCell>
            <TableCell>
              <Select
                value={row.IN_DTS_IND}
                onChange={(e) =>
                  handleChange(index, 'IN_DTS_IND', e.target.value)
                }
              >
                <MenuItem value="Y">Y</MenuItem>
                <MenuItem value="N">N</MenuItem>
              </Select>
            </TableCell>
            <TableCell>
              <Select
                value={row.DOC_LVL_IN}
                onChange={(e) =>
                  handleChange(index, 'DOC_LVL_IN', e.target.value)
                }
              >
                <MenuItem value="C">C</MenuItem>
                <MenuItem value="A">A</MenuItem>
              </Select>
            </TableCell>
          </TableRow>
        ))}
      </TableBody>
    </Table>
  );
};

export default TableWithDropdowns;
 const tableData = [
  { BR_CD: '001', DOC_TYP_DESC: 'Document 1', IN_DTS_IND: 'Y', DOC_LVL_IN: 'C' },
  { BR_CD: '002', DOC_TYP_DESC: 'Document 2', IN_DTS_IND: 'N', DOC_LVL_IN: 'A' },
  { BR_CD: '003', DOC_TYP_DESC: 'Document 3', IN_DTS_IND: 'Y', DOC_LVL_IN: 'A' },
  { BR_CD: '004', DOC_TYP_DESC: 'Document 4', IN_DTS_IND: 'N', DOC_LVL_IN: 'C' },
  { BR_CD: '005', DOC_TYP_DESC: 'Document 5', IN_DTS_IND: 'Y', DOC_LVL_IN: 'A' },
];
