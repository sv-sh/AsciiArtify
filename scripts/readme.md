
Зміни та вдосконалення:

Додана перевірка кількості переданих аргументів.
Додана можливість вказати тип ресурсу (по замовчуванню 'pod').
Замінено kubectl $2 на kubectl get $RESOURCE_TYPE.
Додано оголошення змінної NAMESPACE для читабельності коду.
Виправлено помилку у способі зчитування ліній з виводу kubectl get.
Щодо тестування як плагіну kubectl, слід скопіювати цей скрипт у директорію, яка міститься в вашому шляху пошуку, наприклад, /usr/local/bin. Потім ви можете використовувати його як плагін, наприклад:

kubectl plugin kubeplugin <namespace>

Інструкція з використання:

Запустіть скрипт kubeplugin з вказанням необхідного простору імен Kubernetes.
Спостерігайте за виведеними статистиками про використання ресурсів у форматі "Resource, Namespace, Name, CPU, Memory".