# Aналитика

### Здесь представлена краткая выжимка из отчета, отчет предаставлен в документе отчет.docx

## Задание 1
### <img width="621" alt="4" src="https://github.com/Ma-li-na/Analitica/assets/174958120/6cc5135d-c1d4-4d07-8da7-c8103ed5fc2b">
## Задание 2
### Общая диаграмма
### <img width="497" alt="6" src="https://github.com/Ma-li-na/Analitica/assets/174958120/9226143d-b824-40bb-8fa0-ae2cb057f89e">
### ER-модель
### <img width="670" alt="5" src="https://github.com/Ma-li-na/Analitica/assets/174958120/abb84b8b-8043-4cf1-b0df-770caf4285ff">
### UML activity
### <img width="479" alt="7" src="https://github.com/Ma-li-na/Analitica/assets/174958120/dcdfa413-e944-4bb4-9eca-0223bd09381a">
### UML Sequence добавление товара
### ![image](https://github.com/Ma-li-na/Analitica/assets/174958120/8ac443e8-04a5-460f-b004-d0f432795b58)
### UML Sequence удаление товара и обновление корзины
### ![image](https://github.com/Ma-li-na/Analitica/assets/174958120/9339ae3d-a1cf-45f6-91eb-bccdfc0e2622)
## Задание 3
### Главный экран
### ![image](https://github.com/Ma-li-na/Analitica/assets/174958120/696f8316-1e41-4753-89d2-e9a1aa1cb00d)
### Экран результатов поиска
### ![image](https://github.com/Ma-li-na/Analitica/assets/174958120/f3792db0-e978-4912-8ce5-debeda2223e2)
### Экран добавление товара в корзину
### ![image](https://github.com/Ma-li-na/Analitica/assets/174958120/0d87e732-85f1-4d10-8f9b-39fb537e7670)
## Задание 4
### Прототип
### ![image](https://github.com/Ma-li-na/Analitica/assets/174958120/72752570-6214-4753-89c6-2794ecb2c02f)
### BPMN диаграмма
### ![image](https://github.com/Ma-li-na/Analitica/assets/174958120/27d1e760-3000-48d3-bc50-2f7b672dfac6)
### UML Sequence
### ![image](https://github.com/Ma-li-na/Analitica/assets/174958120/fae4ce0e-e3ab-4d22-9c1a-9ab82f596fa2)
### ER-модель
### ![image](https://github.com/Ma-li-na/Analitica/assets/174958120/18c85591-ebd9-4f33-94cb-24dfd38c0369)
## Задание 5
### Вывести покупателей с числом осуществленных покупок
SELECT Покупатели.Имя, Покупатели.Фамилия, COUNT(Покупки.идентификатор) 
FROM Покупатель 
JOIN Покупки 
ON Покупатели.идентификатор=Покупки.ключ_покупателя 
GROUP BY Покупатели.имя, Покупатели.Фамилия, Покупатели.идентификатор;
### Вывести общую стоимость товаров для каждого покупателя и отсортировать  результат в порядке убывания

•	Общая стоимость товаров в конкретной покупке

SELECT Покупатели.Имя, Покупатели.Фамилия, SUM(Товары.стоимость) 
FROM  Покупатели 
JOIN Покупки 
ON Покупатели.идентификатор=Покупки.ключ_покупателя  
JOIN Товары 
ON Товары.идентификатор=Покупки.ключ_товара 
GROUP BY Покупки.Идентификатор, Покупатели.Имя, Покупатели.Фамилия, По-купки.дата_покупки 
ORDER BY SUM(Товары.стоимость) DESC;

•	Общая стоимость товаров за все покупки покупателя

SELECT Покупатели.Имя, Покупатели.Фамилия, SUM(Товары.стоимость) 
FROM  Покупатели 
JOIN Покупки 
ON Покупатели.идентификатор=Покупки.ключ_покупателя  
JOIN Товары 
ON Товары.идентификатор=Покупки.ключ_товара 
GROUP BY Покупки.Идентификатор, Покупатели.Имя, Покупатели.Фамилия 
ORDER BY SUM(Товары.стоимость) DESC;

### Получить покупателей, купивших только один товар

•	Покупатель мог покупать товар несколько раз, но это всегда была одна одинаковая позиция

SELECT DISTINCT Покупатели.Имя, Покупатели.Фамилия, Покупате-ли.Идентификатор
FROM  Покупатели 
JOIN 
(SELECT Покупки.ключ_покупателя 
FROM Покупки 
GROUP BY Покупки.ключ_покупателя 
HAVING COUNT(DISTINCT Покупки.ключ_товара) = 1)
ON Покупатели.идентификатор=Покупки.ключ_покупателя;

•	Покупатель покупал только один раз в магазине только один то-вар

SELECT DISTINCT Покупатели.Имя, Покупатели.Фамилия, Покупате-ли.Идентификатор
FROM  Покупатели 
JOIN 
(SELECT Покупки.ключ_покупателя 
FROM Покупки 
GROUP BY Покупки.ключ_покупателя 
HAVING COUNT(DISTINCT Покупки.ключ_товара) = 1 AND COUNT(Покупки.идентификатор) = 1) 
ON Покупатели.идентификатор=Покупки.ключ_покупателя;

•	Покупатель хотя бы раз покупал только один товар в конкретной покупке

SELECT DISTINCT Покупатели.Имя, Покупатели.Фамилия, Покупате-ли.Идентификатор
FROM  Покупатели 
JOIN 
(SELECT Покупки.ключ_покупателя 
FROM Покупки 
GROUP BY Покупки.ключ_покупателя, Покупки.дата_покупки
HAVING COUNT(DISTINCT Покупки.ключ_товара) = 1)
ON Покупатели.идентификатор=Покупки.ключ_покупателя;





