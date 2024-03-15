# -code-snippet-demonstrate-how-to-create-a-simple-REST-API-in-Node.js-that-performs-CRUD-operations.
create a simple REST API in Node.js that performs CRUD operations.

const express = require('express');
const bodyParser = require('body-parser');

const app = express();
const PORT = 3000;

app.use(bodyParser.json());

let todos = [
  { id: 1, task: 'Learn Node.js', completed: false },
  { id: 2, task: 'Build a REST API', completed: false },
];

app.get('/todos', (req, res) => {
  res.json(todos);
});

app.get('/todos/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const todo = todos.find(todo => todo.id === id);
  if (todo) {
    res.json(todo);
  } else {
    res.status(404).json({ message: 'Todo not found' });
  }
});

app.post('/todos', (req, res) => {
  const { task, completed } = req.body;
  const id = todos.length + 1;
  const newTodo = { id, task, completed };
  todos.push(newTodo);
  res.status(201).json(newTodo);
});

app.put('/todos/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const { task, completed } = req.body;
  const index = todos.findIndex(todo => todo.id === id);
  if (index !== -1) {
    todos[index] = { id, task, completed };
    res.json(todos[index]);
  } else {
    res.status(404).json({ message: 'Todo not found' });
  }
});

app.delete('/todos/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const index = todos.findIndex(todo => todo.id === id);
  if (index !== -1) {
    todos.splice(index, 1);
    res.status(204).send();
  } else {
    res.status(404).json({ message: 'Todo not found' });
  }
});

app.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});
