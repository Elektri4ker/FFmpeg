Модификация мультиплексора FFMPEG-dash, позволяющая возобновить запись потока с камеры (или любого другого live-stream'а) и упаковку его в контейнер dash.

Модификация исправляет timestamp'ы потока таким образом, как будто он не прерывался

Для функционирования требуется файл start_time, в котором хранится время, в которое поток стартовал.
Во время функционирования данная модификация создает в каталоге с чанками файл timefile, в который пишет информацию, необходимую для возобновления мультиплексирования.
При отсутствии двух вышеуказанных файлов модификация работает как обычный ffmpeg(т.е., не правит никакие timestamp'ы.

Пример использования:

1. Создать файл start time:
   date +%s >start_time

2. Убедиться, что в каталоге, куда будет писаться поток, отсутствует файл timefile:
   rm timefile

3. Стартовать запись
   ffmpeg -i http://127.0.0.1:8080 -f dash \
  -an -flags +global_header out.mpd

4. Если процесс ffmpeg'а был внезапно прерван, повторить команду из п. 3. Модицикация кодека будет ориентироваться на файлы start_time и timefile чтобы правильно ставить timestamp'ы.