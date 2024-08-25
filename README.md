# Практика по UML

## Лабораторная работа 1. Создание диаграммы вариантов использования (диаграммы прецедентов)

### Задание:
- Выделить действующих лиц и прецеденты. 
- Создать диаграмму вариантов использования, в которой будут заданы прецеденты и действующие лица.  
- Вставить отношения между вариантами использования и действующими лицами

**Вариант 3. «Предприятие по сборке и продаже компьютеров».**

### Действующие лица:

- Менеджер по работе с клиентами – сотрудник, который работает с заказчиком и его заказом. 
- Менеджер по снабжению – сотрудник, занимающийся закупкой необходимых комплектующих. 
- Инженер по сборке настольных компьютеров – сотрудник, который занимается сборкой настольных компьютеров. 
- Инженер по сборке ноутбуков – сотрудник, занимающийся сборкой ноутбуков. 
- Инженер по тестированию – сотрудник, который занимается тестированием компьютеров. 
- Завскладом – сотрудник, заведующий складом комплектующих частей.

### Выделим прецеденты:
- Работа с заказом – позволяет менеджеру по работе с клиентами выполнять действия с заказом (добавлять, изменять, удалять). 
- Управление информацией о клиенте – дает возможность менеджеру по работе с клиентами добавлять или удалять клиентов, а также просматривать информацию о них. 
- Управление информацией о поставщиках – позволяет менеджеру по снабжению добавлять или удалять поставщиков. 
- Управление информацией о комплектующих – дает возможность просматривать информацию о комплектующих, производить анализ расходования, делать заказы. 
- Сборка компьютеров – позволяет инженеру по сборке просматривать наряды на сборку компьютеров и отмечать ход выполнения работы. 
- Требование необходимых комплектующих – предназначено для запроса инженером по сборке необходимых запчастей со склада.
- Учет поступления и выдачи комплектующих – позволяет завскладом вести учет поступления и выдачи запчастей со склада.

Для удобства связи «Инженер по сборке настольных компьютеров» и «Инженер по сборке ноутбуков» можно объединить, добавив еще одно действующее лицо – «Инженер по сборке».

**Диаграмма:**
![](Photos/1_usecase.png)

**Код**:

```plantumlcode
@startuml usecase_lab1
title "Usecase для Предприятия по сборке и продаже компьютеров"
left to right direction

actor "Менеджер по работе с клиентами" as manag_clients
actor "Менеджер по снабжению" AS manag_snab
actor "Инженер по сборке настольных компьютеров" as engi_comp
actor "Инженер по сборке ноутбуков" as engi_notebooks
actor "Инженер по тестированию" as engi_auto
actor "Завскладом" as zavsklad
actor "Инженер по сборке" as engi_sbor

usecase "Работа с заказом" as order
usecase "Управление информацией о клиенте" as client_info
usecase "Управление информацией о поставщиках" as postavka_info
usecase "Упарвление информацией о комплектующих" as parts_info
usecase "Сборка компьютеров" as comp_crt
usecase "Требование необходимых комплектующих" as comp_req
usecase "Учет поступления и выдачи комплектующих" as comp_uch
usecase "Тестирование компьютеров" as comp_test

manag_clients -- order
client_info .> order : <<extend>>

manag_snab -- postavka_info
note right of postavka_info
    Позволяет добавлять или удалять поставщиков
end note
manag_snab -- parts_info

engi_sbor -- comp_crt
comp_req <. comp_crt : <<include>>
engi_comp -down--|> engi_sbor
engi_notebooks -down--|> engi_sbor

zavsklad -- comp_uch

engi_auto -- comp_test
@enduml
```

## Лабораторная работа 2. Создание диаграммы классов.

### Задание: 

Создать диаграмму классов для определенного прецедента из лабораторной работы №1, задав атрибуты и операции класса.

Первым создаваемым классом будет класс «Клиент», в котором будут содержаться следующие атрибуты: 
- ИМЯ (ФИО клиента); 
- АДРЕС (адрес клиента); 
- ТЕЛЕФОН (номер телефона клиента); 
Операции: 
- ДОБАВИТЬ КЛИЕНТА; 
- УДАЛИТЬ КЛИЕНТА; 
- ПОЛУЧИТЬ ИНФОРМАЦИЮ. 

Класс «Заказ». 
Атрибуты: 
- НОМЕР ЗАКАЗА; 
- ДАТА ОФОРМЛЕНИЯ; 
- ДАТА ВЫПОЛНЕНИЯ. 
Операции:
- СОЗДАНИЕ НОВОГО ЗАКАЗА;
- ЗАНЕСТИ ИНФОРМАЦИЮ; 
- ПОЛУЧИТЬ ИНФОРМАЦИЮ. 

Класс «Комплектующее изделие». 
Атрибуты: 
- НАИМЕНОВАНИЕ; 
- ПРОИЗВОДИТЕЛЬ; 
- ЦЕНА; 
- ОПИСАНИЕ. 
Операции: 
- ДОБАВИТЬ НОВОЕ КОМПЛЕКТУЮЩЕЕ; 
- УДАЛИТЬ КОМПЛЕКТУЮЩЕЕ; 
- ПОЛУЧИТЬ ИНФОРМАЦИЮ (о комплектующем). 

Класс «Состав заказа». 
Атрибуты: 
- НОМЕР ПУНКТА ЗАКАЗА; 
- КОЛИЧЕСТВО КОМПЛЕКТУЮЩИХ; 
- ЦЕНА. 
Операции: 
- СОЗДАТЬ СТРОКУ ЗАКАЗА; 
- ДОБАВИТЬ ИНФОРМАЦИЮ (о строке); 
- ПОЛУЧИТЬ ИНФОРМАЦИЮ (о строке).

**Диаграмма**:
![](Photos/2_class.png)

**Код**:

```plantumlcode
@startuml class_lab2
title "Диаграмма классов"
left to right direction
allowmixing

class "Client" {
    fio
    address
    phoneNum
    addClient()
    deleteClient()
    getInfo()
}

class "Product" {
    productName
    manufacturer
    price
    description
    addNewProduct()
    deleteProduct()
    getProductInfo()
}

package "OrderPack" {
    class "Order" {
        orderNum
        orderDate
        orderCompleteDate
        newOrderCreate()
        addOrderInfo()
        getOrderInfo()
    }

    class "OrderStructure" {
        pvzNum
        productCount
        orderPrice
        createOrderRow()
        addRowInfo()
        getRowInfo()
    }
    
    boundary "workingOrderParams"
    boundary "addNewOrder"
    control "orderManager"
}

Client "1" -- "1..*" Order : оформляет
OrderStructure "1..*" --* "1" Order: входит в
Product "1" --o "1..*" OrderStructure
addNewOrder "1" --o "1" workingOrderParams

addNewOrder "1" --> "1"  orderManager
orderManager "1" --> "1..*" Order
@enduml
```
