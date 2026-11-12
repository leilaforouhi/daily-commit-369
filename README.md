
import secrets
import string
import json
from pathlib import Path

FILE = Path("api_keys.json")

def load_keys():
    if FILE.exists():
        return json.loads(FILE.read_text(encoding="utf-8"))
    return []

def save_keys(keys):
    FILE.write_text(
        json.dumps(keys, indent=4),
        encoding="utf-8"
    )

def generate_key(length):
    characters = string.ascii_letters + string.digits

    return "".join(
        secrets.choice(characters)
        for _ in range(length)
    )

def main():
    keys = load_keys()

    while True:
        print("\n1. Generate API key")
        print("2. Show saved keys")
        print("3. Delete all keys")
        print("4. Exit")

        choice = input("Choose: ")

        if choice == "1":
            try:
                length = int(input("Key length (16-128): "))

                if not 16 <= length <= 128:
                    print("Length must be between 16 and 128.")
                    continue

                key = generate_key(length)
                keys.append(key)

                save_keys(keys)

                print("\nGenerated API key:")
                print(key)

            except ValueError:
                print("Invalid length.")

        elif choice == "2":
            if not keys:
                print("No saved keys.")
            else:
                for index, key in enumerate(keys, 1):
                    print(f"{index}. {key}")

        elif choice == "3":
            confirm = input("Delete all keys? (yes/no): ")

            if confirm.lower() == "yes":
                keys.clear()
                save_keys(keys)
                print("All keys deleted.")

        elif choice == "4":
            break

        else:
            print("Invalid choice.")

if __name__ == "__main__":
    main()
