# ML-DL
## Yoga_poses_detection
### в своем приложении я использую стандартную модель ssd300 от mmdetection + efficientnet_b0 для дополнительной классификации изображений (тренированную мной)

Для классификации изображений в файле mmdet.core.visualization.image я добавила код, 
который при условии что обнаруженный объект имеет класс "0" - человек
происходит кроп по баундинг боксу детектированного человека 
и вырезанная картинка отправляется на классификацию модели yoga_poses_classificator_model.pkl
модель натрнированная на датасете по классификации картинок для йоги (из Кагл) - основа модели Eficcitntnrt_b0


Функция визуализации show_result_pyplot из mmdetection печатает результат, но ничего не возвращает
по этому я ввела дополнительную функцию  my_output_for_gr() которая возвращает значение numpy.array
для визуализации использовала средства gradio на huggingface
https://huggingface.co/spaces/danilovabg/detection_project


Так как файл image.py подвергся модификации - добавляю его в папку с другими файлами приложния


в файле image.py произведены изменения
1. добавлена фунция predict для предсказания позы человека (строки 225-235)

2. подгружается тренированная модель efficientnet_b0 (стр 237)

3. добавлена функция classify_yoga_pose для классификации йога позы (строки 239-263)

4. лейблы для классов йога поз  107 классов (стр 265 - 300)

5. в функцию imshow_det_bboxes добавлен код для кропа и дополнительной классификации детектированного объекта (стр 273 - 384)

6. изменена функция draw_labels, в ней изменен текст лейблов (стр 146-161)

7. добавлена глобальная переменная которой в функции imshow_det_bboxes присваивается значение глобальной переменной - финальный вид каринки со всеми боксами и лейблами

8. добавлена функция my_output_for_gr возвращающая картинку необходимую для визуализации в приложении

### Я решила выбрать именно детекцию йога поз, так как по моему мнению это может иметь реальное применение в жизни. Люди интересующиеся йогой (особенно начинающие) не всегда помнят названия поз или вообще их не знают, приложение поможет оределить название позы, чтобы после можно было найти информацию об это позе (техника выполнения, показания и противопоказания) Гугл поиск по картинке не выдает релвантных результатов
![изображение](https://user-images.githubusercontent.com/112716383/218672326-755f3f9d-5983-4c62-8cc3-0961e9dc2e40.png)


## ssd_model_with_changed_backbone
Первой моей идеей было сделать модель ssd со вмененным бекбоном, то есть заменить vgg16 на efficientnet_b3
для того чтобы efficientnet_b3 подходил по размерностям нека ssd я добавила два слоя которые при размерности фичермапа 38 x 38 добавляют глубины и выводят в нек не 1 а 2 фичермапа рамерностями [512, 38, 38] и [1024, 38, 38] (модифицированный файл efficientnet.py находится в папке)

к сожалнеюи с тренировкой возникли проблемы, так как на моем ноутбуке тренировка шла очень очень медленно и часто вылетало ядро
по этому пришлось уменьшить датасет (поставив фильтр который фильтрует не только по классам - берет только класс человек но так же и по id картинки, то есть только первые 10 000 картинок)
так как мне удалось натренировать только 10 эпох с маленьким датасетом, к сожалению лосс не уменьшился достаточно и предстаказания которые выдают модель получаются не очень качетсвенными
с колабом я проучалась дней 10, то деньги заканчиваются на gpu, то среда упадет, то проблемы с подключением к гуглдиску (из-за большого датасета)
возможности тренировать модель на машине с гпу у меня нет :(
по этому временно пришлось отказаться от тренировки этой модели
и перейти к плану "б"  - Yoga_poses_detection
