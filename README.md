# digital_humanities_RKI Проект "Это не его конёк"

1)	**Что нам нужно было сделать?**
Перед нами стояла задача определить создателя текста «Конёк-горбунок», авторство которого официально принадлежит Петру Павловичу Ершову. Несмотря на это, не утихает спор о том, действительно ли это так. Одной из самых популярных является версия с авторством Александра Сергеевича Пушкина.
2)	**Как это нужно было сделать?** 
Нам было предложено использовать программу R и ее расширение Stylo, которое используется для сравнения текстов и определения авторства. 
Мы создали корпус со стихотворными (в большей степени) и несколькими прозаическими произведениями авторов-современников (Ершов, Пушкин, Жуковский, Языков, Катенин, Крылов) по типу «author_nameofnovel.txt». Так как программа лучше работает для больших объемов текста, мы объединили некоторые произведения, принадлежащие одному автору. В этом случае в названии текстового файла были указаны все произведения, которые он содержит. 

3)	**STYLO: Default Settings** 
При дефолтных настройках Stylo:
•	Language – other
•	UTF-8
•	MWF: min-100, max-100
•	CULLING: min-0, max-0
•	Cluster Analysis 

При кластерном анализе мы получили такой граф (дерево): 
![screenshot of sample](https://vk.com/album5573227_252548110?z=photo5573227_456239241%2Falbum5573227_252548110)

Программа сразу выделила Крылова и Баратынского – прозаические произведения. В разные ребра графа попали Пушкин и Ершов. «Конек-Горбунок» больше всего оказался похож на произведения Жуковского и «Сказку о царе Салтане» Пушкина. 
Так как произведения, авторство которых установлено, попали в разные част графа, мы можем сказать, что дефолтные настройки Stylo не дают нам точного результата.

4)	**Stylo: MFW Settings** 
Мы обратились к настройкам Stylo, чтобы получить более точные данные. 
MWF – most frequent words – эта программа определяет схожесть текстов по самым частотным словам. 
Min – здесь мы указываем, какое количество слов из топ-листа самых частотных (для всего корпуса) будет использоваться для первого прогона программы.
Max – здесь мы указываем, какое количество слов из топ-листа самых частотных (для всего корпуса) будет использоваться для последнего прогона команды. 
В дефолтных настройках было указано MWF: min-100, max-100. То есть программа пыталась определить авторство по сотне самых частотных слов корпуса. Естественно, никаких значимых результатов мы не получили.
При таком варианте настроек с другим MWF, когда авторство определяется по 1000 самых частотных слов:

•	Language – other
•	UTF-8
•	MWF: min-1000, max-1000
•	CULLING: min-0, max-0
•	Cluster Analysis 

Мы получили такой вариант: 
картинка 2 ![GitHub Logo]()
При таких настройках программа собрала вместе всю прозу, всего Пушкина, а также практически всего Ершова. При этом прозу Ершова («Осенние вечера») программа также выделила отдельно, а «Конёк-Горбунок», как и раньше, оказался на одной ветке с Жуковским. 
Так как при изменённых настройках параметра MFW программа выдала нам результат, более приближенный к действительности, мы решили пойти еще дальше:
•	Language – other
•	UTF-8
•	MWF: min-1000, max-5000
•	CULLING: min-0, max-0
•	Cluster Analysis 

При таких настройках программа искала авторство уже по 5000 самых частотных слов корпуса. 
Мы получили такой результат:
картинка 3

Программа сработала довольно четко. Сначала она отсеяла прозу Ершова, затем его лирику. Затем собрала вместе Пушкина и оставшихся авторов. ! **Примечательно, что «Конёк-Горбунок» попал не к Ершову и не к Пушкину, хотя их кластеры четко выражены, а вновь ближе к Жуковскому и Языкову**.
Этот вариант настроек мы впоследствии признали самым удачным, так как он дает четкое распределение текстов с известным авторством.


5)	**Stylo: CULLING Settings**
Culling анализирует авторство также по вордлисту, но при этом с возможностью отбросить некоторые слова. Например, если мы поставим параметр 20, то в вордлисте окажутся те слова, которые есть хотя бы в 20% текстов их корпуса. Если мы ставим 0, то он не отбросит никакие слова, а если 100, то будут использоваться только те слова, которые появились во всех текстах хотя бы однажды.
Результат с CULLING: min-20, max-20:
картинка 4

Результат с CULLING: min-100, max-100:
картинка 5

