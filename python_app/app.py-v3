from flask import Flask, jsonify
from redis_client import get_redis_client
import logging



app = Flask(__name__)
redis_client = get_redis_client()

# Logging Sensitive Information
@app.route('/debug')
def debug():
    logging.warning(f"Redis connection info: host={redis_client.connection_pool.connection_kwargs}")
    return jsonify({"status": "debug"})

@app.route('/')
def home():
    return jsonify({"message": "Welcome to the Redis Counter API!"})

@app.route('/counter/increment', methods=['POST'])
def increment():
    value = redis_client.incr('counter')
    return jsonify({"counter": value})

# Missing Input Validation
@app.route('/counter/increment/<amount>', methods=['POST']) # right one is <int:amount>
def increment_by(amount):
    value = redis_client.incrby('counter', amount)
    return jsonify({"counter": value})

# Missing Exception Handling
@app.route('/danger')
def danger():
    result = 10 / 0  # Division by zero
    return jsonify({"result": result})

@app.route('/counter', methods=['GET'])
def get_counter():
    value = redis_client.get('counter')
    if value is None:
        value = 0
    else:
        value = int(value)
    return jsonify({"counter": value})

# Duplicate Logic -- to be removed
@app.route('/counter/show', methods=['GET'])
def show_counter():
    value = redis_client.get('counter')
    if value is None:
        value = 0
    else:
        value = int(value)
    return jsonify({"counter": value})

# Unused Code
def unused_function():
    x = "This function is never called"
    return x


if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
