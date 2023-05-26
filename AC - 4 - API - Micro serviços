import requests
import mysql.connector

from flask import Flask, request, make_response, jsonify

app = Flask(__name__)

API_URL = 'http://127.0.0.1:5000'

con = mysql.connector.connect(
    host='localhost',
    port=3309,
    database='db_ApiExterna',
    user='root',
    password='my-secret-pw')


@app.route('/todos', methods=['GET'])
def get_todos():
    consulta_sql = "select * from tb_Todos"
    cursor = con.cursor()
    cursor.execute(consulta_sql)
    todosDoBancoDeDados = cursor.fetchall()

    todos = []
    for todo in todosDoBancoDeDados:
        dicTodo = {"userId": todo[0],
                   "id": todo[1],
                   "title": todo[2],
                   "completed": todo[3]
                   }
        todos.append(dicTodo)

    if (con.is_connected()):
        con.close()
        cursor.close()
        print("Conexão ao MySQL encerrada")


    return make_response(jsonify(todos), 200)


@app.route('/todos', methods=['POST'])
def create_todo():
    cursor = con.cursor()

    todo = request.get_json()

    userId = todo['userId']
    id = todo['id']
    title = todo['title']
    completed = todo['completed']

    cursor.execute('INSERT INTO tb_Todos (userId, id, title, completed) values (%s, %s, %s, %s)',
                   (userId, id, title, completed))

    con.commit()

    response = requests.post(f'{API_URL}/todos', json=todo)

    if (con.is_connected()):
        con.close()
        cursor.close()

    return make_response(jsonify(response.text), response.status_code)


@app.route('/todos/<int:id>', methods=['DELETE'])
def delete_todo(id):
    cursor = con.cursor()
    cursor.execute('DELETE FROM tb_Todos WHERE id = %s', id)
    con.commit()

    response = requests.delete(f'{API_URL}/todos/{id}')

    if (con.is_connected()):
        con.close()
        cursor.close()
        print("Conexão ao MySQL encerrada")

    return jsonify({'mensagem:' f'Cliente {id} excluido com sucesso'}), response.status_code


if __name__ == '__main__':
    app.run(debug=True, port=5001)
    