Оба раза программа относит произведение «Конёк-Горбунок» к Жуковскому (как и ранее) и к «Осенним вечерам» Ершова. То есть если судить по списку слов, которые использовались во ВСЕХ произведениях корпуса, то «Осенние вечера» и «Конёк-горбунок» очень схожи. Но все-таки это список самых популярных слов выбранных авторов (то есть там могут быть и служебные слова, и местоимения и тд).  Очень логично, что программа отделила практически все сказки (Пушкин, Ершов, Жуковский) с похожими элементами.
В целом, вариант с самыми популярными словами для ОДНОГО автора (MFW) кажется более удачным, чем определение авторства по общим словам всего корпуса (CULLING).

6)	**STYLO: Consensus Tree**
Мы представили самый удачный вариант настроек (MFW – max: 5000)  также с помощью консенсусного дерева. Этот график выводит статистический «компромисс» для MFW и CULLING:
картинка 6
Он также отмечает, что «Конёк-Горбунок» ближе к Пушкину и Жуковскому, чем к Ершову.

7)	**STYLO: Multidimentional scaling**
Также работает для MFW и CULLING, но при небольшом количестве циклов (чаще одного) и при равных значениях двух вышеупомянутых параметров. У нас они различны. 
(This option makes sense if there is only a single "MDS" iteration (or just a few). This is achieved by setting the MFW Minimum and Maximum to equal values, and doing the same for Culling Minimum and Maximum.)
картинка 8

8)	**ВЫВОДЫ**: 
Самым показательным параметром в нашем случае оказался MWF – most frequent words – который определяет схожесть текстов по самым частотным словам (пункт 4 и 6). В случае с «Коньком-Горбунком» можно сказать, что словарная база этого произведения ОЧЕНЬ ОТЛИЧАЕТСЯ от остальных произведений Ершова. А схожа скорее с Жуковским. С чем это может быть связано?
1. Это первое (и чудесным образом лучшее) произведение Ершова, написанное в 18-летнем возрасте. В нем он мог сильно копировать язык Жуковского.
2. Это писал не Ершов, а Жуковский 
3. Это писал не Ершов, а Пушкин (кстати, вероятность меньше, у Жуковского)

З.Ы. Просим заметить, что выборка корпуса субъективна, в ней содержится недостаточное количество произведений Жуковского. Для более точных данных можно было бы сделать полные корпуса только для Пушкина, Жуковского  и Ершова, отдельно для прозы, лирики и сказок.

9) **Анализ статей на тему авторства «Конька-Горбунка»** 
Если ввести в Google Academia «Авторство Конька-Горбунка», то результатов без чистки и уточнений получается около сотни. ЕLibrary тоже говорит нам о том, что написано несколько сотен статей на эту тему, и это значит, что исследователей со всего мира до сих пор волнует вопрос, кто же был автором известной сказки. 

Мы просмотрели несколько работ, которые показались наиболее подходящими, а именно:

1. Савченкова Т.П. Творческие взаимоотношения Пушки и Ершова как литературоведческая проблема.

2. Савченкова Т.П. М.К. Азадовский об авторстве зачина сказки П.П. Ершова «Конек-Горбунок».

3. Перцов Н.В. Сказка «Конек-Горбунок» П.П. Ершова в историко-литературном, лингвистическом и текстологическом освещении.

4. Касаткин Л.Л., Касаткина Р.Ф. Язык – свидетель беспристрастный: проблема авторства сказки «Конек-Горбунок»

Все указанные статьи находятся в свободном доступе и могут быть найдены на eLibrary. 
Основное, что хочется отметить, — это то, что ни в одной из статей нет утверждения о том, что произведение было полностью написано Ершовым. Разница лишь в том, что кто-то не приписывает Ершову лишь первую часть «Конька-Горбунка», а кто-то всю сказку целиком. Также авторы шли, в основном, по двум путям: кто-то анализировал конкретно первое издание, а кто-то сравнивал между собой сразу несколько и следил за изменениями. В качестве основных изменений выделяют: «обнародование» стиля, появление диалектизмов (также отмечалось присутствие псковского говора), введение новых эпизодов, отказ от «первоначальной социальной остроты» и «усиление православно-религиозных черт». 

Основным «подозреваемым» в авторстве является, конечно, Пушкин, но также мы можем встретить упоминания, к примеру, и о Жуковском. Все исследователи соглашаются, что Пушкин, несомненно, оказал влияние на Ершова, но вопрос о степени этого влияния все еще остается открытым. В некоторых статьях мы можем увидеть разбор текста «Конька-Горбунка», когда исследователи анализируют рифмы и выделяют некоторые, «выбивающиеся» из общего строя, а также обращают внимание на появление «Пушкинской строки», которой не было в первоначальном варианте сказки. Также проводился анализ таких моментов, как использование деепричастий, локативных наречий и ударений, в результате чего было показано, что авторство «Конька-Горбунка» действительно под вопросом. 


