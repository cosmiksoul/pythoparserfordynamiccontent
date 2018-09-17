# Python parser for dynamic content


```python
#импортим необходимые библиотеки, нам понадобяться селенимум и BeautifulSoup
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.select import Select
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from bs4 import BeautifulSoup 
```


```python
#инициализируем переменные, по которым будет работать парсер
#сохраняем путь к вебдрайверу в переменную
path_to_chromedriver = 'D:/Soft/chromedriver_win32/chromedriver.exe'
#адрес сайта, который будем парсить
page_link = 'https://warspot.ru/'
#также, для удобства, сохраняем класс элемента, который будем парсить.
parse_element = 'a.cm-showcase-related_link'
#метод парсинга для BeautifulSoup 
parsing_method = 'html.parser'
#массив для вывода
output_array = []
```


```python
#подключаем вебдрайвер из селениума
driver = webdriver.Chrome(path_to_chromedriver)
#запускаем вебдрайвер, передаем ему адресс сайта для парсинка
driver.get(page_link)
```


```python
#врубаем ожидание, чтобы динамические элементы успели подгрузиться
#на этом этапе выполнения скрипта у вас должно открыться окно вебдравера, в котором будет загружен сайт.
#вы должны выполнить в окне вебдрайвера необходимые действия для подгрузки динамического контента на сайте
wait = WebDriverWait(driver, 10)
wait.until(EC.visibility_of_element_located((By.CSS_SELECTOR, parse_element)))
```




    <selenium.webdriver.remote.webelement.WebElement (session="3ea81d9429666f9d85f2c4e5b10d0139", element="0.9321541506842028-1")>




```python
#забираем данные со страницы в переменную, закрываем драйвер
data = driver.page_source
driver.close()
```


```python
#передаем данные в BeautifulSoup , указываем, что необходимо вытащить
soup = BeautifulSoup(data, parsing_method)
```


```python
#функция поиска нужного контента объекте супа
#проходимся циклом по обхекту, ищем все элементы по нужному нам классу
#сохраняем атрибут href атрибут элемента в список, возращаем список
def get_parse_element_to_output(output_array_arg, soup_arg):
    
    for link in soup_arg.findAll(True,{'class': 'cm-showcase-related_link'}):
        output_array_arg.append(link.get('href'))
    
    return output_array_arg
```


```python
#инициализация функции, для удобства - выводим все элементы списка в одну строку с построчным разбиением
#для этого оборачиваем функцию в конструкцию "\n".join()
print("\n".join(get_parse_element_to_output(output_array, soup)))
```

    
