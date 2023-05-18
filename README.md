1. Проверяем добавление питомца без имени, статуса или URL-адреса фотографии (400 Bad Request):
invalid_pet = {'status': 'available'}
response = requests.post('https://petstore.swagger.io/v2/pet', json=invalid_pet)
assert response.status_code == 400

2. Пробуем добавить питомца со слишком длинным именем (400 Bad Request):
invalid_pet = {'name': 'ThisIsAVeryVeryVeryVeryVeryVeryLongNameThatExceedsTheCharacterLimit', 'status': 'available', 'photoUrls': []}
response = requests.post('https://petstore.swagger.io/v2/pet', json=invalid_pet)
assert response.status_code == 400

3. Проверяем добавление питомца с недопустимым статусом (400 Bad Request):
invalid_pet = {'name': 'Fluffy', 'status': 'invali', 'photoUrls': []}
response = requests.post('https://petstore.swagger.io/v2/pet', json=invalid_pet)
assert response.status_code == 400

4. Тестовый поиск питомцев по недопустимому статусу (должен вернуть пустой список):
response = requests.get('https://petstore.swagger.io/v2/pet/findByStatus', params={'status': 'invalid'})
assert response.json() == []

5. Тестируем обновление несуществующего питомца (404 Not Found):
updated_pet = {'id': 123456789, 'name': 'Fluffy', 'status': 'available', 'photoUrls': []}
response = requests.put('https://petstore.swagger.io/v2/pet', json=updated_pet)
assert response.status_code == 404

6. Проверяем обновление питомца с недопустимым идентификатором (400 Bad Request):
updated_pet = {'id': 'invalid_id', 'name': 'Fluffy', 'status': 'available', 'photoUrls': []}
response = requests.put('https://petstore.swagger.io/v2/pet', json=updated_pet)
assert response.status_code == 400

7. Проверяем обновление питомца со слишком длинным URL-адресом фотографии (400 Bad Request):
updated_pet = {'id': 1, 'name': 'Fluffy', 'status': 'available', 'photoUrls': ['https://www.example.com/photo/1', 'https://www.example.com/photo/2', 'https://www.example.com/photo/3', 'https://www.example.com/photo/4', 'https://www.example.com/photo/5', 'https://www.example.com/photo/6', 'https://www.example.com/photo/7', 'https://www.example.com/photo/8', 'https://www.example.com/photo/9', 'https://www.example.com/photo/10', 'https://www.example.com/photo/11']}
response = requests.put('https://petstore.swagger.io/v2/pet', json=updated_pet)
assert response.status_code == 400

8. Проверяем удаление несуществующего питомца (404 Not Found):
response = requests.delete('https://petstore.swagger.io/v2/pet/123456789')
assert response.status_code == 404

9. Проверяем удаление питомца с неверным идентификатором (400 Bad Request):
response = requests.delete('https://petstore.swagger.io/v2/pet/invalid_id')
assert response.status_code == 400

10. Проверяем добавление нового питомца с повторяющимся идентификатором (409 Conflict):
duplicate_pet = {'id': 1, 'name': 'Duplicate Pet', 'status': 'available', 'photoUrls': []}
response = requests.post('https://petstore.swagger.io/v2/pet', json=duplicate_pet)
assert response.status_code == 409
