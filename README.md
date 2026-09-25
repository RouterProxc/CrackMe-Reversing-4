# CrackMe-Reversing-4

Решаю задачу с неизвестным (по крайней мере для меня) пакером 


<img width="844" height="492" alt="image" src="https://github.com/user-attachments/assets/3d8cc840-a84c-4642-bf3d-4d6bee93a400" />


В ida не отображается ни одной нормальной функции кроме 1 tls callback`а и getproccessheap

Main не подает признаков вменяемости из за чего я принял решение об упаковке реального кода

При отладке в idaPro (для наглядности имен функций) заметил закономерность что после данного перехода (указанного ниже) попадаем в sub_7FF6ADC21F50


<img width="768" height="408" alt="image" src="https://github.com/user-attachments/assets/cb54591e-792a-44fb-9266-c84fd8c7bb75" />


<img width="1272" height="760" alt="image" src="https://github.com/user-attachments/assets/3dc27438-81a7-40a2-998d-ed1beb03f216" />


Обратите внимание на mov rdi, rdx - потом на значение rdx, а уже после на hex view 1 (снизу) - программа помещает в rdx адрес какой то подпрограммы чей код находится внутри родительской (нашего крякми)

Почему же я решил посмотреть на значение rdx? Я увидел что после выполнения функции вызов которой находится в sub_7FF6ADC21F50 по адресу 140001490 появляется код который и введет в реальную логику программы 


<img width="644" height="532" alt="image" src="https://github.com/user-attachments/assets/c6e3ca36-67ec-44a3-98e5-22140ff0b4c1" />


Обратите внимание на функцию выше *create func* -  там и происходит распаковка (название Create func - ошибочно)

<img width="820" height="604" alt="image" src="https://github.com/user-attachments/assets/038af1ee-7b87-4bb5-a969-ab663992618b" />

rbp+78 = 0x60 - ключ для расшифровки данных

Помимо подобных манипуляций с поиском main`а можно попробовать в некоторых случаях поставить бряк на VirtualProtect (программа задает привилегии на запись подпрограммы в память) и оттуда уже двигаться обратно по стеку вызовов (выполнить до возврата - в x64dbg *не забудьте снять бряк чтобы не зациклиться в функции VirtualProtect)

После цикла расшифровки нахожу в памяти расшифрованный флаг

<img width="840" height="132" alt="image" src="https://github.com/user-attachments/assets/1f2f24ae-0ba3-41f0-9601-d3baf6d55bf3" />

IEEE{S031me_T1mes_We_h1s_t0_Su111ffer}
