# Лабораторная работа №3: Виртуализация

## Оглавление
- [Цель работы](#цель-работы)
- [Выполненные шаги](#выполненные-шаги)
- [Заключение](#заключение)

## Цель работы
Настроить виртуальные машины A, B и C с использованием VirtualBox, обеспечить их сетевое взаимодействие с соблюдением следующих условий:
- Машина A может взаимодействовать с машинами B и C.
- Машина B не может взаимодействовать с машиной C.

## Выполненные шаги

### 1. Установка Oracle VirtualBox
  ![Instalando Oracle](https://github.com/user-attachments/assets/ec750c2d-fd0f-40a5-8fd4-c5ffbd443491)
  - При установке возникла ошибка "Missing Dependencies: Python Core".
  ![Missing python](https://github.com/user-attachments/assets/481ceab6-7ce3-4cc4-bc70-57b8ca483576)
  - Зависимости были установлены с использованием команды:
    ```bash
    pip install pywin32
    ```
  ![pip install pywin32](https://github.com/user-attachments/assets/9132fca1-9eee-49d7-9538-6a0203d565a0)
  - Установка VirtualBox успешно завершена после установки необходимых зависимостей.
  ![Installing oracle virtual box](https://github.com/user-attachments/assets/7d35845f-0380-417f-b817-81e49b2844fa)  

### 2. Создание ВМ-A
  - Выбрана операционная система: Linux -> Ubuntu (64-bit).
  - Настроены параметры ВМ:
      - 4 ГБ оперативной памяти.
      - 2 процессора (CPU).
      - 20 ГБ виртуального жесткого диска.
  ![Hardware vm](https://github.com/user-attachments/assets/14e00ca4-fbaf-4da4-b9f9-0ecb2b22386e)
  ![virtual hard drive](https://github.com/user-attachments/assets/a1d6aa95-ed01-4035-b605-b11e53c6c3b7)
  - Контроллер IDE изначально был пустым.
  ![Controlador IDE vacio](https://github.com/user-attachments/assets/4145250d-c68f-434c-8204-4e672d7d3e45)
  - Добавлен ISO-образ `ubuntu-24.04.1-desktop-amd64.iso` для установки Ubuntu:
  ![Controlador IDE con Ubuntu](https://github.com/user-attachments/assets/06ff3d13-b66e-466e-804d-aaf9ed187577)
      - Этот образ позволяет загрузить операционную систему в ВМ.  

### 3. Установка Ubuntu/Linux на ВМ-A
  - ВМ-A была запущена, и отображено приветственное окно "Welcome to Ubuntu".
  ![Instalación de ubuntu el la VM-A](https://github.com/user-attachments/assets/60dddeda-346a-40ab-ab84-ebb403bc0cee)
  - Создана учетная запись пользователя.
  ![Creando la cuenta](https://github.com/user-attachments/assets/502fe4f7-7c81-419c-b90b-ce55d22130fe)
  - Установка завершена, ВМ успешно перезагружена.
  ![Ubuntu instalado](https://github.com/user-attachments/assets/08a1f121-bf61-4801-bbcd-e1f36ebd5f7f)  
 
### 4. Проверка подключения к сети на ВМ-A
  - Для проверки сети использована команда:
    ```bash
    ping -c 4 google.com
    ```
  ![Testing internet conection VM A](https://github.com/user-attachments/assets/74bf6534-2c8a-4bc0-a31b-15dbe5434a54)
    - Успешный результат подтвердил доступ ВМ-A к Интернету.
  

### 5. Создание ВМ-B, установка Ubuntu и тестирование подключения
  - Параметры ВМ-B:
     - 3 ГБ оперативной памяти.
     - 2 процессора (CPU).
     - 10 ГБ виртуального жесткого диска.
  - Установка Ubuntu выполнена аналогично ВМ-A.
  ![VM-B instalando ubuntu](https://github.com/user-attachments/assets/0c24422d-e179-426d-bbb1-43b1874cb6c0)
  - Проверка подключения выполнена командой:
    ```bash
    ping -c 4 google.com
    ```
  ![Testing internet conection VM B](https://github.com/user-attachments/assets/643978d3-d1c5-4302-8524-d18543e1d886)
    - Подключение успешно.  

### 6. Ошибка при попытке соединения ВМ-A и ВМ-B
  - Настроена внутренняя сеть (Internal Network) с именем `intnet` для обеих ВМ.
  ![Internal Network for VM A and B](https://github.com/user-attachments/assets/49ec9032-32bc-4420-ada9-d19ec3446425)
  - После изменения настройки сети возникла ошибка подключения:
  ![Fail on ping from VM-A to VM-B](https://github.com/user-attachments/assets/518594e9-9fbe-4580-8fa5-f5e97b2b6768)
     - Это произошло из-за отсутствия DHCP-сервера, который распределяет IP-адреса в Internal Network.
     - Кроме того, Internal Network изолирует ВМ от доступа в Интернет.  
    
### 7. Создание NatNetwork
  - Создана сеть типа NatNetwork с именем `Intinter` и IPv4-префиксом `192.168.100.0/24`:
  ![New NATNetwork](https://github.com/user-attachments/assets/c0cfb80c-2af6-4427-b899-4907702b46c7)
     - NatNetwork позволяет автоматическое назначение IP-адресов и подключение к Интернету.
  - Обе ВМ подключены к сети NatNetwork, после чего они были перезагружены.
  - Командой `ip a` проверены IP-адреса:
  ![ip a](https://github.com/user-attachments/assets/970a6017-5dd6-4681-8a1a-a66ec2db398d)
     - ВМ-A: `192.168.100.4`
     - ВМ-B: `192.168.100.5`
  - Подключение внутри сети протестировано:
    ```bash
    ping -c 4 192.168.100.4 
    ping -c 4 192.168.100.5 
    ```
    - Для ВМ-А:
      - Прверка соединения:
      ![ping VM A - A](https://github.com/user-attachments/assets/ee6025be-0aeb-41ae-808a-03343d119d15)
      - Проверка соединения с ВМ-В:
      ![ping VM A - B](https://github.com/user-attachments/assets/c510518f-abab-427e-8373-7dac4119c107)  
    - Для ВМ-В:
      - Прверка соединения:
      ![ping VM B - B](https://github.com/user-attachments/assets/f6b846f1-bc20-4477-9953-6d026fb2e6aa)
      - Проверка соединения с ВМ-А:
      ![ping VM B - A](https://github.com/user-attachments/assets/1ff28145-5e27-4ef7-afb8-f11754b4fe94)
  
### 8. Создание и настройка ВМ-C
  - ВМ-C создана и настроена аналогично ВМ-A и ВМ-B.
  - Подключена к сети NatNetwork.
  - Проверен IP-адрес коммандой `ip a` (192.168.100.6) и доступ в Интернет с помощью команд:
    ```bash
    ping -c 4 google.com
    ping -c 4 192.168.100.6
    ```
  ![probando internet conecction for VM C](https://github.com/user-attachments/assets/76888cd3-6a57-4061-bf6c-f09f130abdda)  
  - Проверено соединение с ВМ-А.
  ![ping VM C - A](https://github.com/user-attachments/assets/3fcf3139-71ed-4b41-be96-c787dbd306c0)  

### 9. Блокировка связи ВМ-B и ВМ-C
  - Добавлено правило для блокировки связи ВМ-B и ВМ-C:
    ```bash
    sudo ufw deny from 192.168.100.5 to 192.168.100.6
    ```
  ![sudo ufw deny for VM C](https://github.com/user-attachments/assets/253d8ae7-b4fe-4528-b7cc-7805de07eb0e)
  - Проверено состояние файервола:
    ```bash
    sudo ufw status verbose
    ```
  ![firewall activado y con la restriccion](https://github.com/user-attachments/assets/fcc34802-575e-47b8-a739-b02521e53686)
     - Файервол был активирован, и правило ограничения отображалось корректно.
  - Добавлено дополнительное правило для исходящих запросов в ВМ-В:
    ```bash
    sudo ufw deny out to 192.168.100.6
    ```
  - Проверка c BM-B:
    ```bash
    ping -c 4 192.168.100.6
    ```
  ![tratando ping vm C desde vm B](https://github.com/user-attachments/assets/48be5688-8dac-4732-ab30-31e7b1fcfab0)
     - Результат: 100% packet loss, как и ожидалось.

### 10. Все 3 ВМ с их соответствующими подключениями
  ![3 maquinas virtuales mostrando sus conecciones](https://github.com/user-attachments/assets/e762e893-1c94-4416-98a2-c54edb9f12e5)
  - ВМ-А имеет доступ к ВМ-В и ВМ-С.
  - ВМ-В имеет доступ к ВМ-А, но не к ВМ-С.
  - ВМ-С имеет доступ к ВМ-А и ВМ-В.

### 11. Создание репозитория, каталога Report и добавление README.md
  - С использованием терминала Ubuntu создан репозиторий, клонирован для локальной работы, а также создан каталог Report:
    ```bash
    mkdir Report
    cd Report
    touch README.md
    ```
  ![Creando repositorio, mkdir y readme](https://github.com/user-attachments/assets/a2882441-08b3-4ef3-920c-269ef9e318fd)
  - Загружено содержимое репозитория на GitHub с использованием стандартных команд Git:
    ```bash
    git add 
    git commit
    git push 
    ```
  ![git add commit push for Report](https://github.com/user-attachments/assets/ac8c69e3-a567-451d-9a3d-cc6ae290fee6)
  - Проверка наличия репозитория и Report в github.
  ![Report in github](https://github.com/user-attachments/assets/389510e9-6239-40f1-94a8-4f858aad1bc0)
     
## Заключение 
В ходе выполнения лабораторной работы были настроены три виртуальные машины с установленной ОС Ubuntu. Выполнена настройка сети для взаимодействия между машинами и запрета доступа между ВМ-B и ВМ-C с использованием правил файервола. Работа успешно завершена, отчет оформлен и загружен на GitHub.
