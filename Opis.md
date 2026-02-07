# Detekcja i śledzenie obiektów na podstawie obrazu z kamery RGB dla potrzeb bezzałogowych statków powietrznych

Niniejszy plik stanowi opis uruchomienia oraz opis wyników prac przeprowadzonych w ramach tworzenia pracy magisterskiej o powyższym tytule.

Częścią prac było porównanie algorytmów detekcji i śledzenia na powszechnie dostępnym zbiorze danych. Kod służący do wykonania tego zadania został zamieszczony w repozytorium [camera-tracking](https://github.com/Max326/camera-tracking). Repozytorium zawiera skrypty do uruchamiania porównań, narzędzia pomocnicze do analizy i wizualizacji wyników oraz wyniki przeprowadzonych testów.

## Uruchomienie projektu

Opis ogólnej konfiguracji i uruchomienia projektu znajduje się w pliku `README.md` w głównym folderze repozytorium. Aby uruchomić projekt, należy przejść opisane w niej kroki. Została ona napisana przez innych członków Koła Naukowego Robotyków.

Aby uruchomić część stanowiącą wkład niniejszej pracy, należy wykonać następujące kroki:

### 1. Uruchomienie symulacji

Najpierw, po uruchomieniu `docker`-a, należy wejść w folder `ros_ws`.
Uruchomienie symulacji należy wywołać, wybierając odpowiedni algorytm śledzenia z biblioteki OpenCV: `ros2 launch drone_bringup drone_simulation.launch.py detector:=hybrid tracker_type:=CSRT`

Projekt skonfigurowany jest w sposób umożliwiający wybór spośród 7 algorytmów śledzenia dostępnych w bibliotece OpenCV:

- 'BOOSTING'
- 'MIL'
- 'KCF'
- 'TLD'
- 'MEDIANFLOW'
- 'MOSSE'
- 'CSRT'

Następnie, w symulatorze `Webots` (uruchomionym powyższą komendą) należy otworzyć świat `aruco_gimbal.wbt`. 

### 2. Uruchomienie symulatora lotu

Uruchomienie symulatora należy wykonać poprzez otworzenie nowego terminala (poza `docker`em) i wywołanie komendy `./run_ardupilot_sitl.sh`. Uruchomi ona skrypt pozwalający na symulację lotu drona w systemie SITL ArduPilota.

### 3. Uruchomienie misji

Po otrzymaniu od symulatora informacji o gotowości do uzbrojenia drona (`FC is ready` oraz `Copter connected, ready to arm`) można uruchomić misję.

Uruchomienie misji (lot drona i podążanie za samochodem) odbywa się poprzez wywołanie (w kolejnym terminalu poza `docker`em) komendy `./run_yolo_mission.sh`. 

## Wyniki

Wyniki testów w symulacji zostały zebrane podczas uruchamiania symulacji. Poniżej przedstawiona została instrukcja uruchamiania skryptu zbierającego wyniki.

W kolejnym terminalu, w `docker`-ze, należy (z folderu `/ros_ws`) uruchomić komendę `ros2 run drone_autonomy benchmark_tracker --ros-args -p samples:=6000 -p name:=your_name`. Komenda umożliwia zdefiniowanie liczby próbek, przez które przeprowadzany będzie test oraz nazwę pliku (`your_name`), do którego zapisane będą wyniki.

Pliki utworzone przez powyższy skrypt znajdują się w folderze `docker/plots`. Po zakończeniu zbierania wyników skrypt ten pokazuje podsumowanie z danego przebiegu. Podsumowania te zostały skopiowane i zapisane w plikach `testy-...` w folderze `docker/plots/summary-results`.

## Nagranie demonstracji działania

Poniżej przedstawione zostało nagranie demonstrujące działanie algorytmu podążania za samochodem.

[demo-git-win.mp4](docker/plots/demo-git-win.mp4)
