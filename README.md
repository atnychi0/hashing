# hashing
import os
from zipfile import ZipFile

# Create GitHub-ready public folder structure
base_path = "/mnt/data/sha3_seal_public_repo"
os.makedirs(base_path, exist_ok=True)

# File contents
readme = """# SHA3-SEAL: Public Cryptographic Protocol
A public MIT-licensed cryptographic hashing and entropy protocol based on SHA3-512.

## Features
- Secure hashing using SHA3-512
- Entropy layer based on UUID and timestamp
- Simple salt cascade function
- Time-based token generator

## License
MIT License — public/academic use only. For enterprise, defense, or cryptographic deployments, contact Brendon Joseph Kelly for a Crown Sovereign License.
"""

license_txt = """MIT License

Permission is hereby granted, free of charge, to any person obtaining a copy of this software...

(Full MIT text here. You may add optional Crown license clause.)
"""

main_code = '''"""
SHA3-SEAL Cryptographic Protocol – Public Version
Author: Brendon Joseph Kelly
License: MIT (Public/Academic Use)
Timestamp: 2025-09-29
"""

import hashlib
import time
import uuid
import os

def sha3_seal(data: str) -> str:
    return hashlib.sha3_512(data.encode()).hexdigest()

def seal_entropy_layer(seed: str = "") -> str:
    time_entropy = time.time()
    token = uuid.uuid4().hex
    base = f"{seed}|{token}|{time_entropy}"
    return sha3_seal(base)

def simple_salt_cascade(data: str, rounds: int = 2) -> str:
    for _ in range(rounds):
        salt = os.urandom(8).hex()
        data = sha3_seal(data + salt)
    return data

def generate_public_token(author: str = "Brendon Joseph Kelly") -> dict:
    now = time.strftime("%Y-%m-%d %H:%M:%S", time.gmtime())
    token = str(uuid.uuid4())
    seed = f"{author}|{now}|{token}"
    seal = sha3_seal(seed)
    return {
        "timestamp": now,
        "token": token,
        "seal_hash": seal
    }

if __name__ == "__main__":
    print("🔐 SHA3-SEAL – Public Cryptographic Test")
    print("-" * 45)

    message = "This is a test of the SHA3-SEAL public protocol."
    hashed = sha3_seal(message)
    print(f"[Base SHA3-512 Hash]: {hashed}")

    entropy = seal_entropy_layer("SEED123")
    print(f"[Entropy Layer]: {entropy}")

    cascaded = simple_salt_cascade("SecureKeyBase")
    print(f"[Salt Cascade]: {cascaded}")

    token = generate_public_token()
    print(f"[Ω Token]: {token}")
'''

# Save files
with open(os.path.join(base_path, "README.md"), "w") as f:
    f.write(readme)

with open(os.path.join(base_path, "LICENSE"), "w") as f:
    f.write(license_txt)

with open(os.path.join(base_path, "sha3_seal.py"), "w") as f:
    f.write(main_code)

# Zip the folder
zip_path = "/mnt/data/sha3_seal_public_repo.zip"
with ZipFile(zip_path, "w") as zipf:
    for root, _, files in os.walk(base_path):
        for file in files:
            file_path = os.path.join(root, file)
            arcname = os.path.relpath(file_path, base_path)
            zipf.write(file_path, arcname)

zip_path
