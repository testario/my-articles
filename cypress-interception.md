# Подмена запроса в тестовом фреймворке Cypress

Для подмены (interception) запроса в Cypress можно использовать команду `cy.intercept()`. Это позволяет перехватить сетевые запросы на любом этапе теста и манипулировать как запросами, так и ответами. Ниже приведен пример использования `cy.intercept()` для подмены ответа на запрос.

Допустим, вам нужно перехватить GET-запрос к URL `/api/items` и вернуть собственный ответ:

```javascript
cy.intercept('GET', '/api/items', {
  statusCode: 200,
  body: [
    { id: 1, name: 'Item 1' },
    { id: 2, name: 'Item 2' }
  ],
  headers: { 'content-type': 'application/json' }
}).as('getItems');
```

В этом примере мы указали:

- Метод запроса `GET`.
- Путь запроса `/api/items`.
- Объект с имитацией ответа, включая код состояния (200), тело ответа (массив элементов), и заголовки.

Мы также использовали `.as()` чтобы дать этому запросу псевдоним `getItems` для последующего ожидания запроса или проверки.

Вот как вы могли бы использовать это в полном тесте:

```javascript
describe('Item API', () => {
  it('intercepts and mocks response', () => {
    // Настраиваем перехват запроса
    cy.intercept('GET', '/api/items', {
      statusCode: 200,
      body: [{ id: 1, name: 'Mock Item' }],
    }).as('getItems');

    // Посещаем страницу, которая делает запрос к /api/items
    cy.visit('/items-page');

    // Ожидаем выполнения перехваченного запроса
    cy.wait('@getItems');

    // После этого можем делать утверждения, основываясь на моках
    cy.contains('Mock Item').should('be.visible');
  });
});
```

В этом примере Cypress первоначально перехватывает запрос на `/api/items` и подменяет ответ на моковый. Когда тест посещает `items-page`, где осуществляется вызов API, Cypress использует подменённый ответ для этого вызова. После выполнения запроса (после `cy.wait('@getItems')`) тест может продолжаться, используя подменённые данные для последующих проверок на странице.

Это всего лишь один из способов использования `cy.intercept()`. Вы также можете перехватывать запросы, чтобы читать их данные, создавать динамические ответы на основе запроса, а также изменять запрос перед его отправкой.
