import pytest
from app import app, redis_client

@pytest.fixture
def client():
    app.config['TESTING'] = True
    client = app.test_client()

    # Reset Redis before each test
    redis_client.flushdb()
    yield client

def test_home(client):
    response = client.get('/')
    assert response.status_code == 200
    assert "Redis Counter API" in response.get_json()["message"]

def test_counter_initial_value(client):
    response = client.get('/counter')
    assert response.status_code == 200
    data = response.get_json()
    assert data["counter"] == 0

def test_counter_increment(client):
    response = client.post('/counter/increment')
    assert response.status_code == 200
    data = response.get_json()
    assert data["counter"] == 1

    response = client.post('/counter/increment')
    data = response.get_json()
    assert data["counter"] == 2
