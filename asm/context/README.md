# Context

Состояние процессора определяется набором регистров. Набор значений регистров называют контекстом. С точки зрения программы на C, контекст - это текущая выполняющаяся строчка кода + значения некоторых локальных переменных.

В этой задаче нужно реализовать функции сохранения и восстановления контекста `savectx` и `jumpctx`.

Функция `savectx` сохраняет текущий контекст и возвращает 0. После этого, пользователь может вызвать `jumpctx` и исполнение программы продолжится так, будто функция `savectx` вернула 1.

Получается из функции `savectx` можно "вернуться несколько раз". В каком-то смысле это похоже на системный вызов `fork`.

Подобную семантику невозможно выразить на языке C, поэтому функции `savectx` и `jumpctx` можно реализовать только на ассемблере.

Сохранённый контекст описывается структурой:

```c
struct Context {
    void* rip;
    void* rsp;
    void* rbp;
};
```

- `rip` - указатель на инструкцию, которую должен начать выполнять процессор после вызова `jumpctx`.
  Требование "исполнение программы продолжится так, будто функция `savectx` вернула 1" можно выполнить, сохранив в `rip` адрес возврата из функции `savectx`.
- `rsp` - указатель на вершину стека. Нужно вычислить значение `rsp` соответствующее значению `rip` которое вы сохранили.
- `rbp` - указатель на начало фрейма. Достаточно просто сохранить это значение.
