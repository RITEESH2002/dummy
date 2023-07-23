// import React from 'react';
// import { MDBDataTable } from 'mdbreact';
// import {useStateContext} from "../../context/ContextProvider";
// import Title from "../../components/Title/Title";
// import '../Color.css'
//
// const DocDetails = () => {
//
//   const {docDetails} = useStateContext();
//
//   let tableContent ;
//
//   if(docDetails.length === 0){
//     tableContent = <div className="text-gray-500 font-extrabold text-xl">NO DATA AVAILABLE HERE</div>
//   }
//   else {
//     const data = {
//       columns: [
//         {
//           label: 'BR_CD',
//           field: 'BR_CD',
//           sort: 'asc',
//           width: 80
//         },
//         {
//           label: 'DOC_TYP_DESC',
//           field: 'DOC_TYP_DESC',
//           sort: 'asc',
//           width: 80
//         },
//         {
//           label: 'IN_DTS_IND',
//           field: 'IN_DTS_IND',
//           sort: 'asc',
//           width: 50
//         },
//         {
//           label: 'DOC_LVL_IN',
//           field: 'DOC_LVL_IN',
//           sort: 'asc',
//           width: 100
//         },
//       ],
//       rows: docDetails && Array.isArray(docDetails)
//         ? docDetails.map(
//           item => (
//             {
//               "BR_CD": item.br_CD ? item.br_CD : '---' ,
//               "DOC_TYP_DESC": item.doc_TYP_DESC ? item.doc_TYP_DESC : '---',
//               "IN_DTS_IND": item.in_DTS_IND ? item.in_DTS_IND : '---',
//               "DOC_LVL_IN": item.doc_LVL_IN ? item.doc_LVL_IN : '---',
//             }
//           )) : []
//     };
//
//     tableContent = <MDBDataTable
//       striped
//       entriesOptions={[5,10,15,20]}
//       entries={5}
//       bordered={true}
//       hover
//       data={data}
//       noBottomColumns={true}
//       className='your-custom-styles'
//     />
//
//     const handleClick = (e) =>{
//       e.preventDefault();
//
//       // setFieldDocCode(fieldDocCode);
//       //
//       // navigate(`/Dashboard/FieldEditOverview/${fieldDocCode}`)
//     }
//
//   }
//   return (
//     <>
//       <div className="m-6 justify-center  p-1 bg-blue-500 rounded-3xl" >
//         <div className=" p-4 bg-blue-50 rounded-3xl">
//           <div className="flex align-items-center">
//             <Title title="DOC DETAILS"/>
//             <button variant="contained" className="h-full p-1 w-32 mt-2 rounded-xl border-none bg-success font-extrabold text-xl cursor-pointer text-white"
//             >
//               Edit
//             </button>
//           </div>
//
//           <hr className="border-bottom mb-4"/>
//           {tableContent}
//         </div>
//       </div>
//
//     </>
//   );
// }
//
// export default DocDetails;
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
  makeStyles,
  TableContainer,
  createTheme,
  TablePagination
} from '@mui/material';

import {useStateContext} from "../../context/ContextProvider";
import Title from "../../components/Title/Title";



