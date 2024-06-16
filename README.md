
import React, { useEffect, useState } from 'react';
import { useNavigate } from 'react-router-dom';
import { deleteEmployee, listEmployees, getEmployee } from '../services/EmployeeService';

const ListEmployeeComponent = () => {
  const [employees, setEmployees] = useState([]);
  const [searchId, setSearchId] = useState('');
  const navigator = useNavigate();

  useEffect(() => {
    getAllEmployees();
  }, []);

  function getAllEmployees() {
    listEmployees()
      .then((response) => {
        setEmployees(response.data);
      })
      .catch((error) => {
        console.error(error);
      });
  }

  function addNewEmployee() {
    navigator('/add-employee');
  }

  function updateEmployee(id) {
    navigator(`/edit-employee/${id}`);
  }

  function removeEmployee(id) {
    deleteEmployee(id)
      .then((response) => {
        getAllEmployees();
      })
      .catch((error) => {
        console.error(error);
      });
  }

  function handleSearchChange(event) {
    setSearchId(event.target.value);
  }

  function searchEmployeeById() {
    if (searchId) {
      getEmployee(searchId)
        .then((response) => {
          setEmployees([response.data]);
        })
        .catch((error) => {
          console.error(error);
          setEmployees([]);
        });
    } else {
      getAllEmployees();
    }
  }

  return (
    <div>
      <h2 className='text-center'>List of Employees</h2>
      <div className='mb-2'>
        <input
          type='text'
          className='form-control'
          placeholder='Search by ID'
          value={searchId}
          onChange={handleSearchChange}
        />
        <button className='btn btn-primary btn-lg mt-2' onClick={searchEmployeeById}>
          Search
        </button>
      </div>
      <button className='btn btn-primary btn-lg mb-2' onClick={addNewEmployee}>
        Add Employee
      </button>
      <table className='table table-striped table-bordered'>
        <thead>
          <tr>
            <th>Employee Id</th>
            <th>Employee First Name</th>
            <th>Employee Last Name</th>
            <th>Employee Email Id</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          {employees.map((employee) => (
            <tr key={employee.id}>
              <td>{employee.id}</td>
              <td>{employee.firstName}</td>
              <td>{employee.lastName}</td>
              <td>{employee.email}</td>
              <td>
                <button className='btn btn-info' onClick={() => updateEmployee(employee.id)}>
                  Update
                </button>
                <button
                  className='btn btn-info'
                  onClick={() => removeEmployee(employee.id)}
                  style={{ marginLeft: '10px' }}
                >
                  Delete
                </button>
              </td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
};

export default ListEmployeeComponent;
