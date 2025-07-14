# iSCSI

Краткий справочник

Я не очень люблю цитировать легконаходимые маны и строчки, так что приведу типовой сценарий употребения iscsi:

сначала мы находим нужные нам target, для этого мы должны знать IP/dns-имя инициатора: iscsiadm -m discovery -t st -p 192.168.0.1 -t st — это команда send targets.
```bash
iscsiadm -m node (список найденного для логина)
iscsiadm -m node -l -T iqn.2011-09.example:data (залогиниться, то есть подключиться и создать блочное устройство).
iscsiadm -m session (вывести список того, к чему подключились)
iscsiadm -m session -P3 (вывести его же, но подробнее — в самом конце вывода будет указание на то, какое блочное устройство какому target'у принадлежит).
iscsiadm - m session -u -T iqn.2011-09.example:data (вылогиниться из конкретной )
iscsiadm -m node -l (залогиниться во все обнаруженные target'ы)
iscsiadm -m node -u (вылогиниться из всех target'ов)
iscsiadm -m node --op delete -T iqn.2011-09.example:data (удалить target из обнаруженных).
```
