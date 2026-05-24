
Топология:

![](https://github.com/Etherne1/otus_network_engineer/blob/main/Pasted%20image%2020241010212925.png?raw=true)

### Задание:


1. Build the Network and Configure Basic Device Settings
2. Create VLANs and Assign Switch Ports Part 
3. Configure an 802.1Q Trunk between the Switches 
4. Configure Inter-VLAN Routing on the Router Part 
5. Verify Inter-VLAN Routing is working

### Решение:
1. Выполняем коммутацию в соответствии с топологией
2. Выполняем преднастройку по заданию
3. Создаем сабъинтерфейсы на роутере
4. Настраиваем  адресацию в соответствии с заданием
5. Создаем VLAN на всех свитчах в соответствии с таблицей:

| VLAN |Name | Interface | Assigned|
|--:|:--|--:|:--|
|3 | Management |S1: |VLAN 3 S2: VLAN 3 S1: F0/6 
|4 | Operations |S2:| F0/18 
|7 | ParkingLot |S1:| F0/2-4, F0/7-24, G0/1-2 S2: F0/2-17, F0/19-24, G0/1-2 
|8 | Native |N/A
 5. Тушим неиспользуемые порты на свитчах
 6. Настраиваем trunk между свитчами и между свитчом и роутером.
 7. Проверяем пинг и трассировку от PC-B до PC-A, вывод ниже:
```
 C:\>tracert 192.168.3.3

Tracing route to 192.168.3.3 over a maximum of 30 hops: 

  1   0 ms      0 ms      0 ms      192.168.4.1
  2   0 ms      0 ms      0 ms      192.168.3.3

Trace complete.
```

[Конфигурая оборудования прилагается](https://github.com/Etherne1/otus_network_engineer/tree/main/Lab01/configs)