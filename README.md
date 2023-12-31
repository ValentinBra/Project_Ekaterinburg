# Модель города Екатеринбург
В репозитории представлен исходный код и материалы, необходимые для разработки модели города Екатеринбург с применением библиотеки CityGeoTools, которую разработал Национальный центр когнитивных разработок Университета ИТМО. Модель предназначена для анализа и визуализации географических данных о городе Екатеринбург, а также для решения разнообразных задач в области геоинформатики. Проект в настоящее время находится в стадии разработки и выполняется командой из четырех участников:

1. Валентин Марковский
2. Ирина Савина
3. Жанна Антохина
4. Дмитрий Ким

## Задание 1
На первом этапе устанавливаются зависимости requirements.txt, необходимые для работы библиотеки, а затем создается [информационная модель](https://github.com/ValentinBra/Project_Ekaterinburg/blob/main/Task1/model_city.ipynb) города с местной crs = 32641 (EPSG: 32641, WGS 84 / UTM zone 41N).

## Задание 2 
Далее происходит [предобработка](https://github.com/ValentinBra/Project_Ekaterinburg/blob/main/Task2/Preprocessing.ipynb) заранее собранного датасета с информацией о жилых зданиях в выбранном городе: удаление ненужных атрибутов, исправление и унификация типов данных, восполнение пропущенных значений с помощью модели регрессии RandomForestRegressor. Модель регрессии с использованием ансамблевого метода ("случайного леса") выбрана для лучшего качества регрессии (предсказания нужного параметра). Восстановление данных о жилой площади происходит с помощью данных об общей площади, количестве этажей и признака жилого помещения.

Затем визуализируем распределение признаков датасета, отображаем датасет на [карте](https://github.com/ValentinBra/Project_Ekaterinburg/blob/main/Task2/map_explore.html) с помощью библиотеки matplotlib.
![image](https://github.com/ValentinBra/Project_Ekaterinburg/assets/115071564/4279fa3f-c479-4cf8-aefa-a01eae428063)


## Задание 3

Для кластеризации готовим .geojson файлы с информацией о:

* зданиях (жилых и нежилых), согласно спецификации CityGeoTools/data_specification/spacematrix/Buildings.json ;
* кварталах, согласно спецификации  CityGeoTools/data_specification/spacematrix/Blocks.json ; 
* городских сервисах, согласно спецификации CityGeoTools/data_specification/services_clusterization/Services.json ; 

Загружаем файлы в модель города, которую создали на первом этапе. 

<h3/>Кластеризация кварталов</h3>
<br/>Проводим кластеризацию кварталов с помощью метода SpaceMatrix из библиотеки и визуализируем результаты.

Пример визуализации для Чкаловского района города Екатеринбурга.
1. Код кластеризации доступен в файле [Сlustering_Space_Matrix](https://github.com/ValentinBra/Project_Ekaterinburg/blob/main/Task3/Сlustering_Space_Matrix.ipynb)
2. Карта Space Matrix досутпна в файле [space_matrix](https://github.com/ValentinBra/Project_Ekaterinburg/blob/main/Task3/clusters_satellite.html)
<img width="729" alt="Снимок экрана 2023-12-13 в 23 21 01" src="https://github.com/ValentinBra/Project_Ekaterinburg/assets/74907402/5cffe13f-43da-4406-beb2-93d7e0e3a276">

<h3/>Кластеризация сервисов для выявления городских "подцентров"</h3>

<br/>Для кластеризации выбрано 5 типов сервисов: 

<br/>* банк; 
<br/>* школа; 
<br/>* фитнес-клуб;
<br/>* кинотеатр;
<br/>* отель.

1. Код кластеризации доступен в файле [Сluster_services](https://github.com/ValentinBra/Project_Ekaterinburg/blob/main/Task3/Сluster_services.ipynb)
2. Карта с кластеризацией досутпна в файле [clusters_satellite](https://github.com/ValentinBra/Project_Ekaterinburg/blob/main/Task3/space_matrix.html)
<img width="1439" alt="Снимок экрана 2023-12-14 в 04 22 33" src="https://github.com/ValentinBra/Project_Ekaterinburg/assets/74907402/9757c0a5-946e-4671-b5d1-e41f6c52af63">


## Задание 4

Расчет доступности сервисов в городе производится на основе интермодального графа в формате GraphML. Пешеходный, автомобильный и транспортный графы загружаются с помощью библиотеки osmnx, собирается в интермодальный и загружается в модель города. 

На основе графа и датасета со зданиями строятся изохроны: 
* 10-минутной пешеходной доступности ;
* 5-минутной автомобильной доступности ;
* 20-минутной доступности на общественном транспорте ; 

<h3/>Построение изохрон</h3>
Получение изохрон и их визуализация выполнена в файле [Task4](https://github.com/ValentinBra/Project_Ekaterinburg/blob/main/Task4/Task4.ipynb)

Изохрона 10-минутной пешеходной доступности
![image](https://github.com/ValentinBra/Project_Ekaterinburg/assets/115021474/81c838cd-7066-42cd-9e03-ae1a050c2010)

Изохрона 5-минутной автомобильной доступности
![image](https://github.com/ValentinBra/Project_Ekaterinburg/assets/115021474/17c34093-6931-46ee-811a-b14b05eef21f)

Изохрона 20-минутной доступности на общественном транспорте
![image](https://github.com/ValentinBra/Project_Ekaterinburg/assets/115021474/9b1a2f9e-e715-44c1-a376-1105dc0eae03)

<h3/>Выборка зданий по изохроне</h3>
Здания в пешеходной изохроне
[buildings_in_walk](https://github.com/ValentinBra/Project_Ekaterinburg/blob/main/Task4/data_output/buildings_in_walk.geojson)

![image](https://github.com/ValentinBra/Project_Ekaterinburg/assets/115021474/b552ebb1-4d81-4b66-a7a5-071de9554efe)

Здания в автомобильной изохроне
[buildings_in_drive](https://github.com/ValentinBra/Project_Ekaterinburg/blob/main/Task4/data_output/buildings_in_drive.geojson)

![image](https://github.com/ValentinBra/Project_Ekaterinburg/assets/115021474/c4b85a12-424b-4915-a27c-5480c0e6fe91)

Здания в изохроне общественного транспорта
[buildings_in_pb](https://github.com/ValentinBra/Project_Ekaterinburg/blob/main/Task4/data_output/buildings_in_pb.geojson)

![image](https://github.com/ValentinBra/Project_Ekaterinburg/assets/115021474/7a858632-2408-4bdc-9336-1f5749692bc9)

## Задание 5

<h3/>5.1 Оценка многообразия школ в кварталах Екатеринбурга</h3>
Для оценки многообразия сервисов были выбраны школы как один из самых востребованных сервисов социальной инфраструктуры (относится к типу rival - конкурентный, так как школы имеют ограниченную вместимость учеников). [Метод](https://github.com/ValentinBra/Project_Ekaterinburg/blob/main/Task5/Provision_count.ipynb) распределения школ по кварталам позволяет узнать количество школ в квартале и затем дать общую оценку - сколько учеников они могут вместить, и соотносится ли это число с количеством детей в квартале.
  
![image](https://github.com/ValentinBra/Project_Ekaterinburg/assets/115021474/70354331-aad2-43fa-88ca-b8fc24ec4883)

<h3/>5.2 Оценка обеспеченности населения школами SFCA</h3>

Для метода используем датасеты с жилыми домами, школами и дорожным графом. На их основе рассчитываем расстояния от каждого дома до каждой школы - создаем матрицу расстояний. Затем получаем матрицу вероятностей и узнаем, с какой вероятностью будет совершен выбор в пользу конкретной школы. На основе заселенности домов и вероятности расчитываем и суммируем спрос, а также получаем показатель удовлетворения спроса каждой отдельной школы.

Итоговым показателем будет обеспеченность школой жителей каждого дома Екатеринбурга. Получаем тепловую карту обеспеченности территорий Екатеринбурга школами, на которой видны очаги недостатка школ. Результаты можно использовать для принятия управленческих решений при разработке плана строительства социальных объектов.  

Недостатком метода является то, что он не учитывает субъективные факторы выбора школы: репутация в городе, качество образования, наличие уникальной образовательной программы, отношения с пед.составом и другие. Это влияет на вероятность выбора учреждения и спрос.
 
<img width="1334" alt="Снимок экрана 2023-12-19 в 17 04 37" src="https://github.com/ValentinBra/Project_Ekaterinburg/assets/74907402/47335eb3-8ed5-4b95-9591-c9c8bb852fdb">

