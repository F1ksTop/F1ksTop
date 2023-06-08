import hashlib

class User:
    def __init__(self, username, password):
        self.username = username
        self.password_hash = self._hash_password(password)

    # Метод для хешування пароля
    def _hash_password(self, password):
        return hashlib.sha256(password.encode()).hexdigest()

    # Метод для перевірки правильності пароля
    def check_password(self, password):
        password_hash = self._hash_password(password)
        return password_hash == self.password_hash


class AuthenticationSystem:
    def __init__(self):
        self.users = {}

    # Метод для реєстрації нового користувача
    def register_user(self, username, password):
        if username in self.users:
            raise ValueError("Користувач з таким іменем вже зареєстрований.")

        user = User(username, password)
        self.users[username] = user

        print("Користувач успішно зареєстрований.")

  
    def authenticate_user(self, username, password):
        if username not in self.users:
            raise ValueError("Користувача з таким іменем не знайдено.")

        user = self.users[username]
        if not user.check_password(password):
            raise ValueError("Неправильний пароль.")

        print("Користувач успішно автентифікований.")


auth_system = AuthenticationSystem()


try:
    auth_system.register_user("user1", "password123")
except ValueError as e:
    print(f"Помилка реєстрації: {str(e)}")

try:
    auth_system.authenticate_user("user1", "password123")
except ValueError as e:
    print(f"Помилка автентифікації: {str(e)}")