const DocDetails = () => {
const theme = createTheme({
  components: {
    // Name of the component
    MuiTableCell: {
      styleOverrides: {
        // Name of the slot
        root: {
          // Some CSS
          fontSize: '1rem',
          fontWeight: 'bold',
        },
      },
    },
    MuiTableContainer: {
      styleOverrides: {
        // Name of the slot
        root: {
          // Some CSS
          border:'1px solid #000087',
          padding:'40px'
        },
      },
    },
    MuiTextField: {
      styleOverrides: {
        // Name of the slot
        root: {
          // Some CSS
          fontWeight:'bolder',
          width:'80%'
        },
      },
    },
    MuiTable: {
      styleOverrides: {
        // Name of the slot
        root: {
          // Some CSS
          width:'100%'
        },
      },
    },
    MuiTableHead : {
      styleOverrides: {
        // Name of the slot
        root: {
          // Some CSS
          backgroundColor:'lightblue',
        },
      },
    }
  },
});

  const {docDetails, setDocDetails} = useStateContext();

  const [page, setPage] = useState(0);
  const [edit, setEdit] = useState(false);

  const rowsPerPageOptions = [5,10,20];
  const defaultRowsPerPage = rowsPerPageOptions[0];
  const [rowsPerPage, setRowsPerPage] = useState(defaultRowsPerPage);

  console.log("UPDATED DOC DETAILS ", docDetails)
  const submitHandle = (e) => {
    e.preventDefault();
    setEdit(!edit);
  }
  const handleChange = (rowIndex, columnName, newValue) => {
    setDocDetails((prevData) =>
      prevData.map((row, index) =>
        index === rowIndex ? { ...row, [columnName]: newValue } : row
      )
    );
  };

  const handleChangePage = (event, newPage) => {
    setPage(newPage);
  }

  const handleChangeRowsPerPage = (event) => {
    setRowsPerPage(parseInt(event.target.value, 10));
    setPage(0);
  }

  const lastIndex = (page+1)*rowsPerPage;
  const firstIndex = lastIndex-rowsPerPage;
  const pageData = docDetails.slice(firstIndex,lastIndex);

  return (
    <>
      <div className="m-6 justify-center  p-1 bg-blue-500 rounded-3xl" >
        <div className=" p-4 bg-blue-50 rounded-3xl">
          <div className="flex align-items-center">
            <Title title="DOC DETAILS"/>
            {edit ? <><button variant="contained" className="h-full p-1 w-32 mt-2 rounded-xl border-none bg-success font-extrabold text-xl cursor-pointer text-white"
            onClick={submitHandle}
            >Submit
            </button></> : <><button variant="contained" className="h-full p-1 w-32 mt-2 rounded-xl border-none bg-success font-extrabold text-xl cursor-pointer text-white"
                      onClick={()=>setEdit(!edit)}
              >Edit
              </button>
              </>
            }
          </div>
          <hr className="border-bottom mb-4"/>
          {edit ? <>
          <TableContainer theme={theme}>
          <Table theme={theme}>
      <TableHead theme={theme}>
        <TableRow>
          <TableCell theme={theme}>br_CD</TableCell>
          <TableCell theme={theme}>doc_TYP_DESC</TableCell>
          <TableCell theme={theme}>
            in_DTS_IND
          </TableCell>
          <TableCell theme={theme}>
            doc_LVL_IN
          </TableCell>
        </TableRow>
      </TableHead>
      <TableBody theme={theme}>
        {pageData.map((row, index) => (
          <TableRow key={index}>
            <TableCell theme={theme}>{row.br_CD}</TableCell>
            <TableCell>
              <TextField
                theme={theme}
                value={row.doc_TYP_DESC}
                onChange={(e) =>
                  handleChange(index, 'doc_TYP_DESC', e.target.value)
                }
              />
            </TableCell>
            <TableCell>
              <Select
                value={row.in_DTS_IND}
                onChange={(e) =>
                  handleChange(index, 'in_DTS_IND', e.target.value)
                }
              >
                <MenuItem value="Y">Y</MenuItem>
                <MenuItem value="N">N</MenuItem>
              </Select>
            </TableCell>
            <TableCell>
              <Select
                value={row.doc_LVL_IN}
                onChange={(e) =>
                  handleChange(index, 'doc_LVL_IN', e.target.value)
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
    </TableContainer>
            <TablePagination
              rowsPerPageOptions = {rowsPerPageOptions}
              component="div"
              count={docDetails.length}
              rowsPerPage={rowsPerPage}
              page={page}
              onPageChange={handleChangePage}
              onRowsPerPageChange={handleChangeRowsPerPage}
            />
            </> : <>
            <TableContainer theme={theme}>
              <Table theme={theme}>
                <TableHead theme={theme}>
                  <TableRow>
                    <TableCell theme={theme}>br_CD</TableCell>
                    <TableCell theme={theme}>doc_TYP_DESC</TableCell>
                    <TableCell theme={theme}>
                      in_DTS_IND
                    </TableCell>
                    <TableCell theme={theme}>
                      doc_LVL_IN
                    </TableCell>
                  </TableRow>
                </TableHead>
                <TableBody theme={theme}>
                  {pageData.map((row, index) => (
                    <TableRow key={index}>
                      <TableCell theme={theme}>{row.br_CD}</TableCell>
                      <TableCell theme={theme}>
                          {row.doc_TYP_DESC}

                      </TableCell>
                      <TableCell theme={theme}>
                        {row.in_DTS_IND}

                      </TableCell>
                      <TableCell theme={theme}>
                       {row.doc_LVL_IN}
                      </TableCell>
                    </TableRow>
                  ))}
                </TableBody>
              </Table>
            </TableContainer>
            <TablePagination
              rowsPerPageOptions = {rowsPerPageOptions}
              component="div"
              count={docDetails.length}
              rowsPerPage={rowsPerPage}
              page={page}
              onPageChange={handleChangePage}
              onRowsPerPageChange={handleChangeRowsPerPage}
            />
            </>
          }
        </div>
      </div>
      </>
  );
};

export default DocDetails;

