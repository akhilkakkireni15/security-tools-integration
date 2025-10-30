import redis
import os

def get_redis_client():
    redis_host = os.getenv("REDIS_HOST", "localhost")
    redis_port = int(os.getenv("REDIS_PORT", "6379"))
    redis_db = int(os.getenv("REDIS_DB", "0"))
    redis_password = os.getenv("REDIS_PASSWORD", "Password123!")  # Hardcoded Credentials (Security Hotspot)
    return redis.StrictRedis(host=redis_host, port=redis_port, db=redis_db, password=redis_password, decode_responses=True)
