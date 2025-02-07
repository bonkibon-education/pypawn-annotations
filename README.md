# pypawn-annotation
Python библиотека, реализует автоматическую аннотацию для функций из языка программирования pawn.

- Установка последней версии модуля
```console
pip install pypawn-annotations
```

- Пример использования модуля. Аннотация к native функции:
```python
import pypawn_annotations

function = pypawn_annotations.PawnFunction(
    string="native bool: function(const iUser, const str: szLogin[32], const str: szPassword[32]);"
)

annotation = pypawn_annotations.Annotation(
    function=function, description=f'Description: this function authorizes the user'
)

print(annotation.show())
```

- Результат выполнения кода
```console
/* 
 * Description: function
 * 
 * @param const int:iUser            iUser
 * @param const str:szLogin[32]      szLogin[32]
 * @param const str:szPassword[32]   szPassword[32]
 * @return                           bool: 
 */
```

**Структура объекта PawnFunction**

- Модуль использует регулярные выражения для поиска групп.

1. Title: string - ключевое слово, обозначающее что строка является функцией (native, forward, func)
2. General tag: tuple - основной тэг функции, который содержит в себе два элемента:
 	
    - enum объект класса AnnotationTags
 	
    - название тэга
  
3. Name: string - имя функции
4. Args: list[dict] - массив словарей каждый из которых содержит в себе четыре ключа:
 	
    - qualifier: tuple - квалификатор, enum объект класса AnnotationQualifier
 	
    - tag: tuple - работает по принципу general tag
 	
    - name: string - имя аргумента

- Вывод в консоль. Cтруктура объекта PawnFunction:
```console
[0] string: native bool: function(const iUser, const str: szLogin[32], const str: szPassword[32]);
[1] title: native
[2] general_tag: (<AnnotationTags.TAG_CUSTOM: 4>, 'bool:')
[3] name: function
[4] args: [{'qualifier': <AnnotationQualifier.QUALIFIER_CONST: 1>, 'tag': (<AnnotationTags.TAG_INT: 0>, 'int'), 'name': 'iUser', 'description': 'iUser'}, {'qualifier': <AnnotationQualifier.QUALIFIER_CONST: 1>, 'tag': (<AnnotationTags.TAG_STR: 2>, 'str'), 'name': 'szLogin[32]', 'description': 'szLogin[32]'}, {'qualifier': <AnnotationQualifier.QUALIFIER_CONST: 1>, 'tag': (<AnnotationTags.TAG_STR: 2>, 'str'), 'name': 'szPassword[32]', 'description': 'szPassword[32]'}]
[5] is_title_tag_function: False
[6] is_title_tag_forward: False
[7] is_title_tag_native: True
[8] flags: 15
```


- Базовые параметры функций:
```python
functions: tuple = (
	"native global_tag: function_name(tag: name = value)",
	"forward global_tag: function_name(tag: name = value)",
	"func global_tag: function_name(tag: name = value)"

	"native global_tag: function_name(const tag: name = value)",
	"forward global_tag: function_name(const tag: name = value)",
	"func global_tag: function_name(const tag: name = value)"
)
```

- Поддержка Венгерской нотации об именовании переменных, для автоматического определения тегов
```python
functions: tuple = (
	"func global_tag: function_name(iHungarianNotation = value)", 
	"func global_tag: function_name(flHungarianNotation = value)",
	"func global_tag: function_name(szHungarianNotation = value)",
	"func global_tag: function_name(bHungarianNotation = value)"
)
```

## Больше примеров здесь:
https://github.com/bonkibon-education/pypawn-annotations-examples
