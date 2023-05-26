import requests
import mysql.connector
from flask import Flask, jsonify, request

app = Flask(__name__)


@app.route('/todos', methods=['GET'])
def get_todos():
    response = requests.get('https://jsonplaceholder.typicode.com/todos')
    todos = response.json()
    return jsonify(todos), response.status_code


@app.route('/todos', methods=['POST'])
def create_todo():
    data = request.get_json()
    response = requests.post('https://jsonplaceholder.typicode.com/todos', json=data)
    return jsonify(response.json()), response.status_code


@app.route('/todos/<int:todo_id>', methods=['DELETE'])
def delete_todo(todo_id):
    response = requests.delete(f'https://jsonplaceholder.typicode.com/todos/{todo_id}')
    return '', response.status_code


if __name__ == '__main__':
    app.run()
