# Структура программы:  
## Структура Training  
В структурах, которые будут описывать разные виды тренировок, применяются одни и те же свойства и методы. Чтобы избежать дублирования кода, логично создать общую структуру Training{}. Поля и методы этой структуры будут справедливы для любого из видов тренировки.
Каждая структура, которая описывает определённый вид тренировки, будет дополнять и расширять общую структуру Training{}.
Все необходимые для расчетов константы будут уже объявлены:  
_MInKm — количество метров в одном километре,  
MinsInHour — количество минут в одном часе,  
LenStep — длина одного шага,  
CmInM — количество сантиметров в одном метре,  
CaloriesMeanSpeedMultiplier — множитель средней скорости бега,  
CaloriesMeanSpeedShift — коэффициент изменения средней скорости бега,  
CaloriesWeightMultiplier — коэффициент для расчета потраченных калорий при ходьбе,  
CaloriesSpeedHeightMultiplier — коэффициент для расчета потраченных калорий при ходьбе,  
KmHInMsec — коэффициент для перевода км/ч в м/с,  
SwimmingLenStep — длина одного гребка при плавании,  
SwimmingCaloriesMeanSpeedShift — коэффициент изменения средней скорости при плавании,  
SwimmingCaloriesWeightMultiplier — множитель веса пользователя при плавании._  
#### Поля структуры Training
_TrainingType, тип string — описывает вид тренировки (Бег, Ходьба, Плавание);  
Action, тип int — описывает количество шагов при беге и ходьбе или количество гребков при плавании;  
LenStep, тип float64 — описывает длину одного шага или гребка;  
Duration, тип time.Duration — описывает продолжительность тренировки;  
Weight, тип float64 — описывает вес пользователя в килограммах._  
#### Методы структуры Training{}  
_Метод distance() возвращает дистанцию в километрах, которую преодолел пользователь за время тренировки.    
Метод meanSpeed() возвращает значение средней скорости движения во время тренировки.   
Метод Calories() возвращает количество килокалорий, израсходованных во время тренировки.
Логика подсчета калорий для каждого вида тренировки будет своя, поэтому в методе общей структуры Training{} не нужны никакие вычисления. Этот метод будет возвращать просто 0.  
Метод TrainingInfo() возвращает переменную структуры InfoMessage{}. Чтобы заполнить все поля структуры InfoMessage{}, нужно будет вычислить дистанцию, среднюю скорость и количество калорий._ 
## Структура InfoMessage  
Эта структура нужна для создания сообщений о проведенной тренировке. У структуры должен быть метод для вывода сообщений на экран.
Поля структуры InfoMessage{}  
_TrainingType, тип string — описывает вид тренировки (бег, ходьба, плавание);  
Duration, тип time.Duration — описывает продолжительность тренировки;  
Distance, тип float64 — расстояние, которое преодолел пользователь за время тренировки, в километрах;  
Speed, тип float64 — средняя скорость, с которой двигался пользователь, в км/ч;  
Calories, тип float64 — количество потраченных калорий на тренировке._  
Для этой структуры будет переопределен метод String(), который выводит всю информацию о тренировке на экран.  
## Интерфейс CaloriesCalculator  
Этот интерфейс нужен, чтобы было удобно работать со всеми видами тренировок. Интерфейс будет состоять из двух методов:  
_Calories() — метод возвращает количество килокалорий, израсходованных во время тренировки.  
TrainingInfo() — возвращает переменную структуры InfoMessage{}._  
## Структура Running  
Структура описывает тренировку Бег. Так как вы создали общую структуру Training{}, которая справедлива для всех видов тренировок, значит, её нужно встроить в структуру Running{}. Больше никакие поля в структуре Running{} не нужны.
Структура Running{} должна реализовывать интерфейс CaloriesCalculator.    
_Метод Calories() возвращает количество израсходованных калорий при беге.  
Метод TrainingInfo() возвращает переменную структуры InfoMessage{} с информацией о проведённой
тренировке._  
Для реализации этого метода для структуры Running{} нужно вернуть вызов такого же метода TrainingInfo(), только для структуры Training{}.   
## Структура Walking  
Структура описывает тренировку Ходьба. Так как вы создали общую структуру Training{}, которая справедлива для всех видов тренировок, значит, её нужно встроить в структуру Walking{}. Для расчета израсходованных калорий при ходьбе нужно еще одно значение — это рост пользователя в метрах. Добавьте поле Height типа float64 в структуру Walking{}.
Структура Walking{} тоже должна реализовывать интерфейс CaloriesCalculator.      
_Метод Calories() возвращает количество израсходованных калорий при ходьбе.  
Метод TrainingInfo() возвращает переменную структуры InfoMessage{} с информацией о проведённой тренировке._  
Для реализации этого метода для структуры Walking{} нужно вернуть вызов такого же метода TrainingInfo(), только для структуры Training{}. 
## Структура Swimming  
Структура описывает тренировку Плавание. Так как вы создали общую структуру Training{}, которая справедлива для всех видов тренировок, значит, её нужно встроить в структуру Swimming{}. Для расчета израсходованных калорий при плавании, нужно добавить ещё несколько полей в структуру:  
_LengthPool, тип int — длина бассейна в метрах,  
CountPool, тип int — количество пересечений бассейна._  
Так как средняя скорость при плавании рассчитывается не так, как для бега и ходьбы, вам нужно будет переопределить метод meanSpeed() из Training{}, только теперь для структуры Swimming{}. 
Структура Swimming{} также должна реализовывать интерфейс CaloriesCalculator.  
_Метод Calories() возвращает количество израсходованных калорий при плавании.  
Метод TrainingInfo() возвращает переменную структуры InfoMessage{} с информацией о проведённой тренировке._  
Для реализации этого метода для структуры Swimming{} нужно вернуть вызов такого же метода TrainingInfo(), только для структуры Training{}.  
## Функция получения данных ReadData()  
Чтобы получить данные о тренировке, нужно будет реализовать функцию ReadData(). Это общая функция с одним параметром типа CaloriesCalculator, то есть вашего интерфейса для всех видов тренировок. А значит, вы можете подавать в функцию в качестве аргумента любые структуры, которые реализовывают этот интерфейс.
В самой функции нужно вызывать методы, которые имеют одинаковые имена для всех структур (видов тренировок), но для переменной интерфейса (параметра функции). Так будет вызываться метод, относящийся к структуре, которую вы передали в функцию ReadData() в качестве аргумента.