*Список источников*
1. [П. Ершов Конек Горбунок](https://solnet.ee/skazki/777)
2. [П. Ершов Cибирский казак](http://libverse.ru/yershov/sibirskii-kazak.html)
3. [П. Ершов Сузге](http://libverse.ru/yershov/syzge.html)
4. [П. Ершов Осенние вечера](http://az.lib.ru/e/ershow_p_p/text_1856_osennie_vechera.shtml)
5. [П. Ершов Избранные стихотворения](http://az.lib.ru/e/ershow_p_p/text_0050.shtml)
6. [В. Жуковский Война мышей и лягушек](http://lib.ru/LITRA/ZHUKOWSKIJ/mouse.txt)
7. [В. Жуковский Сказка о царе Берендее, о сыне его Иване-царевиче, о хитростях Кощея Бессмертного и о премудрости Марьи-царевны, Кощеевой дочери](http://rvb.ru/19vek/zhukovsky/01text/vol3/02tales/300.htm)
8. [В. Жуковский Кот в сапогах](http://rvb.ru/19vek/zhukovsky/01text/vol3/02tales/303.htm)
9. [В. Жуковский Сказка о Иване-Царевиче и сером волке](http://az.lib.ru/e/ershow_p_p/text_0050.shtml)
10. [В. Жуковский Спящая царевна](http://rvb.ru/19vek/zhukovsky/01text/vol3/02tales/301.htm)
11. [Н. Языков Жар-птица](http://az.lib.ru/j/jazykow_n_m/text_0110.shtml)
12. [П. Катенин Княжна Милуша](http://public-library.ru/Katenin.Pavel/knyazhna_milusha.html)
13. [А. Пушкин Золотой петушок](http://rvb.ru/pushkin/01text/03fables/01fables/0801.htm)
14. [А. Пушкин Сказка о медведихе](http://rvb.ru/pushkin/01text/03fables/01fables/0797.htm)
15. [А. Пушкин Cказка о попе и работнике его балде](http://rvb.ru/pushkin/01text/03fables/01fables/0796.htm) 
16. [А. Пушкин Сказка о царе Салтане, о сыне его славном и могучем богатыре князе Гвидоне Салтановиче и о прекрасной царевне Лебеди](http://rvb.ru/pushkin/01text/03fables/01fables/0798.htm)
17. [А. Пушкин Сказка о рыбаке и рыбке](http://rvb.ru/pushkin/01text/03fables/01fables/0799.htm) 
18. [А. Пушкин Сказка о мертвой царевне и о семи богатырях](http://rvb.ru/pushkin/01text/03fables/01fables/0800.htm)
19. [А. Пушкин Сказка о золотом петушке](http://rvb.ru/pushkin/01text/03fables/01fables/0801.htm)
20. [А. Пушкин Руслан и Людмила](http://rvb.ru/pushkin/01text/02poems/01poems/0784.htm)
21. [А. Пушкин Жених](http://vseskazki.su/avtorskie-skazki/skazki-pushkina-online/zhenih-ballada.html)
22. [И. Крылов Щука ](http://rvb.ru/18vek/krylov/01text/vol3/01fables/205.htm)
23. [И. Крылов Булат](http://rvb.ru/18vek/krylov/01text/vol3/01fables/210.htm)
24. [И. Крылов Лев ](http://rvb.ru/18vek/krylov/01text/vol3/01fables/222.htm)
25. [И. Крылов Лещи](http://krylov.lit-info.ru/krylov/basni/leschi.htm)
26. [И. Крылов Оракул](http://rvb.ru/18vek/krylov/01text/vol3/01fables/045.htm)
27. [Е. Баратынский Сцена из Поемы](http://az.lib.ru/b/baratynskij_e_a/text_0160.shtml)



**Рабочая группа проекта**: 
1. Людмила Лучкова – сбор текстов Крылова и Баратынского, загрузка материалов на github 
2. Ольга Осиновская – сбор текстов Пушкина, ссылки (github) 
3. Юлия Яковлева – сбор текстов Катенина, Языкова, Ершова 
4. Мария Глухова – сбор текстов Жуковского, статьи об авторстве 
5. Анастасия Часовских – сбор корпуса, анализ на R

![GitHub Logo](https://www.hse.ru/images/main_en/hse_ru_logo.svg)
