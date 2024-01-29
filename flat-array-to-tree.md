Для того чтобы преобразовать плоский массив с элементами, содержащими `ParentID`, в древовидную (иерархическую) структуру, можно воспользоваться следующим алгоритмом на языке JavaScript.

Предположим, у вас есть массив объектов следующего вида:

```json
[
  {"id": 1, "title": "Item 1", "parentID": null},
  {"id": 2, "title": "Item 2", "parentID": null},
  {"id": 3, "title": "Item 1.1", "parentID": 1},
  {"id": 4, "title": "Item 1.2", "parentID": 1},
  {"id": 5, "title": "Item 2.1", "parentID": 2},
  {"id": 6, "title": "Item 2.2", "parentID": 2},
  {"id": 7, "title": "Item 2.2.1", "parentID": 6}
]
```

Следующий JavaScript код преобразует этот массив в древовидную структуру:

```javascript
function listToTree(data, options) {
  options = options || {};
  var ID_KEY = options.idKey || 'id';
  var PARENT_KEY = options.parentKey || 'parentID';
  var CHILDREN_KEY = options.childrenKey || 'children';

  var tree = [],
      childrenOf = {};
  var item, id, parentId;

  for (var i = 0, length = data.length; i < length; i++) {
    item = data[i];
    id = item[ID_KEY];
    parentId = item[PARENT_KEY] || 0;
    // инициализация children для каждого элемента
    childrenOf[id] = childrenOf[id] || [];
    // добавление детей текущему элементу в массив
    item[CHILDREN_KEY] = childrenOf[id];
    if (parentId != 0) {
      // если это не корень дерева, добавляем элемент к массиву children его родителя
      childrenOf[parentId] = childrenOf[parentId] || [];
      childrenOf[parentId].push(item);
    } else {
      tree.push(item);
    }
  }

  return tree;
}

var flatData = [
  {"id": 1, "title": "Item 1", "parentID": null},
  {"id": 2, "title": "Item 2", "parentID": null},
  {"id": 3, "title": "Item 1.1", "parentID": 1},
  {"id": 4, "title": "Item 1.2", "parentID": 1},
  {"id": 5, "title": "Item 2.1", "parentID": 2},
  {"id": 6, "title": "Item 2.2", "parentID": 2},
  {"id": 7, "title": "Item 2.2.1", "parentID": 6}
];

var treeData = listToTree(flatData);

console.log(treeData);
```

В результате выполнения этого кода получим массив `treeData`, который представляет собой древовидную структуру:

```json
[
  {
    "id": 1,
    "title": "Item 1",
    "parentID": null,
    "children": [
      {
        "id": 3,
        "title": "Item 1.1",
        "parentID": 1,
        "children": []
      },
      {
        "id": 4,
        "title": "Item 1.2",
        "parentID": 1,
        "children": []
      }
    ]
  },
  {
    "id": 2,
    "title": "Item 2",
    "parentID": null,
    "children": [
      {
        "id": 5,
        "title": "Item 2.1",
        "parentID": 2,
        "children": []
      },
      {
        "id": 6,
        "title": "Item 2.2",
        "parentID": 2,
        "children": [
          {
            "id": 7,
            "title": "Item 2.2.1",
            "parentID": 6,
            "children": []
          }
        ]
      }
    ]
  }
]
```

В результате мы получили древовидную структуру, где каждый элемент включает свои дочерние элементы как массив в свойстве `children`. Корневые элементы (те, у которых нет родителя или значение `ParentID` равно `null`), помещаются во внешний массив.

Этот код работает путём создания временного хранилища `childrenOf`, в котором сохраняются все дети (дочерние узлы) для каждого id. Затем каждый элемент связывается с его детьми, и если элемент является корневым (без `ParentID`), он добавляется в итоговый массив `tree`.

Приведенная функция `listToTree` достаточно универсальна для работы с различными структурами данных и поддерживает кастомизацию ключей, используемых в массиве объектов. В данном примере использовались параметры по умолчанию (`id`, `parentID`, `children`).

Вы можете настроить эту функцию под свои нужды, изменяя названия ключей или расширяя функциональность, например, добавив сортировку дочерних элементов или задав условия для фильтрации элементов.

Этот алгоритм может быть адаптирован под любой другой язык программирования, такой как Python, Ruby, Java и т.д., с соответствующими структурами данных и синтаксисом.
