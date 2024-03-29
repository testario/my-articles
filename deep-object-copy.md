Для создания алгоритма глубокого поиска по произвольному полю в объекте (или вложенной структуре, например, дереве) мы можем использовать рекурсию. Ниже представлена реализация на JavaScript, которая делает именно это.

```javascript
function deepSearchById(object, key, keyToFind) {
  if (object[key].toString() === keyToFind.toString()) {
    return object;
  }

  if (typeof object === 'object') {
    for (let prop in object) {
      const value = object[prop];

      if (typeof value === 'object' && value !== null) {
        const found = deepSearchById(value, key, keyToFind);
        if (found) {
          return found;
        }
      }
    }
  }

  return null;
}

// Пример использования:

const data = {
  id: 1,
  name: 'Root',
  children: [
    {
      id: 2,
      name: 'Child1',
      children: [
        {
          id: 3,
          name: 'Grandchild1'
        },
        {
          id: 4,
          name: 'Grandchild2'
        }
      ]
    },
    {
      id: 5,
      name: 'Child2'
    }
  ]
};

const found = deepSearchById(data, "id", 4);
console.log(found);  // Выведет объект с id 4
```

Функция `deepSearchById` принимает целевой объект для поиска, ключ, по которому нужно найти объект и значение ключа, которое нужно найти, и делает следующее:

- Проверяет, совпадает ли ключ текущего объекта с искомым значением `keyToFind`. Если да, возвращает текущий объект.
- Если текущее значение - это объект (или массив, который также считается объектом), она перебирает его ключи и рекурсивно вызывает саму себя для каждого из вложенных объектов.
- Если объект с искомым ключом найден на каком-либо уровне вложенности, он возвращается в качестве результата функции.
- Если совпадение не найдено, функция возвращает `null`.

Обратите внимание, что такой алгоритм поиска может быть неэффективен для очень больших или глубоких структур данных из-за его рекурсивной природы, что может привести к большому использованию стека вызовов и даже к переполнению стека.
