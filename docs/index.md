--- 
title: "Основы геоинформатики: практикум в ArcGIS"
author: "Тимофей Самсонов"
date: "2025-03-17"
description: "Основы геоинформатики: практикум в ArcGIS"
site: bookdown::bookdown_site
knit: "bookdown::render_book"
documentclass: book
bibliography: [book.bib]
biblio-style: [gost-authoryear]
biblatexoptions: [language=auto,autolang=other,movenames=false]
#csl: [gost-r-7-0-5-2008-author-date-alphab.csl]
link-citations: yes
colorlinks: true
github-repo: tsamsonov/arcgis-course
fontsize: 12pt
mainfont: PT Serif
toc_float:
  collapse: section
  smooth_scroll: true
---

# Введение{-}



> Если вы ищете практикум на основе __QGIS__, то он находится [__тут__](https://aentin.github.io/qgis-course/).

Настоящий практикум разработан для обеспечения курса «Основы геоинформатики», который читается на Географическом факультете МГУ имени М.В.Ломоносова в рамках образовательных программ бакалавриата по направлениям подготовки 05.03.02 «География», 05.03.03 «Картография и геоинформатика», 05.03.04 «Гидрометеорология» и 05.03.06 «Экология и природопользования». 

Практикум состоит из 18 заданий и представляет собой комплекс учебных упражнений для освоения базовых технологий геоинформатики и методов пространственного анализа. Состав выполняемых упражнений определяется преподавателем. Упражнения выполняются в ГИС-пакете ArcGIS for Desktop 10.3+.

Практикум доступен в трех форматах: [__HTML__](https://tsamsonov.github.io/arcgis-course/), [__EPUB__](https://tsamsonov.github.io/arcgis-course/arcgis-course.epub) и [__PDF__](https://tsamsonov.github.io/arcgis-course/arcgis-course.pdf). Скачать практикум в форматах PDF и EPUB можно через главное меню (Рис. \@ref(fig:index-down)):

<div class="figure">
<img src="images/download.png" alt="Загрузка файловых версий практикума" width="1582" />
<p class="caption">(\#fig:index-down)Загрузка файловых версий практикума</p>
</div>

__Для практической работы рекомендуется использовать HTML-версию, поскольку она поддерживается в актуальном состоянии__. Формат _EPUB_ удобен своей компактностью и адаптивностью под экран устройства, и может быть использован для работы с электронной книгой или планшетом в оффлайн-режиме. Во всех версиях практикума реализована навигация по разделам.

Актуальная версия практикума, которую вы сейчас просматриваете, сформирована 2025-03-17 и доступна по ссылке <https://tsamsonov.github.io/arcgis-course/>.

__Перед выполнением практикума необходимо внимательно ознакомиться с регламентом и тщательно придерживаться его при выполнении заданий__.

## Регламент {-}

В целях обеспечения порядка на рабочих компьютерах и успешного завершения курса следует придерживаться следующих правил.

- __Ваша личная рабочая директория должна иметь адрес__:

    `D:\GIS\<ваша кафедра>\<фамилия>`

    Например, студент 207 группы Петров хранит результаты своей работы в каталоге `D:\GIS\207_CAR\Петров`.

    Каталоги кафедр уже созданы. Если вас двое, пишите в названии каталога обе фамилии: «Петров-Иванов». По желанию вы можете называть свой каталог латиницей (Petroff).

- __Исходные данные, а также шаблоны отчетов для выполнения всех заданий лежат в каталоге:__

    `\\gserver\DATA\GIS\`

    Внутри папки есть 18 каталогов с упражнениями: Ex01, Ex02, ... Ex18. Состав и порядок выполняемых упражнений определяется вашим преподавателем (их должно быть не менее пяти). 
    
    > __Вы также можете скачать архив с исходными данными и файлом отчета, используя ссылку в начале каждого упражнения.__ Внутри архива находится точно такой же каталог с упражнением, что и в локальной сети. Его необходимо разархивировать и положить в свою папку на диске D (см. следующий шаг).
    
- __Каждое задание вы начинаете с того, что копируете соответствующую папку в свою директорию на локальном диске D.__

    > __Не меняйте ничего в каталоге исходных данных на сервере.__ Помните, что кроме вас над теми же заданиями будут работать студенты других групп. Если вы отредактируете или удалите содержимое исходного каталога упражнения, у ваших коллег возникнут проблемы при выполнении задания. 

- __Отчетный файл вы кладете в сетевую папку__:

    `\\gserver\REPORTS\GIS\<кафедра>\`

    Найдите внутри нее каталог с нужным номером упражнения, и положите в него отчетный файл. Формат имени файла должен быть следующим: 

    `Ex<номер задания>_<номер группы>_<фамилия>.doc`

    Например, студент 207 группы Петров в конце 2-го и 7-го задания должен на основе шаблона отчета создать файлы с именами `Ex02_207_Петров.doc` и `Ex07_207_Петров.doc`.

- __Документ карты с расширением _mxd_, который вы создаете в каждом задании, должен называться аналогично файлу отчета__: 

    `Ex02_207_Петров.mxd`

- __Если вы не знаете, как выполнить то или иное действие, вы можете получить соответствующую справку в разделе [Описание функций](#manual-catalog) настоящего практикума__.

## Принятые обозначения {-}

Текст пособия форматирован в соответствии со следующими соглашениями:

- __Жирным шрифтом__ в тексте отмечены элементы пользовательского интерфейса и программные инструменты.
- _Курсивом_ в тексте отмечены элементы данных и значения параметров.
- [Ссылками]() выделены операции, справку к которым вы можете найти в приложеении [Описание функций](#manual-catalog), [документации ArcGIS](https://desktop.arcgis.com/ru/documentation/) и прочих ресурсах.

<kbd>__Снимок экрана__</kbd> В этом месте вы нажимаете <kbd>Alt</kbd>+<kbd>PrintScreen</kbd> и вставляете изображение из буфера обмена в отчетный файл

> __Серый текст с чертой слева__ — это различные определения, пояснения к предыдущему тексту, а также вопросы на понимание.

## О практикуме {-}

Практикум разработан в 2011-2014 гг. на кафедре картографии и геоинформатики географического факультета МГУ [__Т.Е.Самсоновым__](https://istina.msu.ru/profile/tsamsonov/) и с тех пор ежегодно обновляется. В 2016 году совместно с [И.К.Лурье](https://istina.msu.ru/profile/IK_Lurie/) опубликовано [учебное пособие "Основы геоинформатики"](https://istina.msu.ru/download/45821659/1ej66u:uSUtcUS-XmdMMyRRpC-yflDmCv8/) , содержащее вместе с упражнениями также вводную теоретическую часть по каждой теме. В 2018 г. практикум был преобразован в интерактивный формат и выложен в открытый доступ в сеть Интернет.

Структура настоящего практикума несколько отличается от пособия 2016 г., однако состав упражнений остался прежним. В частности, вводный раздел получил название [I. Знакомство с ГИС](#map-design-quaternary), в нем обучающиеся делают первые шаги в работе с пространственными данными на примере картографической визуализации. Упражнения по _адресным (18)_ и _табличным (8)_ данным перенесены в настоящей редакции в раздел [II. Базовые технологии](#map-ref-general), поскольку в них рассматривается два варианта привязки данных: по адресу и идентификатору. По этой же причине _анализ плотности распределения (14)_ перенесен в более подходящий раздел [V. Пространственное моделирование](#density-analysis). 

Автор выражает благодарность рецензентам к.г.н. [М.Ю.Грищенко](https://istina.msu.ru/profile/sila_trakt/) и к.г.н. [П.Е.Каргашину](https://istina.msu.ru/profile/pavelkargashin/) за полезные замечания.

Библиографическая ссылка:

----
_Самсонов Т.Е._ **Основы геоинформатики: практикум**. М.: Географический факультет МГУ, 2025. DOI: 10.5281/zenodo.1167857
----
