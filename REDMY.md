## Практикум Работа с файлами 

**Задание 1**: Работа с JSON файлом
Задача:
Студентам предлагается JSON файл с данными о студентах. Необходимо создать программу, которая проведет анализ этих данных и выведет информацию на экран.
Прочитать данные из файла students.json.
Определить общее количество студентов в файле.
Найти студента с самым высоким возрастом и вывести его данные (имя, возраст, город).
Определить количество студентов, изучающих определенный предмет (например, Python).


**Задание 2**: Работа с CSV файлом
Задача:
Предоставить студентам файл в формате CSV с данными о продажах в компании. Задача - обработать этот файл и получить полезную информацию.
Считать данные из файла sales.csv.
Подсчитать общую сумму продаж за весь период.
Определить продукт с самым высоким объемом продаж и вывести его на экран.
Разделить данные на категории по месяцам и вывести общую сумму продаж для каждого месяца.  


**Задание 3**: Комбинированная работа с JSON и CSV
Задача:
Предоставить студентам два файла - JSON с информацией о сотрудниках и CSV с данными о их производительности. Задача - соединить эти данные для анализа.
Считать данные из файлов employees.json и performance.csv.
Сопоставить данные о производительности каждого сотрудника с их соответствующей информацией из JSON файла.
Определить среднюю производительность среди всех сотрудников и вывести ее.
Найти сотрудника с наивысшей производительностью и вывести его имя и показатель производительности.
Вот примеры данных для файлов JSON и CSV, которые можно использовать для заданий:


### Решение:

````
import json
import csv
from collections import defaultdict
from datetime import datetime

with open('students.json', 'r', encoding='utf-8') as file:
    data = json.load(file)

def count_students(students):
    return len(students)

def max_age(data):
    students = max(data, key=lambda students: students["возраст"])
    filtered = {k: students[k] for k in ("имя", "возраст", "город")}
    return filtered

def count_students_by_subject(students, subject):
    count = 0

    for student in students:
        if "предметы" in student and subject in student["предметы"]:
            count += 1
    return count

print("Всего студентов:", count_students(data))
print("Самый старший студент:", max_age(data))
print("Изучают Python:", count_students_by_subject(data, "Python"))



def read_sales_data(filename):
    with open(filename, 'r', encoding='utf-8') as csvfile:
        return list(csv.DictReader(csvfile))

def total_amount(sales_data):
    return sum(int(row[" Сумма"]) for row in sales_data)

def product_with_the_highest_sales(sales_data):
    top_product = defaultdict(int)

    for row in sales_data:
        product = row[" Продукт"]
        top_product[product] += 1
    most_common = max(top_product, key=top_product.get)
    return most_common, top_product[most_common]

sales_data = read_sales_data("sales.csv")

product, count = product_with_the_highest_sales(sales_data)
print(f"Самый популярный продукт: {product} ({count} продаж)")

def sales_by_month(sales_data):
    monthly_sales = defaultdict(list)
    monthly_totals = defaultdict(int)

    for row in sales_data:
        date = datetime.strptime(row["Дата"], "%Y-%m-%d")
        month = date.strftime("%Y-%m")
        product = row[" Продукт"]
        amount = int(row[" Сумма"])

        monthly_sales[month].append((date.strftime("%d.%m.%Y"), product, amount))
        monthly_totals[month] += amount

    return monthly_sales, monthly_totals

sales_data = read_sales_data("sales.csv")
months, totals = sales_by_month(sales_data)


for month in sorted(months):
    print(f"Месяц: {month}")
    for sale in months[month]:
        print(f"  - Дата: {sale[0]} | Продукт: {sale[1]} | Сумма: {sale[2]}")
    print(f"Итог за месяц: {totals[month]} руб.")


sales_data = read_sales_data("sales.csv")
print(f'Общая сумма продаж за весь период: {total_amount(sales_data)}')

def read_employees(filename):
    with open (filename, 'r', encoding='utf-8') as file:
        return json.load(file)

def read_performance(filename):
    performance = {}

    with open(filename, 'r', encoding='utf-8') as file:
        reader = csv.DictReader(file)
        for row in reader:
            emp_id = int(row["employee_id"])
            perf = int(row[" performance"])
            performance[emp_id] = perf

    return performance

def merge_data(employees, performance):
    merged = []

    for employee in employees:
        emp_id = employee["id"]
        perf = performance.get(emp_id, 0)
        employee["performance"] = perf
        merged.append(employee)

    return merged


employees = read_employees("employees.json")
performance = read_performance("performance.csv")

merged = merge_data(employees, performance)

print("\nСписок сотрудников с производительностью:")
for employee in merged:
    print(f"{employee["имя"]} ({employee["должность"]}): {employee["performance"]}")

def calculate_average_performance(merged_data):
    if not merged_data:
        return 0
    total = sum(emp["performance"] for emp in merged_data)
    return total / len(merged_data)

average_perf = calculate_average_performance(merged)
print(f"\nСредняя производительность: {average_perf:.2f}")

def find_top_performer(merged_data):
    return max(merged_data, key=lambda emp: emp["performance"])

top_employee = find_top_performer(merged)
print(f"\nЛучший сотрудник: {top_employee["имя"]} ({top_employee["performance"]})")


````
