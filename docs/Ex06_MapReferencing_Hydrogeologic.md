# Привязка и векторизация (гидрогеологическая карта) {#map-ref-hydrogeologic}



[Скачать данные и файл отчета](https://github.com/tsamsonov/arcgis-course/raw/refs/heads/master/data/Ex06.zip)

## Введение {#map-ref-hydrogeologic-intro}

**Цель задания** — знакомство с привязкой, трансформированием и векторизацией геоизображений, элементами базовых технологий ГИС (оверлей, пространственные запросы).

Параметр                    Значение
--------------------------  --------
*Поток*                     Гидрометпоток, Физпоток  
*Теоретическая подготовка*  Системы координат и проекции на картах, привязка геоизображений, трансформирование геоизображений, векторизация геоизображений. Методы трансформации: аффинное, проективное, полиномиальное, метод резинового листа (сплайны). Пространственные запросы, атрибутивные запросы, оверлей.
*Практическая подготовка*   Знание основных компонент интерфейса ArcGIS Desktop (каталог, таблица содержания, карта). Работа с базой геоданных. Настройка символики и подписей объектов.
*Исходные данные*           Слои картографической основы (карт масштаба 1:2 500 000), растровая карта гидрогеологического районирования на район среднего течения Дона.
*Результат*                 Слой гидрогеологического районирования в базе данных. Слой рек, обогащенный данными о принадлежности участков рек к артезианским бассейнам (_дополнительно_). Проект карты с оформленной компоновкой
*Ключевые слова*            Системы координат, проекции, трансформирование координат, пространственная привязка, векторизация, оверлей, пространственные запросы, атрибутивные запросы

### Контрольный лист {#map-ref-hydrogeologic-control}

* Привязать растровую карту к опорным данным
* Создать в базе геоданных класс пространственных объектов для районов
* Наполнить класс районов путем цифрования растровой карты
* Заполнить названия районов и выполнить пространственный запрос рек по пересечению с разными бассейнами
* Осуществить оверлей c целью разбить реки на участки, принадлежащие разным бассейнам, выполнить атрибутивный запрос по принадлежности 
* Подготовить проект гидрогеологической карты с компоновкой

### Аннотация {#map-ref-hydrogeologic-annotation}

Задание посвящено знакомству с привязкой растровых карт, созданием и наполнением баз пространственных данных путем векторизации растра, использованию оверлея, пространственных и атрибутивных запросов (дополнительно). Эти методы входят в число базовых технологий геоинформатики.  В задании предлагается перевести в векторный вид карту гидрогеологического районирования среднего течения Дона и далее наполнить этой информацией участки рек, чтобы в дальнейшем определять источник питания участков рек по их принадлежности артезианским бассейнам.

> __Артезианские воды__ представляют собой одну из компонент питания рек. К артезианским водам относятся подземные воды, находящиеся в водоносных горизонтах, перекрытых и подстилаемых водоупорными (или относительно водоупорными) слоями горных пород, и обладающие гидростатическим напором. 

## Оформление базовых слоев {#map-ref-hydrogeologic-base}
[В начало упражнения ⇡](#map-ref-hydrogeologic)

1. Скопируйте каталог *Ex06* в свою папку.

1. Откройте приложение **ArcMap**, создайте новый документ карты и сохраните его в свою папку *Ex06*.

2. Подключитесь в окне каталога к папке *Ex06*. Убедитесь, что в ней находится база геоданных *Don.gdb* и растровый файл *DonArtesian.png*.

3. Раскройте базу геоданных и перенесите на карту классы *Lakes* и *Rivers*

4. Присвойте слою *Lakes* символ с голубой заливкой и синей обводкой.

5. Настройте оформление слоя *Rivers* следующим образом:

    * Выберите метод _Unique values_ (уникальные значения).

    * В качестве поля для отображения используйте атрибут *Тип*. Нажмите **Add All Values**.

    * Покрасьте все реки в синий цвет. Для этого щелкните на заголовке первого столбца **Symbol** и вызовите команду **Properties for all symbols**. Выберите *точно такой же цвет*, что вы использовали для обводки озер.

    * Установите следующие параметры толщины линий:

        Слой                      Толщина линии
        ------------------------- ---------
        *Реки постоянные крупные* 2 пункта
        *Реки постоянные средние* 1,5 пункта
        *Остальные классы*        1 пункт

    Диалог примет следующий вид (Рис. \@ref(fig:mrh-sym)):
    
    <div class="figure">
    <img src="images/Ex06/image7.png" alt="Настройка параметров толщины линий" width="80%" />
    <p class="caption">(\#fig:mrh-sym)Настройка параметров толщины линий</p>
    </div>

1. Включите опцию подписи рек. Перейдите на вкладку **Labels** и отметьте флажок **Label features in this layer**. Выберите поле *Название* в качестве поля для подписей, смените их цвет на темно-синий и установите криволинейное размещение вдоль линии. Нажмите ОК. В результате операции все реки будут подписаны.

2. Чтобы были подписаны только крупнейшие реки, необходимо использовать определяющий запрос на языке __SQL__. Для этого откройте снова свойства слоя и на вкладке **Labels** выберите метод «*Define classes of features and label each class separately*». Нажмите кнопку **SQL query…** и введите в поле следующий текст запроса:

    `"CLASS" = 2 OR "CLASS" = 3`

    Чтобы избежать ошибок ввода, вы можете дважды щелкнуть на поле `CLASS` в списке слева — оно подставится в запрос. Добавьте знак `=`. Далее нажмите кнопку **Get unique values** и найдите 2-й класс. Щелкните на нем дважды — после этого название поля подставится в текст запроса. После этого введите оператор `OR` и повторите ввод для 3-го класса. Диалог примет следующий вид (Рис. \@ref(fig:mrh-sql)):
    
    <div class="figure">
    <img src="images/Ex06/image8.png" alt="Диалог SQL-запроса" width="50%" />
    <p class="caption">(\#fig:mrh-sql)Диалог SQL-запроса</p>
    </div>

1. Нажмите __Verify__, чтобы проверить корректность введенного запроса. Если ответ положительный, закройте всплывшее сообщение. Если появляется сообщение об ошибке в запросе, убедитесь, что он введен в соответствии с примером.

1. Нажмите **ОК** в диалоге свойств слоя. Карта примет следующий вид (Рис. \@ref(fig:mrh-riv)):

    <div class="figure">
    <img src="images/Ex06/image9.png" alt="Диалог SQL-запроса" width="100%" />
    <p class="caption">(\#fig:mrh-riv)Диалог SQL-запроса</p>
    </div>

1. Сохраните документ карты в _свою_ папку *Ex06* под именем *Don.mxd*.

<kbd>**Снимок экрана №1** — *Реки*</kbd>

## Привязка растровой карты по контрольным точкам {#map-ref-hydrogeologic-referencing}
[В начало упражнения ⇡](#map-ref-hydrogeologic)

1. Добавьте на карту из базы данных слой *DonArtesian.png* и поместите его непосредственно под слоем Rivers. При добавлении слоя появится диалоговое окно, предупреждающее о том, что файл не имеет привязки. Закройте его.

2. Поместите карту в центр окна **ArcMap**.

    > Для выполнения дальнейшей работы прочтите раздел **[Привязка растровых данных](#manual-georef)** в приложении.

4. Откройте панель инструментов **Georeferencing**. Убедитесь, что в ее списке выбран файл *DonArtesian*. Выберите в ее меню команду **Fit to Display**, чтобы переместить непривязанный растр на середину области отображения.

4. Поместите растр непосредственно под слоем *Rivers*.

5. Используя инструмент расстановки контрольных точек, укажите пять контрольных точек в разных частях карты. Желательно, чтобы точки были равномерно распределены по полю карты (по краям и в центре) и *не располагались на одной линии* — это обеспечит хорошие коэффициенты трансформации. В качестве точек используйте места впадения притоков и впадения рек в водохранилища. Например, можно использовать точку впадения реки Хопёр в реку Дон (Рис. \@ref(fig:mrh-cpt), \@ref(fig:mrh-cpu)):

    <div class="figure">
    <img src="images/Ex06/image10.png" alt="Расстановка одной пары контрольных точек" width="50%" />
    <p class="caption">(\#fig:mrh-cpt)Расстановка одной пары контрольных точек</p>
    </div>

    <div class="figure">
    <img src="images/Ex06/image11.png" alt="Результат привязки изображения в окрестности одной пары контрольных точек" width="50%" />
    <p class="caption">(\#fig:mrh-cpu)Результат привязки изображения в окрестности одной пары контрольных точек</p>
    </div>

1. Ознакомьтесь с доступными методами трансформирования по контрольным точкам. Для этого в меню **Georeferencing** выберите команду **Transformation**. По умолчанию выбрано аффинное преобразование.

    > **Какие еще виды трансформирования доступны?** Чем проективное преобразование отличается от аффинного?

    Оставьте выбранным аффинное преобразование.

2. Осуществите трансформирование растра. На панели инструментов **Georeferencing** выберите команду меню **Georeferencing > Update Georeferencing**. Контрольные точки удалятся.

    Картографическое изображение примет следующий вид (Рис. \@ref(fig:mrh-ref)):

    <div class="figure">
    <img src="images/Ex06/image12.png" alt="Привязанная растровая карта" width="100%" />
    <p class="caption">(\#fig:mrh-ref)Привязанная растровая карта</p>
    </div>

    <kbd>**Снимок экрана №2** — *Привязанная растровая карта*</kbd>

3. Сохраните документ карты в формате mxd в папке отчета.

## Векторизация слоя гидрогеологического районирования {#map-ref-hydrogeologic-regions}
[В начало упражнения ⇡](#map-ref-hydrogeologic)

1. **[Создайте](#manual-gdb-create-class)** новый класс пространственных объектов в базе геоданных *Don.gdb*. Для этого:

    * На первом шаге назовите слой *Artesian*, выберите площадную модель пространственных объектов (*Polygon features*).

    * На втором шаге выберите систему координат. Оптимально использовать ту же систему, что используется в базовых данных. Для этого ее можно импортировать у существующего слоя. Нажмите **Add Coordinate Systems > Import**, найдите и укажите любой слой в базе данных *Don.gdb*.

    * На 3-м и 4-м шагах оставьте все параметры по умолчанию.

    * На 5-м шаге добавьте в первую пустую строку новое поле *Basin*. Тип поля — *Text*. В этом поле вы будете хранить название гидрогеологического бассейна.

    * Нажмите **Finish**.

3. Добавьте получившийся слой на карту и разместите его вверху таблицы содержания.

4. Отключите слои рек и озер.

    > Для выполнения дальнейшей работы прочтите раздел **[Редактирование](#manual-edit)** в приложении.

1. Включите режим редактирования слоя. Для этого в его контекстном меню выберите команду **Edit Features > Start Editing**.

7. Откройте список шаблонов слоя и посмотрите доступные опции редактирования в нижней части окна.

8. Векторизуйте все бассейны, обведя их границы. Выполняйте работу в следующей последовательности:

    * Сначала векторизуйте *Донецко-Донской бассейн (IV)* с помощью обычного инструмента **Polygon**.

    * Далее последовательно пристыкуйте к нему оставшиеся бассейны с помощью инструмента **Auto Complete Polygon**. Замкните их по границе листа.

9. После того как векторизация районов завершена, сохраните изменения, выбрав команду **Editor > Save Edits**.

10. Откройте таблицу атрибутов слоя районов. Поочередно выделяя каждый из них (для этого щелкните в самом начале строки), введите в поле *Basin* его название, ориентируясь по выделению на карте. Слово «бассейн» не вводите (Рис. \@ref(fig:mrh-att)):

    <div class="figure">
    <img src="images/Ex06/image13.png" alt="Редактирование атрибута выбранного объекта" width="100%" />
    <p class="caption">(\#fig:mrh-att)Редактирование атрибута выбранного объекта</p>
    </div>

1.  После ввода названий снова сохраните изменения.

2.  Завершите редактирование, выбрав команду **Editor > Stop Editing**.

3.  Измените оформление слоя в соответствии с цветами на исходном растре.

4.  Включите подписи районов по полю *Basin*.

5.  Отключите слой растровой карты. Включите снова слои рек и озер и переместите их вверх таблицы содержания. Картографическоем изображение примет следующий вид (Рис. \@ref(fig:mrh-vec)):

    <div class="figure">
    <img src="images/Ex06/image14.png" alt="Векторизованный слой артезианских бассейнов" width="100%" />
    <p class="caption">(\#fig:mrh-vec)Векторизованный слой артезианских бассейнов</p>
    </div>

1.  Сохраните документ карты в папке отчета.

<kbd>**Снимок экрана №3** — *Слой артезианских бассейнов*</kbd>

## Оформление карты {#map-ref-hydrogeologic-design}
[В начало упражнения ⇡](#map-ref-hydrogeologic)

1. Добавьте на карту слои *Boundaries* (границы) и *Cities* (города). Используйте для их отображения способ **Categories** и настройте отображение разными символами классов границ, а также городов в соответствии с численностью населения. При оформлении подписей городов используйте метод [**классифицированных подписей**](#manual-labels-class). Пример результирующего изображения (Рис. \@ref(fig:mrh-map)):

    <div class="figure">
    <img src="images/Ex06/image24.png" alt="Оформленное картографическое изображение" width="100%" />
    <p class="caption">(\#fig:mrh-map)Оформленное картографическое изображение</p>
    </div>

    <kbd>**Снимок экрана №4** — Карта</kbd>

1. Переключитесь в режим компоновки и оформите легенду карты. Добавьте название «*Гидрогеологическое районирование среднего течения Дона*», а также масштаб и свои ФИО.

2. Экспортируйте результирующую карту в файл в формате *PNG* и вставьте ее в отчет.

> **Следующие разделы являются дополнительными и выполняются на усмотрение преподавателя**. Перед их выполнением целесообразно ознакомиться с разделом **[Выборка объектов](#manual-select)** в приложении.

## Пространственный запрос (дополнительно) {#map-ref-hydrogeologic-query}
[В начало упражнения ⇡](#map-ref-hydrogeologic)

Для получения информации о взаимном положении объектов или поиске объектов, основанном на их местоположении, вы можете использовать три метода:

* Вычисление расстояний
* Пространственный запрос
* Оверлей

Вычисление расстояний позволяет оценить попарные расстояния между объектами, найти для каждого объекта ближайший к нему. Пространственный запрос осуществляет выборку объектов, находящихся в указанных топологических отношениях с другими объектами. Например, вы можете сказать «*выбрать реки, находящиеся целиком внутри (completely within) Московского артезианского бассейна*» или смягчить запрос, указав «*выбрать реки, пересекающие (intersect) Московский артезианский бассейн*». Частным случаем пространственного запроса также является поиск объектов по координатам, диапазону координат или произвольно заданной области. В этом случае пользователь чаще всего обводит на карте прямоугольником интересующую его зону, при этом выбираются объекты, пересекающие или находящиеся целиком внутри выделенной зоны.

Рассмотрим, как можно выберать реки, принадлежащие Приволжско-Хоперскому бассейну.

1. Выделите на карте Приволжско-Хоперский бассейн, используя инструмент ![](images/Ex06/image15.png) на панели **Tools**.

2. Откройте диалог пространственной выборки (**Selection > Select by Location**)

3. Выберите в диалоге пространственной выборки слой *Rivers* в качестве выбираемого (**target**) и слой *Artesian* в качестве выбирающего (**source**). Отметьте галочкой пункт **Use Selected Features** --- это позволит выбирать с использованием уже выбранных объектов.

4. Выберите метод выборки *intersect the source layer feature* --- пересечение.

5. Нажмите **Apply**. Будут выбраны реки, пересекающие выбранный артезианский бассейн (Рис. \@ref(fig:mrh-int)):

    <div class="figure">
    <img src="images/Ex06/image16.png" alt="Пространственная выборка по правилу пересечения (intersect)" width="100%" />
    <p class="caption">(\#fig:mrh-int)Пространственная выборка по правилу пересечения (intersect)</p>
    </div>

1. Выберите метод выборки *are completely within the source layer feature* (полностью внутри).

2. Нажмите **Apply**. Будут выбраны реки, находящиеся полностью внутри выбранного бассейна (Рис. \@ref(fig:mrh-wit)):

    <div class="figure">
    <img src="images/Ex06/image17.png" alt="Пространственная выборка по правилу нахождения внутри (completely within)" width="100%" />
    <p class="caption">(\#fig:mrh-wit)Пространственная выборка по правилу нахождения внутри (completely within)</p>
    </div>
    

1. Очистите выборку с помощью инструмента **Clear Selected Features** (Рис. \@ref(fig:mrh-clr)):

    <div class="figure">
    <img src="images/Ex06/image18.png" alt="Кнопка очищения текущей выборки" width="40%" />
    <p class="caption">(\#fig:mrh-clr)Кнопка очищения текущей выборки</p>
    </div>

## Оверлей (дополнительно) {#map-ref-hydrogeologic-overlay}
[В начало упражнения ⇡](#map-ref-hydrogeologic)

**Оверлей** (от англ. *overlay* — наложение), в отличие от пространственного запроса, создает новые данные путем геометрической композиции входных слоев. Полученные участки наследуют атрибуты от каждого слоя. Эта операция базируется на стандартных отношениях множеств, таких как пересечение, объединение и симметрическая разность. Оверлей позволяет понять, какие комбинации объектов встречаются в пространстве. Так, если в качестве аргументов служат реки и бассейны, то в результате выполнения оверлея реки будут разрезаны на участки в соответствии с границами бассейнов.

Для выполнения оверлея вы будете использовать инструменты **геообработки**.

> **Геообработка (geoprocessing)** в терминологии ArcGIS — это анализ и преобразование пространственных данных. Инструменты геообработки находятся в Арктулбоксе (ArcToolbox), где они сгруппированы по назначению. Некоторые наборы инструментов, такие как Spatial Analyst и 3D Analyst, с которыми вы познакомитесь на следующих занятиях, являются дополнительными модулями ArcGIS.

С помощью оверлея можно разбить речную сеть на сегменты, принадлежащие разным бассейнам, а полученным сегментам автоматически присвоить название бассейна.

1. Щелкните по базе данных *Don.gdb* правой кнопкой мыши и выберите пункт **Make Default Geodatabase**. Эта команда указывает системе, что все результаты обработки данных (новые слои) следует помещать в выбранную базу геоданных.

2. Откройте **ArcToolbox** с помощью кнопки ![](images/Ex06/image19.png) на главной панели инструментов.

3. Раскройте группу инструментов **Analysis Tools > Overlay**. Здесь можно найти различные режимы оверлея.

4. Запустите инструмент **Identity**, который находит геометрическое пересечение двух слоев и присваивает атрибуты второго слоя участкам первого слоя.

5. Заполните его параметры следующим образом:

    Параметр                Значение
    ----------------------- --------
    *Input features*        Rivers
    *Identity Features*     Artesian
    *Output Feature Class*  `<Ваша папка>\Ex06\Don.gdb\Rivers_Identity`
    *JoinAttributes*        ALL

    Диалог инструмента примет следующий вид (Рис. \@ref(fig:mrh-ide)):

    <div class="figure">
    <img src="images/Ex06/image20.png" alt="Параметры инструмента оверлея методом Identity" width="80%" />
    <p class="caption">(\#fig:mrh-ide)Параметры инструмента оверлея методом Identity</p>
    </div>

После выполнения инструмента слой будет добавлен на карту. Раскройте его таблицу атрибутов, чтобы убедиться, что каждому участку реки присвоена принадлежность к артезианскому бассейну — часть строк будет пустой, так как созданный вами слой артезианских бассейнов покрывает не всю территорию (Рис. \@ref(fig:mrh-ida)):

<div class="figure">
<img src="images/Ex06/image21.png" alt="Атрибутивная таблица слоя рек с идентификацией принадлежности участков к артезианским бассейнам" width="100%" />
<p class="caption">(\#fig:mrh-ida)Атрибутивная таблица слоя рек с идентификацией принадлежности участков к артезианским бассейнам</p>
</div>

## Атрибутивный запрос (дополнительно) {#map-ref-hydrogeologic-attributes}
[В начало упражнения ⇡](#map-ref-hydrogeologic)

Атрибутивный запрос позволяет искать объекты по значениям их атрибутов. В результате выполнения оверлея вы можете найти участки рек, принадлежащие артезианским бассейнам, по информации, содержащейся в поле *Basin*.

1. Откройте диалог атрибутивной выборки (меню **Selection > Select by Attributes**).

2. Выберите в качестве выбираемого слой *Rivers\_Identity*.

3. Введите следующий текст запроса:

    `"Basin" = 'Приволжско-Хоперский'`

1. На карте будут выделены водотоки, принадлежащие данному артезианскому бассейну. Обратите внимание на то, что выборка теперь полностью совпадает с границами бассейна.

2. Откройте таблицу атрибутов слоя *Rivers\_Identity* и укажите опцию **Show Selected records**, чтобы показывать только выбранные объекты (Рис. \@ref(fig:mrh-shw)):

    <div class="figure">
    <img src="images/Ex06/image22.png" alt="Кнопка переключения в режим отображения в таблице только выбранных объектов" width="30%" />
    <p class="caption">(\#fig:mrh-shw)Кнопка переключения в режим отображения в таблице только выбранных объектов</p>
    </div>

1. Скомпонуйте окна приложения таким образом, чтобы было видно одновременно окно атрибутивного запроса, таблицу атрибутов слоя со столбцом *Basin*, а также картографическое изображение с выделенными реками. Окно приложения примет следующий вид (Рис. \@ref(fig:mrh-sat)):

    <div class="figure">
    <img src="images/Ex06/image23.png" alt="Выборка участков рек по атрибуту принадлежности артезианскому бассейну" width="100%" />
    <p class="caption">(\#fig:mrh-sat)Выборка участков рек по атрибуту принадлежности артезианскому бассейну</p>
    </div>

1. Сохраните документ карты.

## Контрольные вопросы {#map-ref-hydrogeologic-questions}
[В начало упражнения ⇡](#map-ref-hydrogeologic)

1. В какой последовательности расставляются контрольные точки при привязке данных? Каково их оптимальное расположение?

2. Какой метод трансформирования изображения вы использовали в работе? В чем его суть?

3. Как пристыковать один полигон к другому, не обводя их общую границу? Опишите последовательность действий.

4. Что позволяют делать атрибутивная и пространственная выборки? (*дополнительно*)

5. В чем суть операции оверлея слоев? Чем эта операция отличается от пространственной выборки? (*дополнительно*)
