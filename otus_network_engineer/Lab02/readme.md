
# Lab02

## Топология:
![](https://github.com/Etherne1/otus_network_engineer/blob/main/Lab02/Screenshot%202024-10-13%20194113.png?raw=true)

### Цели:
Часть 1. Создание сети и настройка основных параметров устройства  
Часть 2. Выбор корневого моста   
Часть 3. Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов   
Часть 4. Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов   


### Решение.

##### Предварительная настройка:
```
conf t
no ip domain-lookup
enable secret class
banner motd "unauthorized access is prohibited"
line vty 0 4
 login local
 password cisco
 line con 0 
 password cisco
 logging syn
service password-encryption
```

#### Проверяем наличие связности между свитчами:
  
```
S1>ping 192.168.1.2
Sending 5, 100-byte ICMP Echos to 192.168.1.2, timeout is 2 seconds:
..!!!
Success rate is 60 percent (3/5), round-trip min/avg/max = 0/0/0 ms 

S1>ping 192.168.1.3
Sending 5, 100-byte ICMP Echos to 192.168.1.3, timeout is 2 seconds:
..!!!
Success rate is 60 percent (3/5), round-trip min/avg/max = 0/0/0 ms
```

#### Далее меняем cost на S1 и наблюдаем за работой устройств:
```
S1(config)# interface f0/2

S1(config-if)# spanning-tree cost 18
```




Топология STP поменялась в соответствии с методичкой.

#### Затем возвращаем обратно:
```
S1(config)# interface f0/2

S1(config-if)# no spanning-tree cost 18
```
   

  У меня данная команда в packet tracer никакого эффекта не дала.
```
S1#sh run | b 0/2

interface FastEthernet0/2

switchport mode trunk

spanning-tree cost 18

S1#conf t

Enter configuration commands, one per line. End with CNTL/Z.

S1(config)#int fa0/2

S1(config-if)#no spa co 18

S1(config-if)#do sh run | b 0/2

interface FastEthernet0/2

switchport mode trunk

spanning-tree cost 18

!
```

Поэтому выставил значение на дефолт, для корректного завершения заданий. 

```
S1(config)# interface f0/2

S1(config-if)# spanning-tree cost 19
```

Но из конфига строку вручную убрал, чтобы при проверке не отображалось ошибкой. Можно было убирать не вручную, а залить корректный конфиг, но разницы в процессе нет, команда все равно не отрабатывается.

[Конфиг по частям 1-2.](https://github.com/Etherne1/otus_network_engineer/tree/main/Lab02/config%20before%20part%203)   
[Конфиг после выполнения части 3.](https://github.com/Etherne1/otus_network_engineer/tree/main/Lab02/config%20after%20part%203)   
[Файл лабораторной работы, с заполненными ответами.](https://github.com/Etherne1/otus_network_engineer/blob/main/Lab02/3.1.2.12_Lab___Building_a_Switched_Network_with_Redundant_Links.docx)
